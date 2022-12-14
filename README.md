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

### Data Processing

For data processing, I selected metadata from SRA Run Selector and counts data from GEO. I noticed there was an omission of 20 cerebellum samples from the SRA Run Selector metadata. I went into the metadata files from individual sequencing platforms and added data corresponding to those 20 samples to the metadata.


### Conclusions
*  Criteria for quality control, library preparation, and sample availability are very good
*  Caveat: platform-driven batch effect
   *  Address with multifactor design
*  Imbalanced nature of the dataset (unequal case/controls) might pose problems for some machine learning applications


## Hippocampus displays highest number of differentially expressed genes

I compared differentially expressed genes by tissue type for ALS Spectrum MND vs. Non-Neurological Control using DESeq2 (Log2foldchange>1).

*  Spinal cord thoracic (68 samples) – 111 DEGs
*  Spinal cord lumbar (223 samples) – 1071 DEGs
*  Spinal cord cervical (239 samples) – 946 DEGs
*  Cortex motor medial (122 samples) – 694 DEGs
*  Cortex motor lateral (125 samples) – 665 DEGs
*  Cortex motor unspecified (70 samples) – 75 DEGs
*  Cortex occipital (74 samples) – 1 DEG
*  Cortex frontal (303 samples) – 439 DEGs
*  Cortex temporal (84 samples) – 142 DEGs
*  Cerebellum (300 samples) – 1134 DEGs
*  **Hippocampus (51 samples) – 3701 DEGs**

**Volcano Plot of Hippocampus RNA-seq data**.  
![image](https://user-images.githubusercontent.com/72508803/207172939-47b2097d-4fe0-4c49-be09-c1fcdbc0a69b.png)

### Conclusions

The hippocampus displays highest number of differentially expressed genes, suporting the hypothesis that this tissue is more affected by the disease. These results are also observed when comparing ALS Spectrum MND, Other Neurological Disorders vs. Non-Neurological Control.


## Oxidative phosphorylation, MYC signaling, mTORC1 signaling are deregulated in ALS Spectrum MND hippocampus tissue.

I performed enriched pathway analysis using the [Hallmark gene set](https://www.gsea-msigdb.org/gsea/msigdb/collection_details.jsp#H), KEGG pathways, and GO annotations with fast preranked gene set enrichment analysis (GSEA). I found a number of dysregulated pathways, including oxidative phosphorylation, MYC signaling, and mTORC1 signaling.

![Hallmark gene set](https://user-images.githubusercontent.com/72508803/207180754-28e41123-e6e4-4bfc-a792-616ec4dcf10c.png)

### Conclusions

A recent review of pathways affected in ALS ([Gall et. al, J. Pers Med (2020)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7564998/#:~:text=Here%2C%20the%20different%20pathways%20that,homeostasis%2C%20and%20aberrant%20RNA%20metabolism.)) discussed the following pathways in detail: mitochondrial dysfunction, oxidative stress, axonal transport dysregulation, glutamate excitotoxicity, endosomal and vesicular transport impairment, impaired protein homeostasis, and aberrant RNA metabolism. The dysregulated pathways make sense in this context. For example, affected oxidative phosphorylation and MYC signaling are related to oxidative stress, while dysregulated mTORC1 signaling is related to mitochondrial dysfunction.

## Evaluating potential changes in cell population abundance correlated to disease state.

### Methods for decomposing bulk expression
[Bisque](https://www.nature.com/articles/s41467-020-15816-6): a tool for estimating cell type proportions in bulk expression.

[CIBERSORT](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5895181/): SVM-regression-based approach, utilizes a reference generated from purified cell populations
Major limitation: reliance on sorting cells to estimate a reference gene expression panel

[BSEQ-sc](https://pubmed.ncbi.nlm.nih.gov/27667365/): generates a reference profile from single-cell expression data that is used in CIBERSORT
MuSiC16: leverages single-cell expression as a reference, weighted non-negative least-squares regression (NNLS) model for decomposition

[MuSiC](https://www.nature.com/articles/s41467-018-08023-x): leverages single-cell expression as a reference, weighted non-negative least-squares regression (NNLS) model for decomposition


### Approach

[Jin & Lu, Genome Biology (2021)](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-021-02290-6) provides a thorough analysis of various deconvolution methods.

Ideally: obtain reference compositional information from previous publications
*  [PsychEncode Consortium](https://elifesciences.org/articles/14997): The PEC is producing a public resource of multi‐dimensional genomic data using unbiased genome‐wide approaches on tissue and cell‐type specific samples from approximately 1,000 phenotypically well‐characterized healthy and diseased human post‐mortem brains
   *  Bulk RNA-seq Decomposition and Deconvolution with Single-cell Data
   *  Brain Cell-type Marker Genes and Single-cell Expression Data (in units of TPM), from PEC (Developmental), Darmanis et al. 2015 and Lake et al. 2016
*  [Hipposeq: a comprehensive RNA-seq database of gene expression in hippocampal principal neurons](https://elifesciences.org/articles/14997)
*  [BisqueMarker](https://www.nature.com/articles/s41467-020-15816-6): reference-free method providing cell-type abundance estimations using only known marker genes - a weighted PCA-based (wPCA) decomposition approach
When the targeted tissue type has a relatively stable composition over several samples: deconvolution methods that are robust to non-orthogonal weight matrices (e.g. CIBERSORT, CIBERSORTx, MuSiC)

### Conclusions

*  Approach: Utilize multiple deconvolution techniques independently
*  Concordant results upon downstream analysis will provide strong evidence of successful cell type identification


## Future Directions
**Goal: understanding the etiology and molecular underpinning of Amyotrophic Lateral Sclerosis**

*  Further analysis of DEGs in hippocampus
   *  Examining expression levels of these DEGs in different tissues
*  Pathway analysis and deconvolution in other tissues (e.g. cerebellum)
*  GSEA with a correlation ranking metric to find which genes correlate most with a gene of interest/ look for enriched pathways
*  Regression task to see if there are genes/genes in tissues that are predictive or correlated with disease severity (requires more metadata)
   *  Classification to predict disease state from transcriptomics (with existing metadata)
*  Cluster the transcriptomes of patient samples into distinct molecular subtypes
   *  [Tam et. al, Cel Rep. (2019)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6866666/) did these types of analyses in cortex samples from this dataset
*  [Weighted correlation network analysis](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/1471-2105-9-559) (WGCNA) to find clusters (modules) of highly correlated genes
   *  [Wang et. al, Scientific Reports (2021)](https://www.nature.com/articles/s41598-021-85061-4) did this analysis focusing on the spinal cord in ALS patients
*  ChIP-seq/ ATAC-seq
   *  TDP-43 is a nuclear protein in a non-disease state [(Chen-Plotkin, et. al, Nat Rev Neurol. (2010))](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2892118/)
*  Spatial transcriptomic approach on tissues of interest to see where exactly the differentially expressed genes are and how cells expressing them differ in their tissue architecture
   *  [10X Visium sequencing](https://www.10xgenomics.com/spatial-transcriptomics): cell capture slides that contain four capture areas with 5,000 barcoded spots
   *  [MERFISH spatial profiling](https://vizgen.com/technology/): built upon single molecule FISH
*  Single-cell metabolomic profiling
   *  [Single-cell metabolic regulome profiling (scMEP)](https://www.nature.com/articles/s41587-020-0651-8): approach developed in human cytotoxic T cells
