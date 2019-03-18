---
id: workflow
title: Workflow
---
## QUICK SUMMARY

**GenoML** is a package that enables users to easily run basic machine learning (ML) pipelines on genetic and genomic data.  The goal of this project is to democratize and simplify these sometimes complex analyses.  GenoML is machine learning made easier (for geneticists). Our development focus is automating the annoying and complicated aspects of most machine learning applications in genetics/genomics, from pre-filtering of SNPs to algorithm selection and validation testing.

We have included a command line binary for local computing but also a container for cloud applications (a web server is currently under development).

In its current form, you can use this package to:
1. Select features of interest from genetic data for predictions
2. Integrate clinical and genomic (QTL etc) data into these predictions
3. Train models using multiple predictive algorithms for discrete and continuous traits
4. Build and test external dataset(s) for validation of predicted models

The next phase of this project will include:
1. More efficient performance of the basic pipeline
2. Integration of federated and meta learning methods to facilitate secure predictive modeling without the sharing of underlying participant level data (similar to how phones have learned predictive texting)
3. A cloud-based web server to store, share and apply predictive models


GenoML synthesizes the amazing work of alot of other scientists to facilitate your analyses.  
These include:  
[R](https://www.r-project.org), [CARET](http://topepo.github.io/caret/index.html) and the [tidyverse](https://www.tidyverse.org) as well as [Python](https://www.python.org), [PANDAS](https://pandas.pydata.org) and [SciKit-learn](https://scikit-learn.org/stable/) to handle the general stats, data management, plotting and ML modeling.  
[PLINK](https://www.cog-genomics.org/plink2) for genotype management.  
[GCTA-sBLUP](https://cnsgenomics.com/software/gcta/#Overview) for variant pre-filtering.  
[PRSiceV2](https://choishingwan.github.io/PRSice/) for additional variant pre-filtering.  
[Docker](https://www.docker.com) for containerizing all this so it actually works consistently.

## Workflow
![WORKFLOW](https://github.com/GenoML/genoml-core/blob/master/docs/workflowDiagram_Sept19th2018.png)
### Step 1: Prunes and extracts SNPs, merges input files and preps data for analysis 
##### In this phase you build your dataset for machine learning to begin.
We generally recommend PRSice as the option for pre-filtering variants if you are in a hurry. This method does a great job of incorporating external GWAS data and covariates as well (in case you think the covariates are important in SNP selection). GCTA-sBLUP is also a solid option for SNP filtering incorporating external GWAS data but it cannot at this time incorporate covariates. Both methods allow for the highly recommended variant weighting options (using external GWAS summary statistics to weight variant allele dosages). If you have a large dataset and aren't in a rush, default LD pruning can be extremely useful (at the cost of longer run times since it is the least conservative filtering option in general). If you have no covariates and no external GWAS data, default variant filtering in PLINK via LD pruning is the only option available. Pre-filtering of variants is mandatory as correlated predictors can really bias results for some algorithms and lead to overfitting.  

At this phase, polygenic risk scores are also calculated using linear models and summary stats for these are exported (files with the suffix ".PRS_summary.txt") if you chose the PRSice filtering option.  These summary stats are either for continuous traits as a summary(lm()) in R or the AUC calculated using predicted probabilities, probabilities dummied at 0.50 for the prediction and probabilities dummied at the top left threshold from the initial ROC curve. A file with the suffix "confMatAtBestThresh_PRS.txt" is also exported, which is a confusion matrix for the PRS predictions using the optimized "best" cut-off for delineating cases and controls based on the reciever operator curve. 

The primary output of this phase of analysis is the file with the suffix ".dataForML". This is a file that will include data for all selected variants, the phenotype of interest, covariates and additional predictors. If the variant scaling option was used, variant allele dosages in this file will be scaled based on external GWAS data.
Logs and intermediate files for all instances of PLINK, PRSice and GCTA-sBLUP can also be found at this stage.

### Step 2: Train model and initial tune via grid search 
##### Now that your dataset is built, train a bunch of different models and pick the best performing.
Now that you have a ".dataForML" file, start testing algorithms and building models.

We generally recommend either the BOOSTED or ALL options here for the best balance of completeness and performance. Also, we recommend a minimum grid search interval of 30 and at least 10 iterations of cross validation.

Early on in this phase of analysis, we export files with the suffixes ".nzv" and ".highCor".  If you plan to rerun and/or tweak options, it may be good to exclude these parameters from your data in future analyses if possible as these are either near-zero variance parameters or highly correlated parameters that can lead to overfitting.

Here all algorithms compete to perform best across cross validation.  The model with the highest mean R2 for continuous traits or highest mean AUC for discrete traits across all cross-validation iterations will be selected. You can view the runtimes and performance of each algorithm at this cross-validation and tuning phase in the files with suffixes "methodTimings.tab" and "methodPerformance.tab".  

The best model will also be exported as an Rdata object with the suffix "bestModel.Rdata".  A variable importance matrix showing the relative contribution to predictions of each parameters from the "dataForML" file will be exported with the suffix "varImp.tab". Predictions from the best model in the training set will be exported in the file with the suffix "trainingSetPredicitions.tab", these predictions will be overfit since it is the training dataset ... the same applies to the contents of the file with the suffix "training_summary.txt" which mirrors the "PRS_sumamry.txt" file from the previous step. Additionally, PNG files containing density, ROC and/or regression plots for your best performing model will be output at this point. A standard confusion matrix (suffix "confMat.txt") with additional summary statistics will also be exported, although at this time, this is based off a simple dummying of predictions at a probability of case status at 0.50 for discrete phenotypes. A file with the suffix "confMatAtBestThresh_training.txt" is also exported, which is a confusion matrix for the training set predictions using the optimized "best" cut-off for delineating cases and controls based on the reciever operator curve.

### Step 3: Secondary tuning via extended grid search and maximization of hyperparameters 
##### This phase is a simple but sometimes time consuming extended tune of the best model from step 2.
If you think a longer tune is the way to go, please feel free to run an extended tune on your best model.
Exported files are similar to step 2 but have the additional "tuned" flag in the suffix.

### Step 4: External validation in test data 
##### Fit your best / tuned model from steps 2 and / or 3 to an external dataset and see how it really performs.
This phase is pretty straight forward, test your model in an external dataset. This could be a separate study or a withheld subset of the original dataset, but a separate study is always the best. A file with the suffix "confMatAtBestThresh_validation.txt" is also exported, which is a confusion matrix for the validation predictions using the optimized "best" cut-off for delineating cases and controls based on the reciever operator curve.

The only thing to make sure is that all parameters of interest exist in both datasets (training and validation).  For example, make sure you have the same variants availible in the genotype files as well as the same phenotypes in the phenotpye files across training and validation. If you used any ".addit" or ".cov" files, these should be a mirror of the training phase as well. In addition, these should be on the same numeric scale. Put simply, missing predictors from training will kill the validation.

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
