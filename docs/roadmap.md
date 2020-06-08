---
id: roadmap
title: Roadmap  
---

> Updated 06/03/20

**GenoML** v1 was a beta that lasted for a year, it had >600 unique users and let the team get alot of feedback from the community. It was an aggregation of code we have been using internally for quite some time, all of it coming from a variety of different projects and applications. Today, we've released a much imporved version of **GenoML** that will build a solid foundation for the rapid growth we are planning. We've set some goals below and a loose timeline. 

## Spring / Summer 2020 (GenoMLv2, right now, this exact second)
This development cycle includes:
1. Upping speed and efficiency of the pipeline
2. Transitioning to a 100% Python core
3. Integrating additional algorithm
4. Generally making the pipeline and docs more user friendly
5. Additional feature development 

## Summer / Fall 2020 (GenoMLv3)
This is when the exciting stuff really starts happening.

Everybody contributing to GenoML is really excited about the potential applications of **federated-learning (fed-ML)** and **meta-learning (meta-ML)** methods in genomics.

Particularly in light of recent privacy legislation changes, it would be great to be able to learn across multiple data silos.
We will use meta-ML to do algorithm competition and selection across data silos.
We will use fed-ML for centralized training/tuning based on aggregate summary statistics from private data silos.

GenoMLv3 will include fed-ML similar to what is outlined [here](https://arxiv.org/pdf/1902.01046.pdf). 

It's a lot of work, but we think it will be worth it.

We'll also be including automated versions for all of our basic genomics workflows used to support work at [GP2]() and [CARD]().  
These will include:
* Pre-imputation genotype calling and quality control
* Fast and easy GWAS
* RNA-seq normalization
* Network community builds
* GWAS meta-analysis (random effects)
* Various other functions from the community geared towards making your research easier and more efficient

## 2021
Further expansions of functionality are planned  
Enterprise version for easily scalable work (GUI build requiring substantial time investment)
