---
id: quick-start
title: Quick start
---

Here are some quick "get started" exmaples, please checkout the additional options and details in the [usage][] and [CLI][].

> For debugging purposes, include the ```-v``` or ```-vvv``` flags at the end of a command.

## Install 
~~~~bash
pip install genoml
~~~~

## Train the ML model 
~~~~console
genoml-train --geno-prefix=./exampleData/training --pheno-file=./exampleData/training.pheno --model-file=./exampleModel
~~~~

You can use the IPDGC (International Parkinson's Disease Genomics Consortium) example data, avaiable at [IPDGC example data][].

> Final tuned model and performance metrics are stored in the ```--model-file``` as a zip file. 

## Using the trained ML model for inference
~~~~
genoml-inference --model-file=./exampleModel --valid-dir=./exampleData --valid-geno-prefix=./exampleData/validation --valid-pheno-file=./exampleData/validation.pheno
~~~~

> Valdiation results and model performance metrics are stored in the ```--valid-dir``` directory. 

[usage]: https://genoml.github.io/docs/usage
[CLI]: https://genoml.github.io/docs/cli
[IPDGC example data]: https://github.com/ipdgc/GenoML-Brief-Intro/raw/master/exampleData.zip
