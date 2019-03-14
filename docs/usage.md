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

### Adding convariance and additional files
Using `genotype`, `phenotype` , `GWAS`, `covariance`, and `additional` data, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno --gwas-file=./exampleData/example_GWAS.txt --cov-file=./exampleData/training.cov --addit-file=./exampleData/training.addit --model-file=./exampleModel 
~~~~

### With heritability estimate
**Note:** heritability estimate increases the runtime significaintly.

Using `genotype`, `phenotype` , `GWAS`, and `additional` data, as well as `Heritability estimate`, run:
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno  --gwas-file=./exampleData/example_GWAS.txt --addit-file=./exampleData/training.addit --herit=0.2 --model-file=./exampleModel 
~~~~

## Using the trained ML model for inference
To perform inference or external validation only when `genotype` and `phenotype` data present, run:
~~~~
genoml-inference --model-file=./exampleModel --valid-geno-prefix=./exampleData/validation --valid-pheno-file=./exampleData/validation.pheno
~~~~
