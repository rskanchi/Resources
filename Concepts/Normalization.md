# Why normalization?

Normalization is one of the very first steps in almost every omics analysis workflow. With RNA-seq, proteomics, metabolomics, or other high-throughput data, one of the recommendations is:

> "Normalize the data before doing anything else."

But **why?**. What is the problem that normalization actually is trying to address?

Let's begin with a simple example. Imagine two RNA-seq samples.

|                        |   Sample A |   Sample B |
|------------------------|-----------:|-----------:|
| Total sequencing reads | 10 million | 50 million |
| Reads mapped to Gene X |        500 |      2,500 |

Is Gene X expressed **five times higher** in Sample B?

Notice that Sample B also contains five times more sequencing reads overall than Sample A. Suppose Gene X is expressed at the same biological level in both samples. If Sample A had also been sequenced to a depth of **50 million reads** instead of **10 million**, many more reads would be expected to map to Gene X simply because the transcriptome is sampled more extensively.

In other words, the higher count for Gene X in Sample B may not reflect higher biological expression. It may simply reflect the fact that Sample B was sequenced more deeply.

**Raw read counts reflect both biology and sequencing depth.**

Normalization attempts to reduce this technical variation so that samples become more comparable.

------------------------------------------------------------------------

## What exactly is a sequencing library?

This was one of the concepts that confused me when I first started learning RNA-seq. A **library** sounded like a collection of genes or perhaps a database. But a **sequencing library** is the version of a biological sample that is ready to be sequenced. Before sequencing, each biological sample undergoes several laboratory steps to generate its own sequencing library.

```         
Biological sample -> RNA -> Fragmentation -> Adapter ligation -> PCR amplification -> Sequencing library -> Sequencing -> Reads
```

After sequencing, the total number of reads obtained from that library is referred to as the **library size**. Once I understood that a library corresponds to a biological sample, terms such as **library size** and **library size normalization** became much easier to understand.

------------------------------------------------------------------------

## Why don't all samples have the same number of reads?

In an ideal world, all samples should have an identical sequencing depth. In reality, after each sample has been converted into a sequencing library, several factors influence how many reads are ultimately obtained.

For example,

- No two libraries are prepared in exactly the same way, so some naturally produce more usable sequencing reads/molecules than others. This is like the lab source of variability.
- Sequencing instruments exhibit small run-to-run variation, like sequencing the same library twice may produce slightly different numbers of reads.

The important point is that unequal sequencing depth is expected. It is a normal characteristic of sequencing data rather than an indication that something went wrong.

------------------------------------------------------------------------

## Why aren't simple scaling methods enough?

If sequencing depth were the only source of technical variation, normalization would be straightforward. Dividing the counts for each gene by the total number of sequencing reads in that sample would make the samples comparable. However, RNA-seq introduces another challenge. RNA-seq produces integer counts, but these are not absolute counts of RNA molecules. Instead, they reflect the abundance of each gene relative to the total RNA composition of that sample.

To illustrate this, consider two samples that are each sequenced to a depth of 20 million reads. In Sample A, the sequencing reads are distributed relatively evenly across many genes.

```         
Sample A

Gene A    ███
Gene B    ███
Gene C    ███
Gene D    ███
Gene E    ███
```

Now suppose Gene A is genuinely much more highly expressed in Sample B. Both samples are still sequenced to the same depth of 20 million reads, but because Gene A now represents a much larger proportion of the RNA molecules in that sample, a much larger proportion of the sequencing reads maps to Gene A.

```         
Sample B

Gene A    ███████████████
Gene B    ██
Gene C    ██
Gene D    ██
Gene E    ██
```

The sequencing depth has not changed. What has changed is **how the sequencing reads are distributed among the genes**.

Even if the biological abundance of Genes B–E remained unchanged, they now represent a smaller proportion of the total RNA molecules because Gene A became much more abundant. Consequently, their observed read counts may be lower simply because RNA-seq measures gene abundance relative to the composition of the sample. This phenomenon is known as **composition bias**. RNA-seq normalization methods, such as DESeq2's median ratio normalization and edgeR's TMM normalization, were developed to account for this type of bias.

------------------------------------------------------------------------

## Why do different RNA-seq tools use different normalization methods?

Different software packages attempt to solve the same problem, but they make slightly different assumptions about what a "typical" gene. The three most commonly used RNA-seq workflows are:

| Package | Typical normalization method | Intuition |
|-----------------|---------------------------|---------------------------|
| DESeq2 | Median ratio normalization | Assumes most genes are not differentially expressed; estimates a size factor for each sample. Samples with larger size factors are scaled down slightly, while samples with smaller size factors are scaled up. |
| edgeR | Trimmed Mean of M-values (TMM) | Assumes most genes are not differentially expressed; estimates a scaling factor for each sample and attempts to reduce the influence of genes that are extremely highly expressed, very lowly expressed or show unusually high differences between samples.  |
| limma-voom | Usually TMM (performed through edgeR before voom) |  Same as edgeR, TMM method. |

Each method attempts to estimate how much each sample should be scaled so that most genes become comparable across samples. The differences lie in **how those scaling factors are estimated**, not in the overall goal.

------------------------------------------------------------------------

## Does normalization depend on the differential expression method?

Normalization happens **before** differential expression analysis. Its purpose is to make samples more comparable by computing scaling factors for each sample. Normalization is done **before** statistical testing as in the sequence below:

| Stage | Goal | 
|----------------|----------------|
| Filtering | Remove genes with little information (i.e., almost no counts); they are unlikely to produce reliable differential expression results. |
| Normalization | Make samples comparable; Compute scaling factors that help reduce technical variation so that samples become more comparable. |
| Transformation | Stabilize the mean-variance relationship (if required); using say log2 on all data values. |
| Differential expression | Test whether genes differ between biological groups |

------------------------------------------------------------------------

## How do I know whether normalization worked?

Normalization is not typically evaluated by looking at one gene. Instead, the data as a whole are examined. Some common approaches include:

- PCA
- sample-to-sample distance heatmaps
- boxplots of expression distributions
- density plots

For example, if samples originally clustered according to sequencing depth but cluster according to biological condition after normalization, that suggests normalization successfully reduced unwanted technical variation. 

Normalization should improve comparability without removing biological signal.

------------------------------------------------------------------------

## Common misconceptions

- `Higher counts always mean higher expression`: Not necessarily. Higher counts may simply reflect deeper sequencing.
- `Normalization changes the biology`: No, normalization attempts to reduce technical differences while preserving biological differences.
- `Every normalization method produces identical results`: No, different methods estimate scaling factors differently because they make different assumptions about the data. DESeq2's median ratio normalization and edgeR's TMM normalization often produce broadly similar biological conclusions though.
- `Normalization and transformation are the same thing`: No, normalization adjusts samples and transformation adjusts the values to stabilize the mean-variance relationship (if needed).
- `Normalization removes batch effects`: No, normalization addresses technical differences related to sequencing depth and library composition. Batch effects are a separate issue and usually require additional correction methods if they are substantial.
- `featureCounts already normalizes my data`: No, `featureCounts` simply counts sequencing reads. The resulting raw counts reflect both biological variation and technical effects, such as sequencing depth and composition bias. These technical effects are addressed later during normalization.

------------------------------------------------------------------------

### "Why don't we just normalize using housekeeping genes?"

Housekeeping genes were widely used for normalization. The assumption was that these genes maintained relatively constant expression across samples. This approach is still common in targeted assays such as qPCR. Modern RNA-seq normalization methods generally make use of information from thousands of genes rather than relying on a small number of reference genes.

Similarly, other omics technologies use different normalization strategies.

For example,

- targeted metabolomics often uses internal standards
- untargeted metabolomics may use methods such as median or IQR normalization
- microarrays commonly use quantile normalization

The choice of normalization method depends on both the technology and the underlying assumptions.

------------------------------------------------------------------------

## Example workflow in R

### DESeq2

``` r
dds <- DESeqDataSetFromMatrix(
    countData = counts,
    colData   = metadata,
    design    = ~ condition
)

dds <- estimateSizeFactors(dds)

normalized_counts <- counts(dds, normalized = TRUE)
```

------------------------------------------------------------------------

### edgeR

``` r
dge <- DGEList(counts)

dge <- calcNormFactors(dge)

cpm_values <- cpm(dge)
```

------------------------------------------------------------------------

### limma-voom

``` r
dge <- DGEList(counts)

dge <- calcNormFactors(dge)

v <- voom(dge, design)
```

`limma-voom` typically relies on **edgeR** to estimate normalization factors before applying the voom transformation.

------------------------------------------------------------------------

## What I want to remember

- Raw counts reflect both biology and technical variation.
- Different samples naturally produce different sequencing depths.
- A sequencing library corresponds to one biological sample.
- Library size refers to the total number of sequencing reads obtained from that sample.
- Normalization makes samples more comparable.
- Sequencing depth is only one source of technical variation.
- Composition bias explains why simple library-size scaling is often insufficient.
- Composition bias means that sequencing reads are distributed according to the relative abundance of genes, making library-size scaling insufficient.
- DESeq2, edgeR, and limma all aim to make samples comparable, but estimate normalization factors differently.
- Statistical distributional assumptions become most important during differential expression analysis rather than during normalization.
- Normalization and transformation are different steps with different goals.

------------------------------------------------------------------------

## Related topics

- Filtering lowly expressed genes
- Differential expression
- Design matrix
- Variance stabilizing transformation (VST)
- Principal Component Analysis (PCA)
- Batch correction

This note focuses on the intuition behind normalization rather than the mathematical details of individual methods. Understanding the problem makes it a bit easier to appreciate why different normalization methods exist.
