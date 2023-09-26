# multi QC
https://multiqc.info
https://github.com/CoarfaBCM/CoarfaLab-tools/blob/master/ccPythonBase/docs/RNASeq-Protocol.readme.md

conda install -c bioconda multiqc
then cd to the folder with the raw fastq or trimmed files and run
multiqc .



To run fastqc on multiple fastq.gz samples
 
for m in  *fastq.gz; do fastqc -o fastqc $m; done
 
then can run multiqc to summarize the results
 
multiqc .