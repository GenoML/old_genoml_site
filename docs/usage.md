---
id: usage
title: Usage
---
## Input data
Here is a quick walk through of input file formats and general suggestions. You always need a **phenotype** file and at least **genotype** or **"additional"** feature data. Have to have something to predict the phenotype, right? **GenoML** = **geno**mcis + **m**achine **l**earning, so you get some genomics workflows plus a fully functional auto-ML.

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
This is a big comma delimited text file of genome-wide association study summary stats.
This file must have header as follows, *SNP A1 A2 freq b se p N*, where *SNP* is a unique variant ID, *A1* is the effect allele, *A2* is the reference allele, *freq* is the frequency of A1, *b* is the beta coefficient from GWAS, *se* is the standard error from GWAS, *p* is the p-value from GWAS and *N* is the sample size. No missing data is allowed.

## Now its time for analysis
... you should refer to the **workflows** section of this website for general concepts and helpful hints.  
If you want to get into the details of what can be done with GenoML in this release, please refer to the **API reference** for a full list of commands and options.  
**Remember**, if anything is broken, confusing of you just want new features, please hit us up on [twitter](https://twitter.com/geno_ml) or submit a ticket to our GitHub issues.
