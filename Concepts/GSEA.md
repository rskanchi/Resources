# Gene Set Enrichment Analysis (GSEA)  

Genome-wide expression data are increasing in volume and variety. Powerful computational tools and techniques are required to analyze these data, and to extract biological insight from such information. GSEA overcomes many analytical challenges by evaluating expression profiles at the level of gene sets, that is, groups of genes that share common biological function, chromosomal location or regulation.  

An intuitive approach is to find genes that *significantly* associated with the phenotype, i.e., differentially expressed between two conditions or highly correlated with an outcome.There are several limitation to the intuitive approach: 

- *Not genes but pathways*: What if the biological differences arise not from a handful of individual genes, but from interconnected networks where hundreds of genes with small or large effects act together?
- *Signal is too small*: After correcting for the statistical noise of testing thousands of genes, it's possible that no single gene meets the strict threshold for significance, even when a critical pathway is clearly affected with small and coordinated shifts across many genes in that pathway.  
- *Reproducibility*: Different groups studying the same problem often get non-overlapping top gene lists, since ranking methods are sensitive to noise and variation.  

Gene Set Enrichment Analysis (GSEA) is a non-parametric method that determines whether a predefined set of genes is collectively enriched at the extremes of a ranked gene list. The method calculates an Enrichment Score (ES) using a running-sum statistic. To visualize this, imagine the ranked list of genes as a road. The score steps upward each time a gene from a set appears and steps downward for genes outside the set. A high peak in this running sum indicates that the genes are clustered at one end of the list. GSEA assesses the significance of this clustering using permutation testing, normalizes the score (NES) to account for set size differences, and controls for multiple testing with a False Discovery Rate (FDR) to ensure the results are biologically robust.

#### Calculation of enrichment score (ES)  
ES of a gene set S represents the degree to which genes in S are overrepresented at the extremes of the ranked list L. First, contributions of each gene in L are computed with a weighted bump for genes in set S and a constant dip for genes not in set S. The weights for genes in S depend on the magnitude of association of the gene with the phenotype. The ES(S) is the maximum deviation from zero of the running sum of these contributions.  

#### Estimation of significance level of ES(S)  
Nominal p-value for each ES(S) is computed using permutation test procedure. The phenotype labels are permuted to preserve the complex correlation structure of the gene expression data or the gene-gene correlations and the L and ES recomputed for each permutation. The empirical, nominal p-value of the observed ES is computed using this null distribution of ES for each S.  

#### Adjustment for multiple hypothesis testing  
To account for differences in gene set size, the ES for each gene set is normalized to give a Normalized Enrichment Score (NES). A null distribution of NES is then generated using a fixed set of permutations, which are applied to all gene sets. This null distribution is used to compute the False Discovery Rate (FDR) q-values (and sometimes FWER p-values) for each gene set, which correct for multiple hypothesis testing and ensure robust results.  

#### Core genes of the pathway, leading-edge subset
The leading-edge set is the subset of genes within a pathway that contributes most strongly to the enrichment signal. These are the genes encountered before the running-sum statistic reaches its maximum deviation from zero. Focusing on the leading-edge set helps narrow down large pathways to the key drivers of enrichment, making it easier to interpret results, compare across pathways, and generate biologically meaningful hypotheses.

#### References

[*Gene set enrichment analysis: A knowledge-based approach for interpreting genome-wide expression profiles*](https://www.pnas.org/doi/full/10.1073/pnas.0506580102).  







