---
id: cli
title: The Command Line Interface (CLI)
---

## At a glance
`genoml` lets them know you are running a workflow supported by GenoML and its growing community.  

Then let us know if you would like to work with a continuous or discrete outcome in your \*.pheno file with `genoml continuous` or `genoml discrete`.  

The current iteration of documentation supports the `supervised` learning workflow, the list of workflows incoming is scary!
After that you can add one of the subcommands including choice of `munge`, `train`, `tune`, or `test`, these are steps within the supervised workflow.  

If you are interested in harmonizing a test dataset for external validation of a previous model, use `genoml harmonize`.

The general structure of GenoML commands is `genoml` then `outcome` type (discrete or continuous for now), then `workflow` (supervised for now), then `subcommand` that is usually a step within a larger workflow.

Detailed command line options relating to published GenoML `workflows` and their `subcommands` below. 

## genoml - the root of all commands
~~~~
usage: genoml <command> [<args>]
   continuous      for processing continuous datatypes (ex: age at onset)
   discrete        for processing discrete datatypes (ex: case vs. control status)
   harmonize       for harmonizing incoming test datasets to use the same SNPs and reference alleles prior to munging, training, and testing

genoml

positional arguments:
  command     Subcommand to run
~~~~

## genoml harmonize
~~~~
usage: genoml harmonize [-h] --test_geno_prefix TEST_GENO_PREFIX
                        [--test_prefix TEST_PREFIX]
                        [--ref_model_prefix REF_MODEL_PREFIX]
                        --training_snps_alleles TRAINING_SNPS_ALLELES [-v]

optional arguments:
  -h, --help            show this help message and exit
  --test_geno_prefix TEST_GENO_PREFIX
                        Prefix of the genotypes for the test dataset in PLINK
                        binary format.
  --test_prefix TEST_PREFIX
                        Prefix for the dataset you would like to test against
                        your reference model. Remember, the model will not
                        function well if it does not include the same
                        features, and these features should be on the same
                        numeric scale, you can leave off the '.dataForML.h5'
                        suffix.
  --ref_model_prefix REF_MODEL_PREFIX
                        Prefix of your reference model file, you can leave off
                        the '.joblib' suffix.
  --training_snps_alleles TRAINING_SNPS_ALLELES
                        File to the SNPs and alleles file generated in the
                        training phase that we will use to compare.
  -v, --verbose         Verbose output.
~~~~
## genoml discrete supervised munge
~~~~
usage: genoml discrete supervised munge [-h] [--prefix PREFIX]
                                        [--impute {median,mean}] [--geno GENO]
                                        --pheno PHENO [--addit ADDIT]
                                        [--feature_selection FEATURE_SELECTION]
                                        [--gwas GWAS] [--p P] [--vif VIF]
                                        [--iter ITER]
                                        [--ref_cols_harmonize REF_COLS_HARMONIZE]
                                        [-v]

optional arguments:
  -h, --help            show this help message and exit
  --prefix PREFIX       Prefix for your output build.
  --impute {median,mean}
                        Imputation: (mean, median). Governs secondary
                        imputation and data transformation [default: median].
  --geno GENO           Genotype: (string file path). Path to PLINK format
                        genotype file, everything before the *.bed/bim/fam
                        [default: None].
  --pheno PHENO         Phenotype: (string file path). Path to CSV phenotype
                        file [default: lost].
  --addit ADDIT         Additional: (string file path). Path to CSV format
                        feature file [default: None].
  --feature_selection FEATURE_SELECTION
                        Run a quick tree-based feature selection routine prior
                        to anything else, here you input the integer number of
                        estimators needed, we suggest >= 50. The default of 0
                        will skip this functionality. This will also output a
                        reduced dataset for analyses in addition to feature
                        ranks. [default: 0]
  --gwas GWAS           GWAS summary stats: (string file path). Path to CSV
                        format external GWAS summary statistics containing at
                        least the columns SNP and P in the header [default:
                        nope].
  --p P                 P threshold for GWAS: (some value between 0-1). P
                        value to filter your SNP data on [default: 0.001].
  --vif VIF             Variance Inflation Factor (VIF): (integer). This is
                        the VIF threshold for pruning non-genotype features.
                        We recommend a value of 5-10. The default of 0 means
                        no VIF filtering will be done. [default: 0].
  --iter ITER           Iterator: (integer). How many iterations of VIF
                        pruning of features do you want to run. To save time
                        VIF is run in randomly assorted chunks of 1000
                        features per iteration. The default of 1 means only
                        one pass through the data. [default: 1].
  --ref_cols_harmonize REF_COLS_HARMONIZE
                        Are you now munging a test dataset following the
                        harmonize step? Here you input the path to the to the
                        *_refColsHarmonize_toKeep.txt file generated at that
                        step.
  -v, --verbose         Verbose output.
~~~~
## genoml continuous supervised munge
~~~~
usage: genoml continuous supervised munge [-h] [--prefix PREFIX]
                                          [--impute {median,mean}]
                                          [--geno GENO] --pheno PHENO
                                          [--addit ADDIT]
                                          [--feature_selection FEATURE_SELECTION]
                                          [--gwas GWAS] [--p P] [--vif VIF]
                                          [--iter ITER]
                                          [--ref_cols_harmonize REF_COLS_HARMONIZE]
                                          [-v]

optional arguments:
  -h, --help            show this help message and exit
  --prefix PREFIX       Prefix for your output build.
  --impute {median,mean}
                        Imputation: (mean, median). Governs secondary
                        imputation and data transformation [default: median].
  --geno GENO           Genotype: (string file path). Path to PLINK format
                        genotype file, everything before the *.bed/bim/fam
                        [default: None].
  --pheno PHENO         Phenotype: (string file path). Path to CSV phenotype
                        file [default: lost].
  --addit ADDIT         Additional: (string file path). Path to CSV format
                        feature file [default: None].
  --feature_selection FEATURE_SELECTION
                        Run a quick tree-based feature selection routine prior
                        to anything else, here you input the integer number of
                        estimators needed, we suggest >= 50. The default of 0
                        will skip this functionality. This will also output a
                        reduced dataset for analyses in addition to feature
                        ranks. [default: 0]
  --gwas GWAS           GWAS summary stats: (string file path). Path to CSV
                        format external GWAS summary statistics containing at
                        least the columns SNP and P in the header [default:
                        nope].
  --p P                 P threshold for GWAS: (some value between 0-1). P
                        value to filter your SNP data on [default: 0.001].
  --vif VIF             Variance Inflation Factor (VIF): (integer). This is
                        the VIF threshold for pruning non-genotype features.
                        We recommend a value of 5-10. The default of 0 means
                        no VIF filtering will be done. [default: 0].
  --iter ITER           Iterator: (integer). How many iterations of VIF
                        pruning of features do you want to run. To save time
                        VIF is run in randomly assorted chunks of 1000
                        features per iteration. The default of 1 means only
                        one pass through the data. [default: 1].
  --ref_cols_harmonize REF_COLS_HARMONIZE
                        Are you now munging a test dataset following the
                        harmonize step? Here you input the path to the to the
                        *_refColsHarmonize_toKeep.txt file generated at that
                        step.
  -v, --verbose         Verbose output.
~~~~
## genoml discrete supervised train
~~~~
usage: genoml discrete supervised train [-h] [--prefix PREFIX]
                                        [--metric_max {AUC,Balanced_Accuracy,Specificity,Sensitivity}]
                                        [--prob_hist PROB_HIST] [--auc AUC]
                                        [--matching_columns MATCHING_COLUMNS]
                                        [-v]

optional arguments:
  -h, --help            show this help message and exit
  --prefix PREFIX       Prefix for your output build.
  --metric_max {AUC,Balanced_Accuracy,Specificity,Sensitivity}
                        How do you want to determine which algorithm performed
                        the best? [default: AUC].
  --prob_hist PROB_HIST
  --auc AUC
  --matching_columns MATCHING_COLUMNS
                        This is the list of intersecting columns between
                        reference and testing datasets with the suffix
                        *_finalHarmonizedCols_toKeep.txt
  -v, --verbose         Verbose output.
~~~~
## genoml continuous supervised train
~~~~
usage: genoml continuous supervised train [-h] [--prefix PREFIX]
                                          [--export_predictions EXPORT_PREDICTIONS]
                                          [--matching_columns MATCHING_COLUMNS]
                                          [-v]

optional arguments:
  -h, --help            show this help message and exit
  --prefix PREFIX       Prefix for your output build.
  --export_predictions EXPORT_PREDICTIONS
  --matching_columns MATCHING_COLUMNS
                        This is the list of intersecting columns between
                        reference and testing datasets with the suffix
                        *_finalHarmonizedCols_toKeep.txt
  -v, --verbose         Verbose output.
~~~~
## genoml discrete supervised tune
~~~~
usage: genoml discrete supervised tune [-h] [--prefix PREFIX]
                                       [--metric_tune {AUC,Balanced_Accuracy}]
                                       [--max_tune MAX_TUNE] [--n_cv N_CV]
                                       [-v]

optional arguments:
  -h, --help            show this help message and exit
  --prefix PREFIX       Prefix for your output build.
  --metric_tune {AUC,Balanced_Accuracy}
                        Using what metric of the best algorithm do you want to
                        tune on? [default: AUC].
  --max_tune MAX_TUNE   Max number of tuning iterations: (integer likely
                        greater than 10). This governs the length of tuning
                        process, run speed and the maximum number of possible
                        combinations of tuning parameters [default: 50].
  --n_cv N_CV           Number of cross validations: (integer likely greater
                        than 3). Here we set the number of cross-validation
                        runs for the algorithms [default: 5].
  -v, --verbose         Verbose output.
~~~~
## genoml continuous supervised tune
~~~~
usage: genoml continuous supervised tune [-h] [--prefix PREFIX]
                                         [--max_tune MAX_TUNE] [--n_cv N_CV]
                                         [-v]

optional arguments:
  -h, --help           show this help message and exit
  --prefix PREFIX      Prefix for your output build.
  --max_tune MAX_TUNE  Max number of tuning iterations: (integer likely
                       greater than 10). This governs the length of tuning
                       process, run speed and the maximum number of possible
                       combinations of tuning parameters [default: 50].
  --n_cv N_CV          Number of cross validations: (integer likely greater
                       than 3). Here we set the number of cross-validation
                       runs for the algorithms [default: 5].
  -v, --verbose        Verbose output.
~~~~
## genoml discrete supervised test
~~~~
usage: genoml discrete supervised test [-h] [--prefix PREFIX]
                                       [--test_prefix TEST_PREFIX]
                                       [--ref_model_prefix REF_MODEL_PREFIX]
                                       [-v]

optional arguments:
  -h, --help            show this help message and exit
  --prefix PREFIX       Prefix for your output build.
  --test_prefix TEST_PREFIX
                        Prefix for the dataset you would like to test against
                        your reference model. Remember, the model will not
                        function well if it does not include the same
                        features, and these features should be on the same
                        numeric scale, you can leave off the '.dataForML.h5'
                        suffix.
  --ref_model_prefix REF_MODEL_PREFIX
                        Prefix of your reference model file, you can leave off
                        the '.joblib' suffix.
  -v, --verbose         Verbose output.
~~~~
## genoml continuous supervised test
~~~~
usage: genoml continuous supervised test [-h] [--prefix PREFIX]
                                         [--test_prefix TEST_PREFIX]
                                         [--ref_model_prefix REF_MODEL_PREFIX]
                                         [-v]

optional arguments:
  -h, --help            show this help message and exit
  --prefix PREFIX       Prefix for your output build.
  --test_prefix TEST_PREFIX
                        Prefix for the dataset you would like to test against
                        your reference model. Remember, the model will not
                        function well if it does not include the same
                        features, and these features should be on the same
                        numeric scale, you can leave off the '.dataForML.h5'
                        suffix.
  --ref_model_prefix REF_MODEL_PREFIX
                        Prefix of your reference model file, you can leave off
                        the '.joblib' suffix.
  -v, --verbose         Verbose output.
~~~~
