---
id: workflows
title: Workflows
---
## QUICK SUMMARY

**GenoML** is a package that enables users to easily run basic machine learning (ML) pipelines on genetic and genomic data.  In the future, we are adding a lot of content for basic genomics pipelines as well, please see the **roadmap** or our [twitter](https://twitter.com/geno_ml) for more info. The goal of this project is to democratize and simplify these sometimes complex analyses.  GenoML is machine learning made easier (for geneticists in particular). Our development focus is automating the annoying and complicated aspects of most machine learning applications in genetics/genomics, from pre-filtering of SNPs to algorithm selection and validation testing.

The pip install is generally the way to go, although you could also download the source or container for cloud applications (a web server is currently under development as well).

While using GenoML, a narrative is always provided that describes the calculations and outputs of each step of the workflows, so we'll let the software do most of the talking for us. Below are some quick overviews and helpful hints for various workflows in the GenoML ecosystem (an ecosystem that is growing rapidly with the help of the genomics and ML communities). 

## SUPERVISED LEARNING
### Step 1: Merge and Munge your Data 
##### In this phase, you build and preprocess your dataset for machine learning to begin.

The primary output of this phase of analysis is the file with the suffix ".dataForML.h5". This is a pretty compact h5 archive that will be used as the basis of all future analysis. You can even edit the files inside if you want, but that is not recommended (unless you are pretty good with python).

In this phase, we merge and munge all the data so you can build models or use other pipelines we have in development like network community builds or a GWAS.

The basis of this command, what you can think of as the fork in the road regarding functionality is if you use a discrete or continuous outcome. We'll be adding multi-class and unlabeled functionality soon.
Generally you chose `genoml continuous supervised munge` or `genoml discrete supervised munge`.

A few key insights include:
- Use the feature selection option, this prevents overfitting of machine learning models
- Use only high-quality common SNPs for machine learning analyses if you plan to use genetic data
- Filtering based on the variance inflation factor (VIF) is useful, but not perfect, and can be time consuming for datasets with a lot of features
- Do not include features with > 15% missing data, we'll impute the missing but "you know the story"
- Only samples overlapping all input data files will be kept
- Only numeric data allowed outside of the **ID** columns, the **ID** column needs to be alphanumeric; some combination of letters and numbers 
- When using genetics data for a known disease, we tend to run a quick prune of imputed hard calls (> 0.8 imputation quality) and make sure all GWAS hits from disease(s) of interest are included... then do feature selection and further imputation / pruning within GenoML
- We run a pretty strict linkage disequilibrium (LD) prune for SNPs using an adaptation of PLINK v1.9's `--indep-pairwise 1000 50 0.1`

### Step 2: Train Your Model 
##### Now that your dataset is built, train a bunch of different models and pick the best performing.

Now that you have a dataset built, why not compete a dozen of the top machine learning algorithms against each other to see which one works best?  
This part may trigger some people, we include linear (continuous) and logistic (discrete) regression among our algorithms. These rarely win competitions, but work very well with few factors each with a large effect estimate.  
At this point, the auto-ML functionality of GenoML kicks in. Your dataset is split 70:30 between trained:evaluate. All algorithms are trained on 70% of the data, then evaluated quickly on the same withheld 30% of data.  
The algorithm that provides the best metrics for performance is then selected and the model itself plus various summaries are exported. Please see the narrative provided in log files for further details.

Helpful hits for model training include:
- if your sample sizes in discrete analyses are very far from 1:1 for cases:controls, might be better to use the balanced accuracy option as opposed to the AUC option for metric maximization, see the `--metric_max` option for further details, you can even use sensitivity or specificity if you want.
- `continuous` workflows only train on r2 values, sorry we are a little lazy with that one

### Step 3: Secondary tuning via extended grid search and maximization of hyperparameters 
##### You've just identified the likely best algorithm for your data, lets rebuild a model with a solid hyperparameter tune at cross validation.

Quick parametrized grid search to optimize model performance across multiple folds of withheld data. Pretty straight forward. During the run, check the logs as GenoML walks you through this process.

### Step 4: External Validation in Test Data 
##### The gold standard for performance testing.
For this separate dataset, you are taking the model produced in step 2 or step 3 and applying it.  
There is a great walk through of this in the **quickstart** section.

Some hints for successfully testing a model in an external validation dataset include:
- Check the logs for performance updates and workflow progress
- Make sure you run munge with all data harmonization options enabled, Dan worked really hard on these so make use of them please
- Continuous features and phenotypes all need to be on the same scale (numerically), so scaling is a good thing

## MORE WORKFLOWS AND OPTIONS COMING VERY SOON, CHECK THE ROADMAP FOR MORE!
