# Why do we need Gene Set Enrichment Analysis (GSEA)?

A common strategy in differential expression analysis is to identify genes that are significantly associated with the phenotype, such as genes that are differentially expressed between two conditions or strongly correlated with an outcome. While this approach is useful, it has several limitations.

**What if biology acts through groups of genes rather than individual genes?**

Many biological processes are driven by coordinated changes across groups of functionally related genes rather than by a few genes with large expression changes. Looking only at individually significant genes may overlook these coordinated patterns.

**Sometimes biology whispers instead of shouts**

A biological process may involve many genes changing modestly and coordinately between two conditions or groups. There may be genes that cleared significance threshold with no obvious theme, or no genes at all that survive the strict multiple testing correction even though PCA plot showed a textbook-clean separation between the two groups.

**What if multiple studies of the same biological question identify different genes?**

Studies investigating the same biological question often produce different lists of differentially expressed genes because gene-level rankings are sensitive to sampling variation, technical noise, and biological heterogeneity. Although the individual genes may differ, the underlying biological pathways are often much more reproducible.

**How does GSEA address these limitations?**

Rather than asking whether individual genes are differentially expressed, GSEA asks if there is a group of genes, explaining a pathway or a process or a pre-defined geneset, that is *collectively* nudged toward one end of a **ranked list** of genes in the study. It is a non-parametric method (no assumptions about the data distribution) and is specifically designed to detect these coordinated "whispers" of biology that may be missed by gene-by-gene analyses.

---

## The journey

```         

- Perform a gene-level analysis (differential expression, correlation)
- Assign a ranking statistic to every gene (fold change, correlation)
- Order genes from one biological direction to the other using the ranking statistic
- Walk the ranked list from one end to other
- For a pre-defined geneset S, 
  - running sum goes UP when you hit a gene in the set S
  - and goes DOWN when the gene is not in the pre-defined geneset S
- Compute enrichment score (ES) = the maximum deviation of the running sum from zero
- Normalize ES to obtain normalized ES (NES)
- Permutation test using an appropriate null distribution of ES 
  - sample labels or gene labels
- Caclculate nominal p-values and FDR q-value
  - multiple-testing correction
- Identify leading-edge subset of genes which are responsible for the enrichment
- Interpret the result in biological context
```

Let's try to find answers below for why these steps exist.

---

## Why can't I just use my list of differentially expressed genes (DEGs)?

DEGs are a result of thresholds. Two labs studying the same disease often produce different sets of top genes due to different thresholds. **Reproducibility is fragile at the single-gene level.** GSEA doesn't depend on the DEG thresholds; it retains all genes detected in the study and allows their rank and score to contribute to identifying a biological pattern. Pathway-level signal tends to be more stable, because it aggregates across many noisy measurements. **The signal is real but small everywhere.** If several genes in a pathway each affect the outcome by a modest amount, none of them alone may survive correction for testing thousands of genes, but a coordinated effect across these interacting genes can be a real and consequential biological effect.

GSEA doesn't replace differential expression analysis, it's a second lens applied to the same ranked list, tuned to catch coordinated effects that gene-by-gene testing may fail to catch.

---

## Should I rank genes, and does it matter if I don't?

GSEA needs one number per gene that captures both direction and strength of association with the phenotype or outcome. The genes are ordered from strongest evidence in one direction to strongest evidence in the opposite direction. The order is fundamental because GSEA asks where members of a pre-defined geneset appear in this ranked list. A useful ranking metric should generally show direction of association, reflect strength of association, distinguish genes across the list, and correspond to the biological contrast.

---

## What about duplicated genes?

The ranked list should contain one score per unique gene. Duplicates can arise from transcripts or probes, but a rule is needed. Retaining the transcript with the largest absolute ranking metric, highest mean expression, or most reliable annotation.

---

## What is an Enrichment Score (ES)?

For **one geneset S at a time**, GSEA walks from one end of the ranked list to the other (like say from the gene most associated with or highly expressed in condition A to most associated with or highly expressed in condition B).

To understand what genesets are, please see the (geneset collections)[].

At each gene position:

- the running score increases (steps up), proportional to the ranking metric, when the gene belongs to the geneset S;
- the running score decreases (steps down), proportional to the ranking metric, when it does not.

If set S genes accumulate near the top, the running score rises early. If they accumulate near the bottom, it falls. If they are scattered randomly, the score remains around zero. The **Enrichment Score (ES)** is simply the highest point that running score reaches, the maximum deviation from zero.

A high ES means the geneset is unusually concentrated at one extreme of the ranked gene list. GSEA then moves to answer the next question: *is that concentration real, or could it have happened by chance?*

---

## How is ES computed actually?

Let

- $N =$ total number of ranked genes
- $S =$ gene set with a list of genes
- $N_H=|S|$ be the number of set members represented in the list
- $r_j =$ ranking statistic for gene $j$
- $p =$ the weighting exponent

Define the cumulative hit term:

$$P_{hit}(S,i)=
\sum_{\substack{g_j\in S\,j\le i}}
\frac{|r_j|^p}{\sum_{g_k\in S}|r_k|^p},$$

and the cumulative miss term:

$$P_{miss}(S,i)=
\sum_{\substack{g_j\notin S\,j\le i}}
\frac{1}{N-N_H}.$$

The running score is

$$RS(S,i)=P_{hit}(S,i)-P_{miss}(S,i).$$

The enrichment score is the largest deviation from zero, with sign determined by direction:

$$ES(S)=\max_i |RS(S,i)|.$$

A positive ES indicates enrichment near the top. A negative ES indicates enrichment near the bottom.

---

## Why do hits increase the score and misses decrease it?

The walk compares the observed cumulative distribution of geneset members with the distribution expected if those members were spread through the list. Every hit advances the set distribution. Every miss advances the background distribution.

The maximum difference captures where the set departs most strongly from random placement. The idea is related to the Kolmogorov–Smirnov statistic, although GSEA uses a weighted running sum.

---

## What does the weighting exponent do?

The exponent \(p\) controls how strongly the ranking statistic influences hit increments. When \(p=0\), all hits receive equal weight. When \(p=1\), genes with larger absolute ranking statistics contribute more strongly. The commonly used weighted statistic with \(p=1\) balances set membership with strength of gene-level evidence.

---

## Why do I need a null distribution for the enrichment score?

Suppose an analysis reports `ES=0.62` for a geneset. Is that good? Bad? How should it be interpreted? 

A value of 0.62 might be remarkably high for one geneset but small for another. So what should 0.62 be compared against? That is, the question formally becomes: what enrichment scores would we expect if there were actually no association between this geneset and phenotype?

GSEA generates a null distribution of enrichment scores using permutations (shuffling) to compare the observed ES against it. If the observed ES lies far into the tail of that distribution, it suggests that the geneset is unlikely to have reached that position by chance alone.

---

## What exactly are being shuffled?

There are two major approaches and they do not represent the same null hypothesis.

1. Phenotype label permutation, using sample x gene data.

The [original GSEA method (Subramanian et al., 2005, PNAS)](../Library/GSEA.pdf) permutes **sample (phenotype) labels**. It shuffles which samples are "condition A" vs. "condition B", recomputes the ranked list and the ES for each shuffle, and builds a null distribution for ES. The permutation of class labels preserves gene-gene correlations and the null hypothesis is `there's no association between phenotype and the pathway/geneset S`.

The catch: this requires enough samples (seven or more) per group to generate a meaningful number of distinct shuffles to build a null distribution. The number of distinct permutations is <1000 for less than seven samples per group. 
- 3 per group: C(6,3)/2 = 10
- 4 per group: C(8,4)/2 = 35
- 6 per group: C(12,6)/2 = 924
- 7 per group: C(14,7)/2 = 1716

2. Gene label permutation, using pre-ranked gene list.

Geneset or gene label permutation keeps the ranked gene list fixed and randomizes the association between genes and geneset S. An ES is recalculated for each randomized geneset, generating a null distribution of ES values expected by chance. The null hypothesis is `geneset S is not preferentially concentrated toward either end of the ranked gene list compared with a random geneset of the same size`. It's like asking the question `would a gene set of this size produce an enrichment score this extreme when randomly placed relative to the ranked list?` This is faster and works with small sample sizes or with a pre-ranked list where sample-level data isn't available. For instance, `clusterProfiler`'s `GSEA()`, `gseGO()`, and `gseKEGG()` (built on `fgsea`) default to gene label permutation. 

The two approaches answer slightly different null hypotheses, and the practical constraint (do you still have sample-level data, and do you have enough samples per group) usually decides the method to use.

---

## Why normalize ES into NES?

A geneset of size 400 has far more opportunities to up the "steps" than one with 15 members. If not corrected, GSEA would likely favor large genesets regardless of whether they're truly enriched.

The **Normalized Enrichment Score (NES)** corrects for this by scaling the ES relative to the geneset's size and to the variation seen across permutations. NES makes ES associated with genesets of different sizes comparable.

---

## What does the sign of NES mean?

The sign depends on how the rank metric was defined. For `Disease` - `Control`, positive NES indicates enrichment toward genes higher in disease and a negative NES indicates enrichment toward genes higher in control, or lower in disease.

If the contrast is reversed, the signs reverses. An enrichment plot without a clearly stated ranking direction can lead to bilogy being interpreted backward/in opposite direction.

---

## What is the leading-edge subset?

The ES is the *maximum* point that the running sum reached. The **leading-edge subset** is the set of genes encountered *before* the running sum hit its peak. That is, the genes that actually pushed the geneset or pathway toward enrichment. This matters because "pathway X is significantly enriched" is a statement about the whole gene set, but it's usually a handful of core genes driving that result, not all 200 members equally. The leading-edge subset turns a pathway-level enrichment into a shorter gene list to begin interpretation (it's not like a proof of mechanism).

For a positively enriched geneset, the leading-edge genes are those encountered from the top of the list up to the position where the running score reaches its maximum. For a negatively enriched geneset, they are the genes contributing up to the negative peak from the opposite end.

---

## How do I read an enrichment plot?

A standard plot usually contains:

- Running sum curve of enrichment score. Its largest positive or negative deviation is the ES.
- Barcode or tick marks which mark the location of genes from the geneset S. Dense ticks near one end indicate concentration there.
- Ranked-list metric in the lower panel may show the gene-level statistic from positive to negative.
- Peak identifies the point defining the leading-edge subset.

Interpret the plot together with ranking direction, NES, significance, set size, and leading-edge genes.

---

## Why do I get zero significant pathways even though my PCA plot clearly separates the groups?

A PCA separation may arise because many genes differ between the groups. However, those genes may:

- belong to many unrelated pathways,
- be poorly represented in the selected geneset collection,
- change in opposite directions within the same pathway, or
- produce pathway-level signals that are not strong enough to survive multiple-testing correction.

---

## What's the difference between GSEA and ORA?

ORA: `Given my list of significant genes, which biological pathways contain more of these genes than expected by chance?`
GSEA: `Without choosing a significance cutoff, do the genes from a biological pathway tend to appear near the top or bottom of my ranked gene list?`

ORA treats genes as either selected or not selected, whereas GSEA considers where every gene falls in a ranked list. GSEA uses information from all genes, so it can detect pathways in which many genes change modestly but consistently. ORA, on the other hand, focuses only on genes that meet the chosen selection criteria.

---

## What I want to remember

- GSEA tests whether a geneset trends toward one end of a ranked list. It doesn't require a significance cutoff.
- The Enrichment Score is the peak of a running-sum walk down the ranked list.
- Use NES to compare enrichment scores across genesets, not the raw ES.  
- Permutation type matters: sample-label permutation is more rigorous but needs enough samples per group; gene-label permutation is the practical default for pre-ranked lists (and what `clusterProfiler`/`fgsea` use by default).
- The leading-edge subset tells you which genes actually drove a significant result.
- No significant pathways doesn't necessarily mean no biology;  check the ranking metric and geneset collection choice.
- GSEA and ORA ask different questions from different inputs; they're complementary, not interchangeable.

---

## Related topics

- Over-representation analysis (ORA)
- [Gene set collections](GeneSet_Collections.md)
- Differential expression
- [Principal component analysis, PCA](PCA.md)
