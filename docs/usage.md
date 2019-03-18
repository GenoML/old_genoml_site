---
id: usage
title: Usage
---

## Train the ML model
This step performs data pruning as well as model training and tunning.

**Example data:** You can use the IPDGC (International Parkinson's Disease Genomics Consortium) example data, available at: https://github.com/ipdgc/GenoML-Brief-Intro/raw/master/exampleData.zip

### Basic 
Requires `genotype` and `phenotype` data, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno --model-file=./exampleModel
~~~~

### With external GWAS summary stat
Using `genotype`, `phenotype` , and `GWAS` data, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno  --gwas-file=./exampleData/example_GWAS.txt --model-file=./exampleModel
~~~~

### Adding convariates and additional predictor files
Using `genotype`, `phenotype` , `GWAS`, `covariate`, and `additional` data, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno --gwas-file=./exampleData/example_GWAS.txt --cov-file=./exampleData/training.cov --addit-file=./exampleData/training.addit --model-file=./exampleModel 
~~~~

### With heritability estimate
**Note:** heritability estimate increases the runtime significaintly and is not recommended (will be deprecated in future versions).

Using `genotype`, `phenotype` , `GWAS`, and `additional` data, as well as `heritability estimate`, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno  --gwas-file=./exampleData/example_GWAS.txt --addit-file=./exampleData/training.addit --herit=0.2 --model-file=./exampleModel 
~~~~

## Using the trained ML model for inference
To perform inference or external validation only when `genotype` and `phenotype` data present, run:
~~~~
genoml-inference --model-file=./exampleModel --valid-geno-prefix=./exampleData/validation --valid-pheno-file=./exampleData/validation.pheno
~~~~

## INPUT DATA (see example data)
##### Here is a quick walk through of input file formats and general suggestions. Please see examples in /exampleData.
#### Genotypes (mandatory file) 
Generally these should be single nucleotide polymorphisms and samples passing standard GWAS type quality controls.
If using imputed data, we suggest generally filtering at an imputation quality > 0.8 and using the hard call genotypes.
Additionally, rarer variants can be problematic, and we suggest filtering at a minor allele frequency > 1% (or > 5%) if sample size permits.
Limiting missingness in these files is helpful even though there is secondary imputation as part of genoML.
This is the standard .bed, .bim and .fam binaries from [PLINK](https://www.cog-genomics.org/plink/1.9/input#bed).  

#### Phenotypes (mandatory file)
A basic three column white-space delimited file as per PLINK specifications with the column headers FID, IID and PHENO and correspond to samples in the genotype data.  
In general, there should be no missing data.  Please code discrete phenotypes as 1/2, with 2 as a case and 1 as a control. Continuos phenotypes should be relatively normally distributed if possible.
File suffix should be ".pheno".

#### Covariates (optional file)
White-space delimited file with the first header columns FID and IID, the following columns can contain any numeric data deemed necessary.  
In general we use this for principal components to adjust for population substructure at the variant selection phase.
File suffix should be ".cov".

#### Additional (optional file)
White-space delimited file with the first header columns FID and IID, the following columns can contain any numeric data deemed necessary.  
In general we use this for parameters we want to use as predictors that aren't SNPs.  For example, this could include clinical data or thousands of gene expression probes, whatever you want.
File suffix should be ".addit".  

#### GWAS (optional file)
This is a big white-space delimited text file in the GCTA summary stats format.
This file must have header as follows, *SNP A1 A2 freq b se p N*, where *SNP* is a unique variant ID, *A1* is the effect allele, *A2* is the reference allele, *freq* is the frequency of A1, *b* is the beta coefficient from GWAS, *se* is the standard error from GWAS, *p* is the p-value from GWAS and *N* is the sample size. No missing data is allowed.

# BASIC WORKFLOW (AND NOTES ON OUTPUTS)
![WORKFLOW](https://github.com/GenoML/genoml-core/blob/master/docs/workflowDiagram_Sept19th2018.png)
#### Step 1: Prunes and extracts SNPs, merges input files and preps data for analysis 
##### In this phase you build your dataset for machine learning to begin.
In the autoMl workflow, different input data will trigger different SNPpruning workflows. In many scenarios we recommend PRSice as the option for pre-filtering variants. This method does a great job of incorporating external GWAS data and covariates as well (in case you think the covariates are important in SNP selection). If you specify external GWAS weights without also specifying heritability, PRSice will run. GCTA-sBLUP is also a solid option for SNP filtering incorporating external GWAS data but it cannot at this time incorporate covariates. GCTA-sBLUP will be implemented if you specify a heritability estiamte. Both methods allow for the highly recommended variant weighting options (using external GWAS summary statistics to weight variant allele dosages). If you have no covariates and no external GWAS summary statistics, default variant filtering in PLINK via LD pruning is the only option available. Pre-filtering of variants is mandatory as correlated predictors can really bias results for some algorithms and lead to overfitting.  

At this phase, polygenic risk scores are also calculated using linear models and summary stats for these are exported (files with the suffix ".PRS_summary.txt").  These summary stats are either for continuous traits as a summary(lm()) in R or the AUC calculated using predicted probabilities, probabilities dummied at 0.50 for the prediction and probabilities dummied at the top left threshold from the initial ROC curve. A file with the suffix "confMatAtBestThresh_PRS.txt" is also exported, which is a confusion matrix for the PRS predictions using the optimized "best" cut-off for delineating cases and controls based on the reciever operator curve. 

#### Step 2: Train model and initial tune via grid search 
##### Now that your dataset is built, train a bunch of different models and pick the best performing.
Now that the data is reduced, start testing algorithms and building models.

We generally recommend either the BOOSTED or ALL options here for the best balance of completeness and performance. Also, we recommend a minimum grid search interval of 30 and at least 10 iterations of cross validation. The current default is BOOSTED.

Early on in this phase of analysis, we export files with the suffixes ".nzv" and ".highCor".  If you plan to rerun and/or tweak options, it may be good to exclude these parameters from your data in future analyses if possible as these are either near-zero variance parameters or highly correlated parameters that can lead to overfitting.

Here all algorithms compete to perform best across cross validation.  The model with the highest mean R2 for continuous traits or highest mean AUC for discrete traits across all cross-validation iterations will be selected. You can view the runtimes and performance of each algorithm at this cross-validation and tuning phase in the files with suffixes "methodTimings.tab" and "methodPerformance.tab".  

The best model will also be exported as an Rdata object with the suffix "bestModel.Rdata".  A variable importance matrix showing the relative contribution to predictions of each parameters from the "dataForML" file will be exported with the suffix "varImp.tab". Predictions from the best model in the training set will be exported in the file with the suffix "trainingSetPredicitions.tab", these predictions will be overfit since it is the training dataset ... the same applies to the contents of the file with the suffix "training_summary.txt" which mirrors the "PRS_sumamry.txt" file from the previous step. Additionally, PNG files containing density, ROC and/or regression plots for your best performing model will be output at this point. A standard confusion matrix (suffix "confMat.txt") with additional summary statistics will also be exported, although at this time, this is based off a simple dummying of predictions at a probability of case status at 0.50 for discrete phenotypes. A file with the suffix "confMatAtBestThresh_training.txt" is also exported, which is a confusion matrix for the training set predictions using the optimized "best" cut-off for delineating cases and controls based on the reciever operator curve.

#### Step 3: Secondary tuning via extended grid search and maximization of hyperparameters 
##### This phase is a simple but sometimes time consuming extended tune of the best model from step 2.
If you think a longer tune is the way to go, please feel free to run an extended tune on your best model.
Exported files are similar to step 2 but have the additional "tuned" flag in the suffix.
This is part of the default pipeline run in ```genoml-train``` and is highly reccomended.

#### Step 4: External validation in test data 
##### Fit your best / tuned model from steps 2 and / or 3 to an external dataset and see how it really performs.
This phase is pretty straight forward, test your model in an external dataset. This could be a separate study or a withheld subset of the original dataset, but a separate study is always the best. A file with the suffix "confMatAtBestThresh_validation.txt" is also exported, which is a confusion matrix for the validation predictions using the optimized "best" cut-off for delineating cases and controls based on the reciever operator curve.

The only thing to make sure is that all parameters of interest exist in both datasets (training and validation).  In addition, these should be on the same numeric scale.

## Additional details
##### Some of whats going on under the hood of genoML
#### PRSice command
~~~~
Rscript $pathToGenoML/otherPackages/PRSice.R --binary-target T --prsice $pathToGenoML/otherPackages/PRSice_linux -n $cores --out $prefix.temp --pheno-file $pheno.pheno --cov-file $cov.cov -t $geno -b $gwas --print-snp --score std --perm 10000 --bar-levels 5E-8,4E-8,3E-8,2E-8,1E-8,9E-7,8E-7,7E-7,6E-7,5E-7,4E-7,3E-7,2E-7,1E-7,9E-6,8E-6,7E-6,6E-6,5E-6,4E-6,3E-6,2E-6,1E-6,9E-5,8E-5,7E-5,6E-5,5E-5,4E-5,3E-5,2E-5,1E-5,9E-4,8E-4,7E-4,6E-4,5E-4,4E-4,3E-4,2E-4,1E-4,9E-3,8E-3,7E-3,6E-3,5E-3,4E-3,3E-3,2E-3,1E-3,9E-2,8E-2,7E-2,6E-2,5E-2 --no-full --fastscore --beta --snp SNP --A1 A1 --A2 A2 --stat b --se se --pvalue p  
~~~~
or for continuous outcomes  
~~~~
Rscript $pathToGenoML/otherPackages/PRSice.R --prsice $pathToGenoML/otherPackages/PRSice_linux -n $cores --out $prefix.temp --pheno-file $pheno.pheno --cov-file $cov.cov -t $geno -b $gwas --print-snp --score std --perm 10000 --bar-levels 5E-8,4E-8,3E-8,2E-8,1E-8,9E-7,8E-7,7E-7,6E-7,5E-7,4E-7,3E-7,2E-7,1E-7,9E-6,8E-6,7E-6,6E-6,5E-6,4E-6,3E-6,2E-6,1E-6,9E-5,8E-5,7E-5,6E-5,5E-5,4E-5,3E-5,2E-5,1E-5,9E-4,8E-4,7E-4,6E-4,5E-4,4E-4,3E-4,2E-4,1E-4,9E-3,8E-3,7E-3,6E-3,5E-3,4E-3,3E-3,2E-3,1E-3,9E-2,8E-2,7E-2,6E-2,5E-2 --no-full --fastscore --binary-target F --beta --snp SNP --A1 A1 --A2 A2 --stat b --se se --pvalue p  
~~~~
#### GCTA-sBLUP command
~~~~
nsnps=`wc -l < $geno.bim | awk '{print $1}'`
sbluplambda=$(echo "scale = 10; $nsnps*((1/$herit)-1)" | bc)
$pathToGenoML/otherPackages/gcta64 --bfile $geno.forSblup --cojo-file $gwas --cojo-sblup $sbluplambda --cojo-wind 10000 --thread-num $cores --out $prefix.temp
~~~~
#### Plink LD pruning command
~~~~
$pathToGenoML/otherPackages/plink --bfile $geno --indep-pairwise 10000 1 0.1 --out $prefix.temp
$pathToGenoML/otherPackages/plink --bfile $geno --extract $prefix.temp.prune.in --recode A --out $prefix.reduced_genos
~~~~

