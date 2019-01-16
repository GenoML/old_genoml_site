---
id: example1
title: Example 1
---

## Step-by-step examples 
Please refer to the following quick examples for running GonML (for full `usage`, please refer to [Usage](#usage)):

### Step 1 - genoml data-prune:
To perform `data-prune` only on `genotype` and `pehnotype` data:

    python genoml.py data-prune --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno 

To perform `data-prune`  on `genotype`, `pehnotype` , `GWAS`, and `covariance` data:
 
    python genoml.py data-prune --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno  --gwas-file=./exampleData/example_GWAS.txt  

To perform `data-prune`  on `genotype`, `pehnotype` , `GWAS`, `covariance`, and `additional` data:

    python genoml.py data-prune --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno --cov-file=./exampleData/training.cov --gwas-file=./exampleData/example_GWAS.txt --addit-file=./exampleData/training.addit  

To perform `data-prune`  on `genotype`, `pehnotype` , `GWAS`, and `additional` data, as well as `Heritability estimate`:

    python genoml.py data-prune --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno  --gwas-file=./exampleData/example_GWAS.txt --addit-file=./exampleData/training.addit --herit=0.2  

To perform `data-prune`  on `genotype`, `pehnotype` , `GWAS`, `covariance`, and `additional` data, as well as `Heritability estimate`:

    python genoml.py data-prune --geno-prefix=./exampleData/training --pheno-file=./exampleData/example.pheno --cov-file=./exampleData/training.cov --gwas-file=./exampleData/example_GWAS.txt --addit-file=./exampleData/training.addit --herit=0.5 

### Step 2 - genoml model-train:
To perform `model-train`  on the output of `data-prune` with the prefix given to you from the prune step `prune-prefix=./tmp/20181225-230052`:
 
    python genoml.py model-train --prune-prefix=./tmp/20181225-230052 --pheno-file=./exampleData/training.pheno  

### Step 3 - genoml model-tune:
To perform `model-tune`  after `model-train`  on the output of `data-prune` with the prefix given to you from the prune step `prune-prefix=./tmp/20181225-230052`:

    python genoml.py model-tune --prune-prefix=./tmp/20181225-230052 --pheno-file=./exampleData/training.pheno

### Step 4 - genoml model-validation:
To perform external `model-validate`:

    python genoml.py model-validate --prune-prefix=./tmp/20181225-230052 --pheno-file=./exampleData/training.pheno --valid-geno-prefix=./exampleData/validation --valid-pheno-file=./exampleData/validation.pheno
 
test1
