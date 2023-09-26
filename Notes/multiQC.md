# multi QC
https://multiqc.info

conda install -c bioconda multiqc
then cd to the folder with the raw fastq or trimmed files and run
multiqc .



To run fastqc on multiple fastq.gz samples
 
for m in  *fastq.gz; do fastqc -o fastqc $m; done
 
then can run multiqc to summarize the results
 
multiqc .