---
id: cli
title: The Command Line Interface (CLI)
---

Detailed command line options for `genoml-train`, `genoml-inference`, and `genoml-cli`. 

> Please note `genoml-cli` is only recommended for debugging and advanced users. We plan to use the `genoml-cli` for internal unit testing and therefore they are in a constant state of flux / improvement. Most useres should stick to ```genoml-train``` and ```genoml-inference```.

## genoml-train
~~~~
genoml-train

Usage:
  genoml-train (--geno-prefix=geno_prefix) (--pheno-file=<pheno_file>) (--model-file=model_file) [--addit-file=<addit_file>] [--cov-file=<cov_file>] [--cv-reps=<cv_reps>] [--grid-search=<grid_search>] [--gwas-file=<gwas_file>] [--herit=<herit>] [--impute-data=<impute_data>] [--n-cores=<n_cores>] [--train-speed=<train_speed>] [--no-tune] [-v | -vv | -vvv]
  genoml-train -h | --help
  genoml-train --version

Options:
  --geno-prefix=geno_prefix               Prefix with path to genotype files in PLINK format, *.bed, *.bim and *.fam.
  --pheno-file=<pheno_file>               Path to the phenotype file in PLINK format, *.pheno.
  --model-file=model_file                 Path to the model file to save.
  --gwas-file=<gwas_file>                 Path to the GWAS file, if available.
  --cov-file=<cov_file>                   Path to the covariance file, if available.
  --herit=<herit>                         Heritability estimate of phenotype between 0 and 1, if available.
  --addit-file=<addit_file>               Path to the additional file, if avialable.
  --n-cores=<n_cores>                     Number of cores to be allocated for computation [default: 1].
  --train-speed=<train_speed>             Training speed: (ALL, FAST, FURIOUS, BOOSTED). Run all models, only  the fastest models, run slightly slower models, or just run boosted models which usually perform best when using genotype data [default: BOOSTED].
  --cv-reps=<cv_reps>                     Number of cross-validation. An integer greater than 5. Effects the speed [default: 5].
  --impute-data=<impute_data>             Imputation: (knn, median). Governs secondary imputation and data transformation [default: median].
  --grid-search=<grid_search>             Grid search length for parameters, integer greater than 10, 30 or greater recommended, effects speed of initial tune [default: 10].
  --no-tune                               Disable Tuning. This reduces the accuracy of the trained model but significately increases the training time.
  -v -vv -vvv                             Verbose output.
  -h --help                               Show this screen.
  --version                               Show version.

Examples:
  genoml-train --model-file=model.genoml_model --geno-prefix=./exampleData/example --pheno-file=./exampleData/training.pheno --gwas-file=./exampleData/example_GWAS.txt
~~~~

## genoml-inference
~~~~
genoml-inference

Usage:
  genoml-inference (--model-file=model_file) (--valid-dir=valid_dir) (--valid-geno-prefix=valid_geno_prefix) (--valid-pheno-file=<valid_pheno_file>) [--valid-cov-file=<valid_cov_file>] [--valid-addit-file=<valid_addit_file>] [--n-cores=<n_cores>] [-v | -vv | -vvv]
  genoml-inference -h | --help
  genoml-inference --version

Options:
  --model-file=model_file                 Path to the model file.
  --valid-dir=valid_dir                   Folder to save output.
  --n-cores=<n_cores>                     Number of cores to be allocated for computation [default: 1].
  --valid-geno-prefix=valid_geno_prefix   Prefix with path to the validation genotype files in PLINK format, *.bed, *.bim and *.fam.
  --valid-pheno-file=<valid_pheno_file>   Path to the validation phenotype file in PLINK format, *.pheno.
  --valid-cov-file=<valid_cov_file>       Path to the validation covariance file, if available.
  --valid-addit-file=<valid_addit_file>   Path to the the validation additional file, if avialable.
  -h --help                               Show this screen.
  -v -vv -vvv                             Verbose output.
  --version                               Show version.

Examples:
  genoml-inference --model-file=./model.genoml_model --valid-geno-prefix=./exampleData/validation --valid-pheno-file=./exampleData/validation.pheno
~~~~

## genoml-cli
~~~~
genoml-cli

Usage:
  genoml-cli data-prune  (--geno-prefix=geno_prefix) (--pheno-file=<pheno_file>) [--gwas-file=<gwas_file>] [--cov-file=<cov_file>] [--herit=<herit>] [--addit-file=<addit_file>] [--temp-dir=<directory>] [--prune-prefix=<prune_prefix>]
  genoml-cli model-train (--prune-prefix=prune_prefix) (--pheno-file=<pheno_file>) [--n-cores=<n_cores>] [--train-speed=<train_speed>] [--cv-reps=<cv_reps>] [--grid-search=<grid_search>] [--impute-data=<impute_data>]
  genoml-cli model-tune (--prune-prefix=prune_prefix) (--pheno-file=<pheno_file>) [--cv-reps=<cv_reps>] [--grid-search=<grid_search>] [--impute-data=<impute_data>] [--best-model-name=<best_model_name>]
  genoml-cli model-validate (--prune-prefix=prune_prefix) (--pheno-file=<pheno_file>) (--geno-prefix=geno_prefix) (--valid-geno-prefix=valid_geno_prefix) (--valid-pheno-file=<valid_pheno_file>) [--valid-cov-file=<valid_cov_file>] [--gwas-file=<gwas_file>] [--valid-addit-file=<valid_addit_file>] [--n-cores=<n_cores>] [--impute-data=<impute_data>]  [--best-model-name=<best_model_name>]
  genoml-cli -h | --help
  genoml-cli --version

Options:
  --geno-prefix=geno_prefix               Prefix with path to genotype files in PLINK format, *.bed, *.bim and *.fam.
  --pheno-file=<pheno_file>               Path to the phenotype file in PLINK format, *.pheno.
  --gwas-file=<gwas_file>                 Path to the GWAS file, if available.
  --cov-file=<cov_file>                   Path to the covariance file, if available.
  --herit=<herit>                         Heritability estimate of phenotype between 0 and 1, if available.
  --addit-file=<addit_file>               Path to the additional file, if avialable.
  --temp-dir=<directory>                  Directory for temporary files [default: ./tmp/].
  --n-cores=<n_cores>                     Number of cores to be allocated for computation [default: 1].
  --prune-prefix=prune_prefix             Prefix given to you at the end of pruning stage.
  --train-speed=<train_speed>             Training speed: (ALL, FAST, FURIOUS, BOOSTED). Run all models, only  the fastest models, run slightly slower models, or just run boosted models which usually perform best when using genotype data [default: BOOSTED].
  --cv-reps=<cv_reps>                     Number of cross-validation. An integer greater than 5. Effects the speed [default: 5].
  --impute-data=<impute_data>             Imputation: (knn, median). Governs secondary imputation and data transformation [default: median].
  --grid-search=<grid_search>             Grid search length for parameters, integer greater than 10, 30 or greater recommended, effects speed of initial tune [default: 10].
  --best-model-name=<best_model_name>     Name for the best model [default: best_model].
  --valid-geno-prefix=valid_geno_prefix   Prefix with path to the validation genotype files in PLINK format, *.bed, *.bim and *.fam.
  --valid-pheno-file=<valid_pheno_file>   Path to the validation phenotype file in PLINK format, *.pheno.
  --valid-cov-file=<valid_cov_file>       Path to the validation covariance file, if available.
  --valid-addit-file=<valid_addit_file>   Path to the the validation additional file, if avialable.
  -h --help                               Show this screen.
  --version                               Show version.

Examples:
  genoml-cli data-prune --geno-prefix=./exampleData/example --pheno-file=./exampleData/training.pheno
  genoml-cli data-prune --geno-prefix=./exampleData/example --pheno-file=./exampleData/training.pheno  --gwas-file=./exampleData/example_GWAS.txt
  genoml-cli data-prune --geno-prefix=./exampleData/example --pheno-file=./exampleData/training.pheno --cov-file=./exampleData/training.cov --gwas-file=./exampleData/example_GWAS.txt --addit-file=./exampleData/training.addit
  genoml-cli data-prune --geno-prefix=./exampleData/example --pheno-file=./exampleData/training.pheno  --gwas-file=./exampleData/example_GWAS.txt --addit-file=./exampleData/training.addit --herit=0.2
  genoml-cli data-prune --geno-prefix=./exampleData/example --pheno-file=./exampleData/training.pheno --cov-file=./exampleData/training.cov --gwas-file=./exampleData/example_GWAS.txt --addit-file=./exampleData/training.addit --herit=0.5
  genoml-cli model-train --prune-prefix=./tmp/20181225-230052 --pheno-file=./exampleData/training.pheno
  genoml-cli model-tune --prune-prefix=./tmp/20181225-230052 --pheno-file=./exampleData/training.pheno
  genoml-cli model-validate --prune-prefix=./tmp/20181225-230052 --pheno-file=./exampleData/training.pheno --valid-geno-prefix=./exampleData/validation --valid-pheno-file=./exampleData/validation.pheno
~~~~
