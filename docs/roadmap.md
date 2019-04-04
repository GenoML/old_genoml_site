---
id: roadmap
title: Roadmap  
---

> Update 03/26/19

We consider GenoML v1 a beta, it's the aggregation of code we have been using internally for quite some time, allcoming from a variety of different projects and applications. Moving towards a more refined GenoML v2 and v3, we've set some goals below and a loose timeline. 

## Spring / summer 2019 (GenoMLv2)
This development cycle includes:
1. Upping speed and efficiency of the pipeline.
2. Transitioning to a majority python core.
3. Integrating additional alogirthms from [scikit-learn](https://scikit-learn.org/stable/) and [TensorFlow](https://www.tensorflow.org).
4. Generally making the pipeline and docs more user friendly.
5. Additional feature extraction and variant pre-filtering methods. 

## Summer / fall 2019 (GenoMLv3)
This is when the exciting stuff really starts happening.

Everybody contributing to GenoML is really excited about the potential applications of **federated-learning (fed-ML)** and **meta-learning (meta-ML)** methods in genomics.

Particularly in light of recent privacy legislation changes, it would be great to be able to learn across multiple data silos.
We will use meta-ML to do algorithm competition and selection across data silos.
We will use fed-ML for centralized training/tuning based on aggregate summary statistics from private data silos.

GenoMLv3 will include fed-ML similar to what is outlined [here](https://arxiv.org/pdf/1902.01046.pdf). 

Its alot of work, but we think it will be worth it.

> Update 03/20/19

GenoMLv1 testing is ongoing, wider distribution pending April 8th 2019.
Do not use versions < 1.0.3.
