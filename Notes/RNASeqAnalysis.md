RNA seq analysis  
------------------------------------------------------------


Linear model approach to differential expression analysis requires an understanding of [how to create design matrices](https://github.com/rskanchi/Resources/tree/main/Library/DesignMatricesGuide.pdf). Design matrices define the structure of relationship between the explanatory and the response variables. Here, the explanatory variables are the treatments or conditions in which the samples exist or are designed to exist, and the response is the gene expression.



Software 
------------------------------------------------------------

Software            | Info
--------------------|----------------------------------------------------
R                   | Programming language for statistical computing and graphics; [R website](https://www.r-project.org/)
Rstudio             | Integrated development environment, IDE; [Rstudio website](https://posit.co/)
Visual Studio Code  | IDE; [VS website](https://code.visualstudio.com/)
Kallisto            | Program for quantifying abundances from bulk and single cell RNA-seq; [Kallisto](https://pachterlab.github.io/kallisto/)
featureCounts       | Program to count RNA-seq and genomic DNA-seq reads; [featureCounts](https://subread.sourceforge.net/)
FastQC              | Quality control tool for high-throughput sequence data; [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
MultiQC             | Tool to summarize results from multiple samples; [MultiQC](https://multiqc.info/)
STAR                | Algorithm for RNA-seq alignment, mapping reads to reference genome; [STAR](https://github.com/alexdobin/STAR)

The [hostmicrobe page](https://hostmicrobe.org/) includes [instructions](https://protocols.hostmicrobe.org/conda) on how to install Kallisto, FastQC and MultiQC using conda.

References  
------------------------------------------------------------
[NGS Analysis](learn.gencore.bio.nyu.edu)  
[Intro to differential gene expression analysis using RNA-seq](https://github.com/rskanchi/Resources/tree/main/Library/Intro2RNAseq.pdf)