---
id: quick-start
title: Quick start
---

Here are some quick get started exmaples, please checkout the documents for full usage.

## Install 
~~~~
pip install genoml
~~~~

## Train the ML model 
~~~~
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno --model-file=./exampleModel
~~~~

You can use the IPDGC (International Parkinson's Disease Genomics Consortium) example data, avaiable from [here](https://github.com/ipdgc/GenoML-Brief-Intro/raw/master/exampleData.zip).
#### Note, the zip archive output by ```--model-file``` includes your tuned model plus performance metrics. 

## Using the trained ML model for inference
~~~~
genoml-inference --model-file=./exampleModel --valid-dir=./exampleData --valid-geno-prefix=./exampleData/validation --valid-pheno-file=./exampleData/validation.pheno
~~~~
#### Note, valdiation results and model performance metrics are included in the directory created by ```--valid-dir```
