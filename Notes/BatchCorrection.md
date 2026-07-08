# What are batch effects?

Batch effects are unwanted technical differences between samples that arise from factors such as sequencing run, library preparation, processing date, plate, laboratory, or technician. These technical differences can introduce systematic variation that is unrelated to the underlying biology of interest.

If batch effects are not accounted for, samples may cluster by technical variables rather than biological conditions, leading to misleading biological conclusions and potentially incorrect downstream analyses.

---

# Why perform batch correction?

The goal of batch correction is to reduce unwanted technical variation while preserving true biological differences.

Batch correction can improve:

* Sample clustering
* Predictive modeling
* Other downstream exploratory analyses

Importantly, batch correction is **not** intended to remove biological variation.

---

# How do I know whether batch correction is needed?

A common first step is to visualize the samples using Principal Component Analysis (PCA).

Generate PCA plots and color the samples by:

* Batch
* Sequencing run
* Plate
* Processing date
* Biological condition
* Histology
* Sex
* Any other variables of interest

If samples cluster primarily by a technical variable rather than a biological variable, batch effects are likely present.

---

# How do I know whether batch correction worked?

Compare PCA plots before and after correction.

A successful batch correction generally shows:

* Reduced clustering by the batch variable
* Preservation of biological separation
* No obvious introduction of new artifacts

It is often helpful to generate PCA plots colored by multiple variables both before and after correction. This allows you to verify that technical variation has decreased while meaningful biological structure remains.

---

# Is batch correction the same as normalization?

No, batch correction is **not** the same as normalization.  

These are different preprocessing steps that address different sources of variation.

| Step                 | Purpose                                              |
| -------------------- | ---------------------------------------------------- |
| **Normalization**    | Adjusts for sequencing depth and library size        |
| **Batch correction** | Removes unwanted technical variation between batches |

Normalization should generally be performed before considering batch correction.

---

# Should I perform PCA on raw counts?

Generally, no.

Raw RNA-seq counts have a strong mean-variance relationship, meaning highly expressed genes dominate the variance. As a result, PCA performed directly on raw counts can be difficult to interpret.

Instead, PCA is typically performed on transformed data, such as:

* Variance stabilizing transformation (VST)
* Regularized log transformation (rlog)
* logCPM (edgeR)

These transformations stabilize the variance across genes, making distances between samples more meaningful.

---

# Why use variance stabilizing transformation (VST)?

RNA-seq counts are heteroscedastic, meaning genes with larger counts also tend to have larger variances.

The variance stabilizing transformation (VST) reduces this dependence between the mean and the variance, producing expression values that are more appropriate for:

* PCA
* Clustering
* Heatmaps
* Distance calculations

VST is intended for visualization and exploratory analyses, and not for differential expression testing.

---

# What does `blind = TRUE` mean?

When running `vst(dds, blind = TRUE)`, the transformation ignores the experimental design and estimates the mean-variance relationship across all samples.

This makes it appropriate for unbiased exploratory analyses such as PCA and quality control because the transformation is not influenced by knowledge of the biological groups.

In contrast, `vst(dds, blind = FALSE)` uses the experimental design during dispersion estimation. This can be preferable when many genes are expected to differ between biological groups, as it helps preserve large biological differences during the transformation.

* **blind = TRUE**: exploratory PCA and quality control
* **blind = FALSE**: downstream visualization after differential expression analysis or when preserving known biological effects is desirable

---

# Which batch correction method should I use?

## ComBat

ComBat uses an empirical Bayes framework to estimate and remove known batch effects from continuous expression data.

It is commonly applied to:

* Microarray data
* Proteomics
* Metabolomics
* Other normalized expression datasets

### Advantages

* Widely used
* Robust empirical Bayes framework
* Preserves biological covariates when specified

### Limitations

* Requires known batch labels
* Assumes batch effects can be modeled statistically
* Can overcorrect if batch and biology are confounded

---

## ComBat-seq

ComBat-seq extends ComBat specifically for RNA-seq count data.

Instead of assuming normally distributed expression values, ComBat-seq models counts using a negative binomial distribution.

Unlike the original ComBat, ComBat-seq operates directly on raw count data.

### Advantages

* Designed specifically for RNA-seq counts
* Maintains integer count structure
* Suitable for downstream count-based analyses

### Limitations

* Requires known batch labels
* Cannot separate biology from batch if they are completely confounded

---

## limma::removeBatchEffect()

`removeBatchEffect()` removes batch effects using a linear model.

It is commonly used for visualization after normalization or transformation.

Typical applications include:

* PCA
* Heatmaps
* Clustering

For differential expression analyses using limma, edgeR, or DESeq2, it is usually preferable to include batch directly in the statistical model rather than modifying the expression matrix beforehand.

---

## Why shouldn't I batch-correct before differential expression analysis?

Usually, **no**.

For differential expression (DE), the preferred approach is to analyze the **original count data** while including batch as a covariate in the statistical model.

For example, `design = ~ batch + condition` allows the model to estimate the effect of the biological condition while accounting for unwanted technical variation due to batch.

In other words, the model adjusts for batch **during statistical inference**, without modifying the original counts.

### Then why perform batch correction?

Differential expression analysis is only one part of an RNA-seq workflow.

Many exploratory analyses such as PCA, clustering, heatmaps, and dimensionality reduction operate directly on the expression matrix and do **not** account for batch effects statistically. In these cases, unwanted technical variation can dominate the results and obscure the underlying biology.

Creating a batch-corrected expression matrix allows these analyses to better represent the biological relationships between samples.

A useful way to think about it is:

| Goal                                                       | Recommended approach                         |
| ---------------------------------------------------------- | -------------------------------------------- |
| **Statistical inference** (Differential Expression)        | Original counts + include batch in the model |
| **Exploratory analysis** (PCA, clustering, heatmaps, etc.) | Batch-corrected expression data              |

This distinction explains why both approaches are commonly used within the same RNA-seq analysis workflow.

---

# Can batch correction remove real biology?

Yes, batch correction can remove real biology.

Batch correction should only be applied to variables representing unwanted technical variation.

If the corrected variable is biologically meaningful, true biological signal may be removed.

Similarly, if all cases were processed in one batch and all controls in another, batch correction cannot reliably distinguish technical effects from biological effects.

This situation is known as **confounding**, and no statistical method can completely recover the true biological signal.

---

# Example workflow using ComBat-seq

```r
library(DESeq2)
library(sva)
library(ggplot2)
library(ggfortify)

# counts: gene (rows) x sample (columns) count matrix
# pheno: sample (rows) metadata 
# pheno$batch: technical batch variable
# pheno$condition: biological variable of interest


# PCA before batch correction

dds.before <- DESeqDataSetFromMatrix(countData = counts, colData = pheno, design = ~ condition)
vsd.before <- vst(dds.before, blind = TRUE)
pca.before <- prcomp(t(assay(vsd.before)))

autoplot(pca.before, data = pheno, colour = "batch") + theme_classic()

# Batch correction
combat.counts <- ComBat_seq(counts = as.matrix(counts), batch  = pheno$batch, group  = pheno$condition)

# PCA after batch correction
dds.after <- DESeqDataSetFromMatrix(countData = round(combat.counts), colData = pheno, design = ~ condition)
vsd.after <- vst(dds.after, blind = TRUE)
pca.after <- prcomp(t(assay(vsd.after)))

autoplot(pca.after, data = pheno, colour = "batch") + theme_classic()

# Differential expression (use counts not batch-corrected data)

dds <- DESeqDataSetFromMatrix( countData = counts, colData = pheno, design = ~ batch + condition)
dds <- DESeq(dds)

```

---

# What I want to remember!

* Batch effects represent unwanted technical variation.
* PCA is one of the simplest ways to detect and evaluate batch effects.
* Batch correction aims to remove technical variation while preserving biology.
* Perform PCA on transformed data (e.g., VST), not raw counts.
* `blind = TRUE` is recommended for exploratory PCA and quality control.
* ComBat-seq should be applied to raw RNA-seq counts.
* For differential expression analysis, it is generally preferable to model batch in the design formula rather than batch-correct the counts.
* Always verify that biological signal is preserved after batch correction.

---

## References

- Johnson WE, Li C, Rabinovic A. Adjusting batch effects in microarray expression data using empirical Bayes methods. *Biostatistics*. 2007.
- Zhang Y, Parmigiani G, Johnson WE. ComBat-seq: Batch effect adjustment for RNA-seq count data. *NAR Genomics and Bioinformatics*. 2020.
- Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology*. 2014.


## Related topics
- Normalization
- Principal Component Analysis (PCA)
- Differential Expression
- Design Matrix


