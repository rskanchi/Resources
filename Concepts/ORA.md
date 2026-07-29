# Over-representation analysis (ORA)

Differential expression analysis may identify tens, hundreds, or thousands of genes associated with a condition or outcome. A list of genes alone, however, does not immediately explain the underlying biology.

ORA helps organize that list by asking whether particular biological pathways, processes, or other predefined gene sets contain **more of these genes than expected by chance**. The question it addresses is:  

> Is a gene set represented in my selected gene list more often than expected by chance?

To understand what gene sets are and how different collections are organized, see [gene set collections](GeneSet_Collections.md).

---

## The journey

```
- Perform gene-level analysis
- Generate the list of relevant or outcome-associated genes   
  - for example, FDR < 0.05 and |log2 fold change| > 1
- Define the background or gene universe
- Choose the relevant gene set collection(s)
- Build one 2 x 2 contingency table (discussed below) per gene set in a collection
  - count how many selected genes belong to the gene set
  - compare the observed overlap with the overlap expected by chance
- Apply Fisher's exact or hypergeometric test and compute a p-value
- Correct for multiple testing (because multiple gene sets in a collection)
- Examine the genes driving enrichment (overlapping genes)
- Interpret the result in biological context

```

---

## I have my list of differentially expressed genes (DEGs). Now what?

Differential expression analysis informs which genes differ between conditions. Biological systems usually operate through groups of interacting or functionally related genes. ORA moves from `Which genes changed?` to `Are particular biological processes represented unusually often among the genes that changed?` This requires several decisions:

- Which genes count as significant?
- Which genes belong in the background?
- Should UP and DOWN genes be analyzed separately?
- Which gene set collection should be tested?
- How should many simultaneous tests be handled?

ORA is simple only after these decisions are made carefully.

---

## What is the gene universe? 

The universe is the set of genes that could reasonably have been selected as relevant/significant. For a differential expression experiment, that is generally not every gene in the genome. A gene universe often includes genes that

- were measured by the assay
- passed quality control and expression filters 
- had valid identifiers
- could be matched to the gene-set annotation

If 20,000 genes exist but only 12,000 were tested, the other 8,000 had no opportunity to enter the DEG list. Including them changes the expected overlap and may distort the result.

---

## What input does ORA need?

ORA requires three main pieces of information: 

1. A selected gene list
2. A background gene list or the gene universe
3. A collection of predefined gene sets

The selected list may contain:

- differentially expressed genes
- genes associated with an outcome
- genes selected by a predictive model
- genes from a genomic region
- proteins or metabolites mapped to genes

The criteria used to create this list should be defined before running ORA.

---

## Why does ORA require a selected gene list?

ORA treats each gene as belonging to one of two categories:  selected, not selected. For example, genes may be selected via differential expression analysis using the threshold `FDR < 0.05` or `FDR < 0.05 and |log2 fold change| > 1`. The enrichment result therefore depends on the threshold used to define the selected list.

A slightly different cutoff can change:

- how many genes enter the analysis
- which genes enter the analysis
- which gene sets appear enriched

The input selected list (signature or DEGs) changes, so the contingency table changes. ORA requires a cutoff, but results may be sensitive to adjusted p-value threshold (significance) and fold-change threshold (effect size). This cutoff dependence is one motivation for [geneset enrichment analysis, GSEA](GSEA.md).

---

## Should I use a fold change cutoff too?

It depends on the scientific question. An adjusted p-value threshold prioritizes statistical evidence. A fold-change threshold prioritizes effect magnitude. Requiring both creates a more focused list but may exclude modest coordinated changes. The rule should be chosen before interpreting results and not repeatedly adjusted until a favorite pathway appears.

---

## What exactly is being counted?

Suppose that

- N = genes form the background or gene universe
- M = of those genes belong to one gene set
- n = genes were selected as significant
- k = significant genes belong to that gene set

The information forms a $$\(2\times2\)$$ table:

| | In the gene set | Not in the gene set | Total |
|---|---:|---:|---:|
| Significant genes | k | n-k | n |
| Other tested genes | M-k | N-M-n+k | N-n |
| Total | M | N-M | N |

Every gene set in a gene set collection produces its own table (which forms the statistical basis for enrichment in software used for this analysis).

---

## Is ORA really just a contingency table?

Statistically, yes. Biologically, the decisions and discussions before and after the table form the core of ORA; as seen in some of the questions above. Eventually, the scientific interpretation is important: whether overlapping terms (gene set names) describe the same biology, which genes produced the signal, whether direction is clear, and whether the interpretation fits the experiment.

The contingency table is simple. The scientific interpretation is not.

---

## What is the null hypothesis?

There is a contingency table based on a list of selected genes and a pre-defined gene set. For this pair, the null hypothesis is:

> Genes from the pre-defined gene set are not among my selected genes more often than expected by chance

The one-sided alternative is that the gene set is over-represented.

---

## What does “expected by chance” mean?

Suppose:

- 1,000 genes were tested
- 50 genes were selected
- 100 belong to a particular pathway or gene set
- 12 of the selected genes belong to that pathway

Because 10% of the background genes belong to the pathway, we would expect approximately 5 pathway genes in a randomly selected list of 50 genes.
$$
50 \times \frac{100}{1000}=5
$$
The observed overlap is 12, compared with an expectation of about 5. ORA evaluates how likely it would be to observe an overlap of 12 or greater by chance.

---

## What does the statistical test do?

Suppose we have already identified our selected genes and a particular gene set. We now know how many genes overlap.

The natural question becomes:

> Is this overlap larger than we would expect simply by chance?

If the row and column totals of the contingency table are treated as fixed, then the number of gene set members among the selected genes follows a **hypergeometric distribution**:
$$
P(K=k) = \frac{\binom{M}{k}\binom{N-M}{n-k}} {\binom{N}{n}}
$$
where

- N = number of genes in the background
- M = number of background genes belonging to the gene set
- n = number of selected genes
- K = number of selected genes belonging to the gene set

For over-representation, we calculate the probability of observing **at least** the overlap that was observed:
$$
P(K\ge k_{obs}) = \sum_{j=k_{obs}}^{\min(M,n)} \frac{\binom{M}{j}\binom{N-M}{n-j}} {\binom{N}{n}}
$$
In other words, the calculation asks:

> **If I randomly selected `n` genes from the background, how often would I obtain at least this many genes from the gene set?**

If this probability is very small, the observed overlap is unlikely to have arisen by chance alone, suggesting that the gene set is over-represented in the selected gene list.

Fisher's exact test and the hypergeometric test are closely related descriptions of the same counting problem. For ORA, they produce the same one-sided p-value for testing over-representation.

---

## Why does changing the universe change the ORA result?

The expected proportion of genes from a set is

$$
\frac{M}{N}
\]

If the universe changes, both `M` and `N` may change. So, the same selected DEG list can produce a different p-value under a different background.

---

## Why can a very small overlap be significant?

In addition to the number of overlapping genes, statistical significance also depends on

- the size of the selected list
- the size of the gene set
- the size of the background
- how unlikely the overlap is under random selection

For a very small gene set, an overlap of three or four genes may be statistically unlikely. However, a result based on only a few genes may also be unstable or difficult to interpret biologically.

The p-value should be considered together with gene set size, overlap count, enrichment ratio, contributing genes and biological relevance.

---

## Why are some genes in a gene set not used in the enrichment test?

The theory presented in the previous questions assumes an experimental background (all genes that could have been selected as differentially expressed in the experiment) and a gene set. In practice, ORA software first prepare inputs before performing the statistical test. One common preprocessing step is to restrict each gene set to genes that are also present in the experimental background. The enrichment test is then performed using this effective gene set. For example:

- 15,000 genes passed filtering and formed the experimental background
- A particular gene set contains 220 genes
- Of those, 180 are present in the experimental background

For this gene set:

- the background size remains 15,000
- the effective gene set size is 180

The remaining 40 genes were not measured or tested in the experiment and therefore could not have been selected. They are excluded from the enrichment test for this gene set.

Genes in the experimental background that do not belong to this or any other gene set are **not** removed from the analysis. They remain part of the background because they were eligible to be selected, even though they are not in the chosen gene set collection.

---

## What do the gene ratio, background ratio, and enrichment ratio mean?

Many ORA tools report the proportion of selected genes that belong to a gene set

$$
\text{Gene ratio}=\frac{K}{n}
$$

where

- k = number of selected genes in the gene set
- n = total number of selected genes

They may also report the proportion of background genes that belong to the gene set

$$
\text{Background ratio}=\frac{M}{N}
$$

where

- M = number of genes in the gene set that are present in the background
- N = total number of genes in the background

A gene set is over-represented when it occupies a larger fraction of the selected list than of the background

$$
\frac{K}{n}>\frac{M}{N}
\]

The enrichment ratio compares these two proportions

$$
\text{Enrichment ratio}
=
\frac{K/n}{M/N}
\]

This is equivalent to comparing the observed overlap with the expected overlap

$$
\text{Enrichment ratio}
=
\frac{\text{observed overlap}}
{\text{expected overlap}}
\]

Using the example above, if the observed overlap is 12 genes and the expected overlap is 5 genes

$$
\frac{12}{5}=2.4
\]

The gene set is represented approximately 2.4 times more often in the selected list than expected under random selection. The enrichment ratio describes the size of the over-representation. The p-value describes how surprising the observed overlap is under the null hypothesis. These quantities help explain the result, but they do not replace the statistical test. They should also be interpreted alongside the actual number of overlapping genes.

---

## Which genes are actually driving the enrichment?

For ORA, the contributing genes are

$$
L\cap S,
\]

where \(L\) is the selected list and \(S\) is the gene set.

The pathway label begins the interpretation. But it's important to ask questions such as

- How many genes contribute?
- Are they all changing in the expected direction?
- Are one or two genes dominating the interpretation?
- Are they shared across many terms?
- Do they fit the tissue, disease, treatment, and time point?

---

## Why do I need multiple testing correction?

ORA performs one statistical test for each gene set in the collection. ORA performs one statistical test for every gene set in the collection. As the number of statistical tests increases, so does the likelihood of obtaining apparently significant results simply because many hypotheses were evaluated. The p-values are therefore adjusted for multiple testing, commonly using the Benjamini–Hochberg false discovery rate.

An adjusted p-value (FDR) estimates the statistical evidence for enrichment while accounting for the number of gene sets tested. The correction applies only to the collection of gene sets evaluated in that analysis. 

---

## Should UP and DOWN genes be analyzed together?

ORA can show that a pathway contains many differentially expressed genes, but it cannot reveal whether those genes tend to increase or decrease.

Separate analyses allow clearer questions:

> Which pathways are over-represented among genes higher in condition A?

and

> Which pathways are over-represented among genes lower in condition A?

Even with separate analyses, enrichment does not prove that the biological pathway is activated or inhibited. The direction refers to the expression of the selected genes, not necessarily to the functional state of the pathway.

---

## Can ORA miss real biology?

ORA is useful when there is a meaningful (as regards to the scientific question) selected gene list. It is fast, interpretable, and often effective, but it can miss biology. For instance, a biologically interesting gene set or pathway may not make it to the list of enriched pathways when

- only a few of the pathway genes passed the selection threshold
- many pathway genes change modestly but fail the cutoff at the gene level analyses
- the pathway is missing or incompletely represented in the chosen gene set collection
- the gene set is very large or very small
- the DEG list is small
- the background is poorly defined

[GSEA](GSEA.md) addresses some of these limitations by using the entire ranked list. ORA and GSEA are not rivals, they ask related but different questions and can provide complementary evidence.

---

## What's the difference between ORA and GSEA?

ORA asks:

> Given my selected gene list, which gene sets contain more selected genes than expected by chance?

GSEA asks:

> Do genes from a gene set tend to occur near the top or bottom of a ranked list of all genes?

ORA:

- requires a selected gene list
- depends on a selection threshold
- evaluates overlaps with each gene set

GSEA:

- uses a ranked list
- does not require a significance cutoff
- uses information from all ranked genes
- evaluates whether gene set members cluster toward one end of the list

---

## What I want to remember

- ORA asks whether a gene set is represented among selected genes more often than expected by chance
- ORA requires both a selected gene list and an appropriate background
- The universe should contain genes that had a reasonable opportunity to be selected
- ORA results depend on the threshold used to define the selected list
- It is based on a \(2\times2\) table and Fisher's exact or hypergeometric testing
- Separate UP and DOWN analyses could be more informative
- An enriched gene set does not prove pathway activation or causality
- Contributing genes should be inspected before biological claims are made
- ORA and GSEA use the same gene sets (predefined biological knowledge) but test them differently

---

## Related topics

- [Gene Set Collections](GeneSet_Collections.md)
- [Gene Set Enrichment Analysis](GSEA.md)
- Differential expression
