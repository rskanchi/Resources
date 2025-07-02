# Affymetrix data

Affymetrix GeneChip arrays are high-density oligonucleotide microarrays used to measure gene expression levels across thousands of genes simultaneously. Each gene is represented by a *probe set* consisting of 11-20 pairs of 25 base pair oligonucleotides (DNA sequences). The probe pairs include a perfect match (PM) with the target mRNA sequence and a mismatch (MM) probe which is identical to the PM probe except for a single base change to detect background noise (non-specific binding).  

The GeneChip arrays enable researchers to: - Measure expression levels of thousands of genes in parallel.  
- Compare gene expression between tissues, conditions, or time points.  
- Identify differentially expressed genes for biomarker discovery or mechanistic insight.  

# Data normalization

Summarization and normalization are critical steps in analyzing Affymetrix GeneChip data.

- To obtain a single expression measure for a gene, the intensity values from its 11-20 probe pairs need to be summarized. Early methods, like Affymetrix's Average Difference (AD), averaged the difference between PM and MM intensities. This approach is suboptimal due to issues like unequal variance across probes.

- Arrays often exhibit variation that is introduced during sample preparation, array manufacturing, and processing (labeling, hybridization, scanning), which are non-biological in origin. Normalization is the process of reducing this variation between arrays to ensure that observed differences in gene expression truly reflect biological variation rather than technical artifacts. Without appropriate normalization, comparing data from different arrays can lead to misleading results and inaccurate conclusions about gene expression differences.

- Robust Multi-array Average (RMA): The RMA method addresses the limitations of previous methods by adopting a new, empirically motivated statistical model.

1.  Background correction: PM intensities alone are used to adjust raw probe intensities to reduce noise. This is based on empirical findings that MM values often contain signal and that subtracting them can introduce noise.

2.  Quantile normalization: Removes systematic technical variation between arrays, making them directly comparable.

3.  Log-scale linear additive model: A log-scale linear additive model is fitted to the log-transformed PM intensities.

$$
log_2({PM}_{ij}) = e_i + a_j + \epsilon_{ij}
$$

where:

-   $e_i$ is the log$_2$ expression for sample i
-   $a_j$ is the probe-specific effect
-   $\epsilon_{ij}$ is the error term (random noise)

This model accounts for both the log-scale expression value of the gene on an array and a log-scale affinity effect for each probe, along with an error term.\

This comprehensive approach leads to significantly improved performance in terms of precision, consistency of fold change estimates, and sensitivity/specificity in detecting differential expression.

# References

- [Irizarry et al. 2003, Summaries of Affymetrix GeneChip probe level data. Nucleic Acids Research](https://doi.org/10.1093/nar/gng015)  

- [Bolstadet et al. 2003, A Comparison of Normalization Methods for High Density Oligonucleotide Array Data Based on Bias and Variance. Bioinformatics](https://doi.org/10.1093/bioinformatics/19.2.185)   

- [Irizarry et al. 2003, Exploration, Normalization, and Summaries of High Density Oligonucleotide Array Probe Level Data. Biostatistics](https://doi.org/10.1093/biostatistics/4.2.249)    

Adhoc analysis performed to combined expression data from `.CEL` files, and normalize [using R](https://github.com/CoarfaBCM/Rupa_projects/blob/main/AdHoc_requests/01072025_Fuqua_Affymetrix_Human/01072025_Fuqua_Affymetrix_Human.md)

