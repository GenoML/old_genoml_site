---
id: introduction
title: Introduction 
sidebar_label: Introduction 
---
**GenoML = genomics + machine learning**  
A community for democratizing and automating common genomics and machine learning workflows for open science!  
Munge --> train --> tune --> predict ... explore + cluster + visualize + summarize.  

**GenoML** is in its current release is an automated machine learning tool (autoML) that optimizes basic machine learning pipelines with a preference for genomic data (although you can really use it on any type of data you want :smirk:).  As the community grows, so does **GenoML**'s capabilities. Right now, we are focused on data munging and autoML, but in the near future, we'll be adding a ton more workflows for both machine learning and genomics based on our work with [Data Tecnica International](https://www.datatecnica.com/), [the Global Parkinson's Genomics Project](https://www.parkinsonsroadmap.org/gp2/), the US National Institutes of Health's [Center for Alzheimer's and Related Dementias](https://www.nia.nih.gov/news/new-nih-alzheimers-center-accelerate-translational-research) and the [Chan Zuckerberg Initiative](https://chanzuckerberg.com/).    

In recent years, the demand for machine learning experts has outpaced the supply, despite the surge of people entering the field. To address this gap, there have been big strides in the development of user-friendly machine learning software that can be used by non-experts. The first steps toward simplifying machine learning involved developing simple, unified interfaces to a variety of machine learning algorithms (e.g. R, Python scikit-learn). Although R and python have made it easy to experiment with machine learning, there is still a fair bit of knowledge and background in data science that is required to produce high-performing machine learning models. Development of machine learning models for genomic data in particular are notoriously difficult for a non-expert to tune properly. 

In order for machine learning software to truly be accessible to non-experts, we have designed an easy-to-use interface which automates the process of training a large selection of candidate models. **GenoML** will automate the most tedious part of machine learning by intelligently exploring many possible models to find the best one for your data. We have also included a number of options for cleaning and optimizing geomic input data. In the future, we will be adding tons of extra functionality included expanded machine learning workflows and automating common genomics tasks.

**GenoML** can also be a helpful tool for the advanced user, by providing a simple wrapper function that performs a large number of modeling-related tasks that would typically require many lines of code, and by freeing up their time to focus on other aspects of the data science pipeline tasks such as data-preprocessing, feature engineering and model deployment. **GenoML** is quite robust in terms of production scale analyses, ask core team member [Hampton Leonard](https://twitter.com/HamptonLLeonard) about mining the combined UK Biobank genetics and electronic medical record datasets if you need an example.

Please note **GenoML is always under active development** and we encourage you to check back on this repository regularly for updates. We just completed a full python build that integrates better tools for cloud computing as well as laying the foundation for meta/federated learning methods (to keep data sharing safe and secure while working across data silos) to be added to the API. We plan to implement **meta-learning (meta-ML)** methods for algorithm selection across data silos as well as **federatedlearning (fed-ML)** for centralized tuning of predictive models using summary statistics extracted from these data silos. We believe that these remote learning methods across data silos will be important in future studies within the healthcare space and in the context of the changing global data privacy landscape. **Check the [roadmap]() for next major milestones in GenoML developement!**

## Issues, suggestions, need help?
Check us out on **Github**. Please report any issue or suggestions to the GenoML issues page at https://github.com/GenoML/genoml/issues.
If you want to try and troubleshoot your issue by yourself, please try running your command with the ```-v``` or ```-vvv``` options at the end to generate verbose outputs. **If you'd like to get more involved in GenoML and the community we are trying to build, please see the [collaborations page]() for additional info.**

## Team  
#### Core  
Mary Makarious (NIA-NIH)  
Hampton Leonard (NIA-NIH / DTI)  
Dan Vitale (NIA-NIH / DTI)  
Sayed Hadi Hashemi (UIUC)  
David Saffo (NU)  
Eduardo Salmerón Castaño (UM)
Cornelis Blauwendraat (NIA-NIH)  
Hirotaka Iwaki (NIA-NIH / MJFF / DTI)  
Roy H. Campbell (UIUC)  
Andrew B. Singleton (NIA-NIH)   
Juan A. Botia (UM)  
Faraz Faghri (NIA-NIH / UIUC / DTI)  
Mike A. Nalls (NIA-NIH / DTI)  
#### Collaborators
Lana Sargeant (VCU)  
Susan Chacko (Biowulf-NIH)  
Rafael Jordá Muñoz (UM)  
#### Affiliations  
Laboratory of Neurogenetics, National Institute on Aging, National Institutes of Health (NIA-NIH)  
Data Tecnica International, LLC (DTI)  
Department of Computer Science, University of Illinois at Urbana-Champaign (UIUC)  
College of Computer Sciences, Northeastern University (NU)  
The Michael J. Fox Foundation (MJFF)  
Biowulf High Performance Computing Cluster, National Institutes of Health (Biowulf-NIH)  
School of Nursing, VCU Health, Geriatric Health Clinic, Virginia Commonwealth University (VCU)  
University of Murcia (UM)  

## Acknowledgement

This work is brought to you by collaborative efforts in open source software supported to some degree by the Laboratory of Neurogenetics at the National Institute on Aging (National Institutes of Health), the Biowulf High Performance Computing Cluster (National Institutes of Health), University Illinois at Urbana-Champaign, the Michael J Fox Foundation, University of Murcia and Data Tecnica Int'l.

## Citations

#### If using GenoML for a publication, we ask that you cite the dependencies for the pre-filtering (SNP pruning) aspects of the pipeline including:

Purcell, Shaun, et al. “PLINK: A Tool Set for Whole-Genome Association and Population-Based Linkage Analyses.” American Journal of Human Genetics, vol. 81, no. 3, Sept. 2007, pp. 559–75.

Chang, Christopher C., et al. “Second-Generation PLINK: Rising to the Challenge of Larger and Richer Datasets.” GigaScience, vol. 4, Feb. 2015, p. 7.

#### GenoML stands on the shoulders of giants and would not be possible without these excellent resources from the python community (let's give some credit where its due):
pandas
pandas-plink
h5py
numpy
cython
scikit
xgboost
tensorflow
seaborn
matplot
statsmodels
argparse
