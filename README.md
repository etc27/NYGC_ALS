# Exploring external NYGC Amyotrophic Lateral Sclerosis dataset GSE153960
[Gene Expression Omnibus Link](https://ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE153960)

[Prudencio, Humphrey, Pickles, et. al, J Clin Invest. (2020)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7598060)



## Experimental Design of NYGC ALS Consortium RNA-Seq
1659 samples from 11 tissues and 439 donors
*  Disease state
    *  Non-Neurological Control (280 samples)
    *  ALS Spectrum Motor Neuron Disease (MND) (962 samples)
    *  ALS Spectrum MND, Other Neurological Disorders (266 samples)
    *  Other Neurological Disorders (129 samples)
*  Disease state
    *  Spinal cord thoracic, spinal cord lumbar, spinal cord cervical
    *  Cortex motor medial, cortex motor lateral, cortex motor unspecified
    *  Cortex occipital, cortex frontal, cortex temporal
    *  Cerebellum
    *  Hippocampus 
*  Over 250 markers were used to confirm tissue, neuroanatomical regions, and assigned sex
*  Median sequencing depth of 42 million read pairs, with a range between 16 and 167 million read pairs
*  RNA-Seq samples were subjected to quality control modeled on the criteria of the Genotype Tissue Expression Consortium
    *  a unique alignment rate greater than 90%
    *  ribosomal bases of less than 10%
    *  a mismatch rate of less than 1%
    *  a duplication rate of less than 0.5%
*  Given the large size of the NYGC ALS cohort, RNA-Seq data were produced on 2 different sequencing platforms. Exploratory analyses showed a substantial batch effect in gene expression due to the 2 sequencing platforms.


Conclusions
*  Criteria for quality control, library preparation, and sample availability are very good
*  Caveat: platform-driven batch effect
   *  Address with multifactor design
*  Imbalanced nature of the dataset (unequal case/controls) might pose problems for some machine learning applications


