# Gene set collections

Gene set enrichment methods do not discover pathways from scratch. They test whether our data support biological knowledge that has already been organized into **gene sets**.

That sounds simple until we open [MSigDB](https://www.gsea-msigdb.org/gsea/msigdb) and see there are so many **gene set collections**, with a few hundreds to thousands of gene sets in each collection.

---

## What exactly is a gene set?

A gene set is a group of genes connected by a defined biological, functional, regulatory, positional, or experimental relationship.

A gene set may represent:

- a biological process
- a signaling or metabolic pathway
- a molecular function
- a cellular location
- genes regulated by the same transcription factor
- genes associated with a disease or phenotype 
- genes that changed together in a published experiment

Mathematically, a gene set can be written as

\[
S = \{g_1, g_2, \ldots, g_K\},
\]

where \(K\) is the number of genes assigned to the set.

The notation is easy. The important part is the biological statement connecting the genes.

---

## Is a pathway the same as a gene set?

Not always; a [biological pathway](https://www.genome.gov/about-genomics/fact-sheets/Biological-Pathways-Fact-Sheet) usually describes a series of actions in a cell (an organized biological mechanism) that results in the formation of a new product or a change in that cell. 

A gene set is simply a collection of genes with a defined relationship. It can be researcher-defined! Some examples of genesets to say it is broader in context:

- a Reactome pathway can be represented as a gene set
- GO Biological Process term
- a transcription-factor target list
- a disease signature from a published study

*Pathway analysis* is convenient shorthand, but many enrichment analyses include gene sets that are not pathways in the strict mechanistic sense.  

---

## Who decided that these genes belong together?

Some gene sets are highly curated and mechanistically focused. Some are broad, inferred, or derived from one experimental context. Some of the sources gene sets come from are:

- expert curation of published evidence
- pathway databases
- ontology annotation
- transcription-factor binding data
- perturbation experiments
- computational inference
- signatures reported in published studies

Well-studied genes are extensively annotated and may occur in many gene sets. Poorly characterized genes may appear in few or none. It's important to understand that enrichment methods test using these gene sets. So, absence of enrichment does not necessarily mean absence of biology. The relevant biology may be missing or incompletely annotated.

---

## Where do all these gene sets live?

A widely used resource is the **Molecular Signatures Database (MSigDB)**.

| Collection | Focus/theme |
|---|---|
| H | Hallmark gene sets |
| C1 | Positional gene sets |
| C2 | Curated gene sets, including pathway databases and published signatures |
| C3 | Regulatory target gene sets |
| C4 | Computational gene sets |
| C5 | Ontology gene sets, including Gene Ontology |
| C6 | Oncogenic signatures |
| C7 | Immunologic signatures |
| C8 | Cell-type signatures |

These collections have different number of gene sets and answer different biological questions. 

---

## Hallmark, GO, KEGG, ... : why are there so many?

Because biological knowledge can be organized at different levels.

### Hallmark

The Hallmark collection contains 50 gene sets representing broad and relatively coherent biological states or processes. It was designed to reduce redundancy among larger collections.

Hallmark is often useful for a first pass because the output is compact and easier to summarize.

### Gene Ontology

Gene Ontology contains three domains:

- Biological Process
- Molecular Function
- Cellular Component

GO is hierarchical. Broad parent terms contain more specific child terms. This provides extensive coverage but also creates substantial overlap and redundancy.

### KEGG

KEGG contains curated metabolic, signaling, disease, and molecular-system pathways. The pathway diagrams are often intuitive, although pathway breadth varies.


---

## Hallmark has only 50 gene sets. Is that a limitation?

With a Hallmark collection, fewer hypotheses are tested, there's less redundancy, broad programs are easier to recognize, and the results are easier to communicate/interpret.

It could be a limitation because:

- fewer biological themes are covered
- broad categories can hide specific mechanisms
- an important process may not have its own Hallmark set

A useful strategy is often to begin with Hallmark to identify broad programs and follow with GO Biological Process or Reactome for details. It's like Hallmark shows the neighborhood. GO and Reactome show every street.  

---

## GO Biological Process has thousands of terms. Is it better?

More coverage is valuable, but more output doesn't mean more understanding.

GO may return terms such as:

- leukocyte activation
- regulation of leukocyte activation
- positive regulation of leukocyte activation
- lymphocyte activation
- regulation of lymphocyte activation

These are not necessarily five independent discoveries, but overlapping descriptions of one underlying immune signal. The challenge is therefore not only to identify significant terms, but also to determine which terms represent distinct biology.

GO's large size also increases the chance of finding statistically significant terms that are not biologically central to your study. Some enriched pathways may reflect generic cellular processes, immune activity, stress responses, or other background biology rather than the mechanism you are trying to understand. Interpreting GO results therefore requires biological judgment, not just statistical significance.

---

## Can one gene belong to several gene sets?

Yes, a gene can act in different cellular contexts, so it can be part of several pathways and can appear in both broad and specific ontology terms. For instance, a cytokine-related gene may appear in inflammatory response, cytokine signaling, immune-cell activation, infection response, JAK–STAT signaling, and disease-specific signatures.

---

## Why are there so many versions of gene set collections?

Gene set collections evolve. Genes are added or removed, pathway definitions change, ontologies are revised, and identifiers are updated. Two analyses using the same collection name but different versions may not test exactly the same hypotheses. So, an analysis should specify:

- the database or collection name and its version/release (or the access date when appropriate)
- the organism

---

## Does gene set size matter?

Very small sets may produce unstable results based on only a few genes. Very large sets may be so broad that enrichment becomes difficult to interpret. Analysis tools often allow for restricting analysis to gene sets within a chosen size range, for example 10–500 genes. The optimal range depends on the gene set collection and the scientific question.

---

## Which collection should I use?

There is no universally best collection.

| Question | Possible collection |
|---|---|
| What broad biological programs differ? | Hallmark |
| Which specific biological processes differ? | GO Biological Process |
| Which signaling or metabolic pathways differ? | Reactome or KEGG |
| Which regulators may be involved? | Transcription factor target collections |
| Which immune signatures resemble my data? | Immunologic signatures |
| Which cell states may contribute? | Cell type signatures |

Using multiple collections can be informative, but each additional collection introduces more hypotheses, more redundancy, and more interpretation. Running every available collection because the software permits it is not the same as having an appropriate analysis plan.

---

## Does a significantly enriched gene set mean the biological pathway is activated?

A significantly enriched gene set does not prove that the corresponding biological pathway is activated or inhibited. It simply indicates that the genes in that gene set exhibit a pattern that is unlikely to have occurred by chance under the enrichment method used. The result is evidence that the pathway may be involved, but enrichment analysis alone cannot establish pathway activity or causality.

The gen eset significance should be interpreted in context by examining the direction of enrichment, the genes driving the signal, the experimental comparison, and whether the finding is biologically consistent with the tissue, time point, cell composition, and other supporting evidence.

---

## What do ORA and GSEA do with these collections?

**ORA** asks whether a selected gene list contains more members of a gene set than expected by chance. **GSEA** asks whether members of a gene set tend to occur near the top or bottom of a ranked list of all identified geens in the study. 

The collection defines the biological hypotheses. ORA and GSEA define how those hypotheses are tested.

See [ORA](ORA.md) and [GSEA](GSEA.md).

---

## What I want to remember

- A gene set is a group of genes connected by a defined relationship.
- Not every gene set is a mechanistic pathway.
- One gene can belong to many gene sets.
- Significant terms may be driven by overlapping genes.
- Database version, organism, identifier type, and mapping quality matter.
- An *enriched* gene set doesn't mean the correspoding pathway is activated or inhibited.
- The collection determines which biological hypotheses can be tested.
- ORA and GSEA use the same biological knowledge but test it differently.

---

## Related topics

- [Over-Representation Analysis](ORA.md)
- [Gene Set Enrichment Analysis](GSEA.md)
- Differential expression
- Multiple testing
