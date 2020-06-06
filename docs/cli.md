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
