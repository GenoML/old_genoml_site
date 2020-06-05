---
id: usage
title: Usage
---
## Input data
Here is a quick walk through of input file formats and general suggestions. You always need a **phenotype** file and at least **genotype** or **"additional"** feature data. Have to have something to predict the phenotype, right?

### Phenotypes (required)
A basic two column CSV with **ID** and **PHENO** corresponding to samples in the genotype data.  
ID must be a string, i.e. composed of letters and possibly some combination of letters and numbers.
In general, there should be no missing data.  Please code discrete phenotypes as 0/1, with 1 as a case and 0 as a control. Continuous phenotypes should be relatively normally distributed if possible.
File suffix should be ".pheno".

### Genotypes (optional)
Generally these should be single nucleotide polymorphisms and samples passing standard GWAS type quality controls.
If using imputed data, we suggest filtering at an imputation quality > 0.8 and using the hard call genotypes.
Additionally, rarer variants can be problematic, and we suggest filtering at a minor allele frequency > 1% (or > 5%) if sample size permits. The fewer samples you have, the less you should focus on variants that have lower minor allele frequencies.
Limiting missingness in these files is helpful even though there is secondary imputation as part of GenoML.
This input data is the standard .bed, .bim and .fam binaries from [PLINK](https://www.cog-genomics.org/plink/1.9/input#bed).  
We are working on teaching GenoML to eat more file types, if you have some favorite formats for genotypes, please get in touch either by posting an issue on GitHub or via [twitter](https://twitter.com/geno_ml).

### Additional (optional)
Comma delimited file with the first header columns **ID**, the following columns can contain any numeric data deemed necessary.  
In general we use this for parameters we want to use as predictors that aren't SNPs.  For example, this could include clinical data or thousands of gene expression probes, whatever you want.  Please just make sure the file has headers that are strings (i.e. not just numbers).
File suffix should be ".addit".  

### GWAS (optional)
This is a big tab delimited text file of genome-wide association study summary stats.
This file must have header as follows, *SNP A1 A2 freq b se p N*, where *SNP* is a unique variant ID, *A1* is the effect allele, *A2* is the reference allele, *freq* is the frequency of A1, *b* is the beta coefficient from GWAS, *se* is the standard error from GWAS, *p* is the p-value from GWAS and *N* is the sample size. No missing data is allowed.

## Complete options for common workflows
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

## 
