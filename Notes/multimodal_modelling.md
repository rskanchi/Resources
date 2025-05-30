## What is Multimodal Modeling?

Multimodal modeling (MM) is the art and science of **combining multiple data types** (modalities) to make better 
predictions, classifications, or insights when compared to single data modality alone.

Biological systems are *complex* and no single data source can capture it all. Integrative models often reveal new biology. 
In terms of clinical value, multimodal models can lead to more accurate prognoses, earlier diagnoses, and better personalization of treatments.
Different modalities can provide overlapping information (*redundancy*) and unique insights (*complementarity*) which together lead to 
more robust models and better generalization.

## Common Modalities in Multimodal Modeling

| **Modality**        | **What It Measures**                         | **Tech Used**                             | **Central Dogma Step**       | **Snapshot Of**                                 | **Reliability & Caveats**                                |
| ------------------- | -------------------------------------------- | ----------------------------------------- | ---------------------------- | ----------------------------------------------- | -------------------------------------------------------- |
| **Genomics**        | DNA sequence, mutations, copy number         | WGS, WES, SNP arrays                      | DNA (starting blueprint)     | What’s *inherited* or *mutated*                 | Stable, but doesn’t reflect what’s active                |
| **Epigenomics**     | DNA methylation, chromatin accessibility     | ATAC-seq, bisulfite sequencing            | Regulatory overlay           | What’s *permitted* to be expressed              | Context-dependent; reflects cell state                   |
| **Transcriptomics** | mRNA levels (which genes are transcribed)    | RNA-seq, microarrays                      | RNA (DNA to RNA)             | What the cell *plans to do* (intention)         | Doesn’t guarantee translation into protein               |
| **Proteomics**      | Protein abundance/modifications              | Mass spec, western blot, RPPA             | Protein (RNA to Protein)     | What’s *actually being done* (action)           | Gold standard for function but harder to measure broadly |
| **Metabolomics**    | Small molecules, metabolites                 | LC-MS, NMR                                | Downstream cellular activity | Metabolic state and stress                      | Highly dynamic; varies by cell type and microenvironment |
| **Clinical Data**   | Phenotypes, treatments, outcomes             | EHRs, clinical trials, registries         | Not molecular                | Real-world outcome context                      | Varies across populations and care settings              |
| **Imaging**         | Tissue/organ structure (macro view)          | MRI, PET, CT                              | Macroscopic anatomy          | Tumor location, invasion, morphology            | Can’t see molecular drivers                              |
| **Pathology**       | Cellular structure and staining              | H&E staining, IHC, digital pathology      | Tissue-level morphology      | Cell shape, proliferation, protein localization | Qualitative but essential for diagnosis                  |
| **Single-cell**     | A modality per individual cell               | scRNA-seq, CyTOF, spatial transcriptomics | Any level (zoomed-in)        | Heterogeneity and rare cell types               | Data-rich but computationally intense                    |

## Individual data modality

- Understand the data's structure, quality, and potential biases (missingness, batch effects, etc.).
  - Handle missing data appropriately (imputation, removal, etc.).
  - Ensure appropriate normalization, transformation, etc. are done.
- Consider dimensionality reduction techniques (PCA, t-SNE, UMAP) to visualize high-dimensional data.
- Perform exploratory data analysis (EDA) to visualize distributions, correlations, and outliers.
- Identify informative features within each modality.
- Use domain knowledge to select relevant features that are biologically meaningful.
- Consider feature engineering to create new features that capture interactions or relationships between variables.

## Integrate modalities

There are 3 common approaches to integration after ensuring samples are aligned across modalities:

1. Early integration: Combine data before modeling. Simple approach but can add to high-dimensional data issues.
2. Intermediate integration: Extract features from each modality, then combine. Allows for more focused feature extraction but requires careful alignment of features.
3. Late integration: Train separate models and combine predictions. Flexible and interpretable, but may miss interactions between modalities.

- Choose the approach based on data characteristics, model complexity, and interpretability needs.
- Deep learning can do all this but interpretability is an issue.

