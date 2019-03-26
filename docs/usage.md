---
id: usage
title: Usage
---
## Input data
Here is a quick walk through of input file formats and general suggestions. 

> Example data: You can use the IPDGC (International Parkinson's Disease Genomics Consortium) example data, available [here](https://github.com/ipdgc/GenoML-Brief-Intro/raw/master/exampleData.zip).

### Genotypes (required) 
Generally these should be single nucleotide polymorphisms and samples passing standard GWAS type quality controls.
If using imputed data, we suggest filtering at an imputation quality > 0.8 and using the hard call genotypes.
Additionally, rarer variants can be problematic, and we suggest filtering at a minor allele frequency > 1% (or > 5%) if sample size permits. The fewer samples you have, the less you should focus on variants that have lower minor allele frequencies.
Limiting missingness in these files is helpful even though there is secondary imputation as part of GenoML.
This input data is the standard .bed, .bim and .fam binaries from [PLINK](https://www.cog-genomics.org/plink/1.9/input#bed).  

### Phenotypes (required)
A basic three column tab delimited file as per PLINK specifications with the column headers FID, IID and PHENO corresponding to samples in the genotype data.  
In general, there should be no missing data.  Please code discrete phenotypes as 1/2, with 2 as a case and 1 as a control. Continuos phenotypes should be relatively normally distributed if possible.
File suffix should be ".pheno".

### Covariates (optional)
Tab delimited file with the first header columns FID and IID, the following columns can contain any numeric data deemed necessary.  
In general we use this for principal components to adjust for population substructure at the variant selection phase.
File suffix should be ".cov".

### Additional (optional)
Tab delimited file with the first header columns FID and IID, the following columns can contain any numeric data deemed necessary.  
In general we use this for parameters we want to use as predictors that aren't SNPs.  For example, this could include clinical data or thousands of gene expression probes, whatever you want.
File suffix should be ".addit".  

### GWAS (optional)
This is a big tab delimited text file of genome-wide association study summary stats.
This file must have header as follows, *SNP A1 A2 freq b se p N*, where *SNP* is a unique variant ID, *A1* is the effect allele, *A2* is the reference allele, *freq* is the frequency of A1, *b* is the beta coefficient from GWAS, *se* is the standard error from GWAS, *p* is the p-value from GWAS and *N* is the sample size. No missing data is allowed.

## Train the ML model
This step performs data pruning as well as model training and tunning.

### Basic 
Requires `genotype` and `phenotype` data, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno --model-file=./exampleModel
~~~~

### With external GWAS summary stats
Using `genotype`, `phenotype` , and `GWAS` data, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno  --gwas-file=./exampleData/example_GWAS.txt --model-file=./exampleModel
~~~~

### Adding convariates and additional predictor files
Using `genotype`, `phenotype` , `GWAS`, `covariate`, and `additional` data, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno --gwas-file=./exampleData/example_GWAS.txt --cov-file=./exampleData/training.cov --addit-file=./exampleData/training.addit --model-file=./exampleModel 
~~~~

### Optional flags 
The following flags provide more flexibility:
| Flag           | Description                                                                                                                                                                                                            |
|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| --no-tune      | Disable Tuning. This marginally reduces the accuracy of the trained model but significately increases the training time.                                                                                                          |
| --n-cores=     | Number of cores to be allocated for computation [default: 1].                                                                                                                                                          |
| --train-speed= | Training speed: (ALL, FAST, FURIOUS, BOOSTED). Run all models, only,the fastest models, run slightly slower models, or just run boosted models which usually perform best when using genotype data [default: BOOSTED]. |
| --cv-reps=     | Number of cross-validation. An integer greater than 5. Effects the speed [default: 5].                                                                                                                                 |
| --impute-data= | Imputation: (knn, median). Governs secondary imputation and data transformation [default: median].                                                                                                                     |
| --grid-search= | Grid search length for parameters, integer greater than 10, 30 or greater recommended, effects speed of initial tune [default: 10].                                                                                    |

## Using the trained ML model for inference
To perform inference or external validation only when `genotype` and `phenotype` data present, run:
~~~~
genoml-inference --model-file=./exampleModel --valid-geno-prefix=./exampleData/validation --valid-pheno-file=./exampleData/validation.pheno
~~~~


