# Why PCA?

## What is it?

Principal Component Analysis (PCA) is a dimensionality reduction technique that summarizes high-dimensional data into a small number of new variables called **principal components**.

Instead of looking at thousands of variables individually (like genes, proteins, or metabolites), PCA captures the major patterns of variation across all variables and represents each sample using only a few principal components. This makes complex datasets much easier to visualize and interpret.

---

## Why should I care?

Modern omics datasets often measure thousands of features for each sample.

For example:

* RNA-seq: ~20,000 genes
* Proteomics: thousands of proteins
* Metabolomics: hundreds to thousands of metabolites

And we can visualize data in two or three dimensions easily, not thousands of dimensions.

PCA reduces these thousands of variables into a few dimensions while preserving as much of the overall variation in the data as possible.

This allows us to:

* Explore relationships between samples
* Identify clusters
* Detect outliers
* Evaluate technical effects such as batch
* Generate publication-quality visualizations

---

## Why can't I simply plot gene expression?

A scatter plot requires two variables.

An RNA-seq experiment contains expression values for thousands of genes.

Which two genes should you choose?

Even if you picked two genes, they would almost certainly fail to represent the overall relationships between samples.

PCA solves this problem by combining information from **all measured variables** into a small number of informative dimensions.

---

## Principal components sound complicated. What are they really?

Imagine each sample has expression values for **20,000 genes**.

Each sample can therefore be thought of as a single point in a **20,000-dimensional space**.

PCA finds a new set of axes that summarize the data while capturing as much of the variation between samples as possible.

These new axes are called **principal components**.

The first principal component (PC1) captures the largest source of variation in the data.

The second principal component (PC2) captures the next largest source of variation that is independent of PC1.

Additional principal components continue to explain progressively smaller amounts of variation.

Importantly, principal components usually do **not** correspond to individual genes. Instead, each principal component represents a weighted combination of all measured variables.

---

## What does "variance explained" mean?

Each principal component captures a certain percentage of the total variation present in the dataset.

For example,

* PC1 explains 38% of the variation.
* PC2 explains 17% of the variation.

Together, PC1 and PC2 explain 55% of the total variation in the data.

A larger percentage means that the component summarizes more of the differences between samples.

---

## Why do we color PCA plots by metadata?

PCA itself does **not** know anything about disease status, treatment groups, batch, sex, or any other sample annotations.

It simply identifies the largest sources of variation in the data.

By coloring samples according to metadata, we ask questions such as:

* Do disease samples separate from controls?
* Do samples cluster by sequencing batch?
* Are males and females separated?
* Are there outliers?

Coloring the points allows us to interpret what biological or technical factors may explain the observed separation.

---

## Can PCA detect batch effects?

Not exactly.

PCA does **not** detect batch effects.

Instead, PCA identifies the largest sources of variation in the dataset.

If batch happens to explain a large proportion of the variation, samples will separate according to batch.

Likewise, if disease status, treatment, or another biological variable explains most of the variation, PCA may separate samples according to those variables instead.

---

## The words we're afraid to utter: eigenvalues and eigenvectors

Let's take a peek under the hood. If you've taken a statistics or linear algebra course, you've probably encountered these terms. These form the mathematical foundation of PCA.

At a high level, PCA first summarizes how variables vary together using a covariance or correlation matrix. It then computes the **eigenvectors** of that matrix, which define a new coordinate system that captures the major patterns of variation in the data.

The **eigenvalues** tell us how much variation is captured along each new direction.

In practice, software report two results that are easier to interpret:

- **Scores**: indicate where each sample is located in PCA space
- **Loadings**: indicate how strongly each original variable contributes to each principal component

> **Scores tell you about samples. Loadings tell you about variables.**

For a step-by-step walkthrough of PCA using a tiny dataset of 8 samples and 6 genes, see the [Understanding PCA](https://github.com/rskanchi/EigenSpace/blob/main/docs/UnderstandingPCA.md) example in the [EigenSpace](https://github.com/rskanchi/EigenSpace) repository.

---

## Example R code

```r

# counts   : gene (rows) × sample (columns) count matrix
# metadata : sample (rows) metadata
# dds      : DESeq2 dataset object
# vsd      : variance-stabilized expression matrix
# pca      : principal component analysis results

library(DESeq2)
library(ggplot2)
library(ggfortify)

dds <- DESeqDataSetFromMatrix(
    countData = counts,
    colData = metadata,
    design = ~ condition
)

# Variance stabilizing transformation
vsd <- vst(dds, blind = TRUE)

# PCA
pca <- prcomp(t(assay(vsd)))

# Plot samples colored by disease status
autoplot(
    pca,
    data = metadata,
    colour = "condition"
) +
theme_classic()
```

---

## What I want to remember

* PCA is a dimensionality reduction technique.
* PCA summarizes thousands of variables into a few informative dimensions.
* Principal components are weighted combinations of all measured variables.
* PCA identifies the largest sources of variation in the data.
* PCA does not know what causes the variation; it simply summarizes it.
* PCA is one of the most useful tools for quality control and exploratory data analysis.
* For RNA-seq, perform PCA on transformed data (like using VST or log2), not raw counts.

---

## Related topics

- [Normalization](Normalization.md)
- Variance stabilizing transformation, VST
- [Batch correction](BatchCorrection.md)
- Differential expression
- Design matrix
- What is UMAP?





