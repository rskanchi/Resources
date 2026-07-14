# Single cell 

# Cellranger count QC

QC column name/metric | What does it mean?
----- | -----
Sample | Sample name
Estimated Number of Cells | Number of barcodes that Cell Ranger determined are associated with intact cells. Indicator of no. of cells successfully captured.
Mean Reads per Cell | Average no. of sequencing reads associated with each cell. Calculated as No. of Reads / Estimated no. of Cells.
Median Genes per Cell | The median no. of genes detected across all cells. Key metric for cell complexity; higher is generally better.
Number of Reads | Total no. of raw sequencing reads generated for the sample.
Valid Barcodes | Fraction of Reads match a barcode on the 10x whitelist. **A high percentage (typically >98%) indicates the library was made correctly**.
Valid UMIs | Fraction of UMIs that were not homopolymers (e.g., not 'AAAAAAA') and do not contain 'N' bases. High values are expected.
Sequencing Saturation | Estimate of how thoroughly the library was sequenced. **A high saturation (e.g., >80%) means that generating more sequencing reads is unlikely to detect many new UMI molecules**. **Lower saturation means you could get more data by re-sequencing the library**.
Q30 Bases in Barcode | Percentage of bases in the cell barcode sequence that have a quality score of 30 or higher. Q30 means there's a 1 in 1,000 chance the base was called incorrectly. Higher is better.
Q30 Bases in RNA Read | Percentage of bases in the actual RNA transcript sequence with a quality score of 30 or higher. **Direct measure of sequencing quality**.
Q30 Bases in UMI | Percentage of bases in the Unique Molecular Identifier (UMI) sequence with a quality score of 30 or higher.
Reads Mapped to Genome | Percentage of Reads successfully align to the reference genome.
Reads Mapped Confidently to Genome | Fraction of Reads align to only one unique location in the genome.
Reads Mapped Confidently to Intergenic Regions | Reads confidently mapped to areas between genes. High numbers can sometimes suggest genomic DNA contamination.
Reads Mapped Confidently to Intronic Regions | Reads confidently mapped to the intron (non-coding) regions of genes. These are included in the final counts as they can indicate pre-mRNA.
Reads Mapped Confidently to Exonic Regions | Reads confidently mapped to the exon (protein-coding) regions of genes.
Reads Mapped Confidently to Transcriptome | Percentage of Reads mapped to a known gene (either exonic or intronic). **Important mapping metric**.
Reads Mapped Antisense to Gene | Reads mapped to a gene but on the opposite strand. **High values could indicate an issue with library preparation**.
Fraction Reads in Cells | Percentage of total sequencing Reads assigned to cell-associated barcodes. **A higher fraction suggests cleaner, more efficient cell capture with less ambient RNA**.
Total Genes Detected | Total no. of unique genes found across all cells in the sample.
Median UMI Counts per Cell | Median no. of unique molecules (UMIs) detected per cell. **Critical measure of library complexity and sequencing depth**.