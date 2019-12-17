---
id: about
title: About 
sidebar_label: About 
---

**GenoML** is an Automated Machine Learning tool that optimizes basic machine learning pipelines for genomic data. In recent years, the demand for machine learning experts has outpaced the supply, despite the surge of people entering the field. To address this gap, there have been big strides in the development of user-friendly machine learning software that can be used by non-experts. The first steps toward simplifying machine learning involved developing simple, unified interfaces to a variety of machine learning algorithms (e.g. R, Python scikit-learn). Although R and python have made it easy to experiment with machine learning, there is still a fair bit of knowledge and background in data science that is required to produce high-performing machine learning models. Development of machine learning models for genomic data in particular are notoriously difficult for a non-expert to tune properly. 

In order for machine learning software to truly be accessible to non-experts, we have designed an easy-to-use interface which automates the process of training a large selection of candidate models. **GenoML** will automate the most tedious part of machine learning by intelligently exploring many possible models to find the best one for your data.

**GenoML** can also be a helpful tool for the advanced user, by providing a simple wrapper function that performs a large number of modeling-related tasks that would typically require many lines of code, and by freeing up their time to focus on other aspects of the data science pipeline tasks such as data-preprocessing, feature engineering and model deployment.

Please note **GenoML is still under active development** and we encourage you to check back on this repository regularly for updates. The current version is stable but under transition. We are moving towards a complete python build that integrates better tools for cloud computing as well as meta/federated learning methods (to keep data sharing safe and secure while working across data silos). We plan to implement **meta-learning (meta-ML)** methods for algorithm selection across data silos as well as **federatedlearning (fed-ML)** for centralized tuning of predictive models using summary statistics from data silos. We believe that these remote learning methods across data silos will be important in future studies and in the context of the changing global data privacy landscape.

## Issues, suggestions, and collaboration

Please report any issue or suggestions to the GenoML issues page at https://github.com/GenoML/genoml/issues.

For other collaboration inquiries, please email mike@datatecnica.com. We are really happy to collaborate with pretty much anyone. If you are interested in this project, it would be great to hear from you.

## Team  
#### Core  
Mary Makarious (NIA-NIH)  
Hampton Leonard (NIA-NIH / DTI)  
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

Euesden, Jack, et al. “PRSice: Polygenic Risk Score Software.” Bioinformatics , vol. 31, no. 9, May 2015, pp. 1466–68.

