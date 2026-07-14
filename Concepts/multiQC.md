# multiQC
https://multiqc.info

MultiQC is a bioinformatics tool to aggregate log files or reports from multiple samples and/or tools into a single, interactive HTML report. It scans a directory, identifies the output formats of over 150 supported tools, and compiles all key statistics and metrics into one unified report with interactive plots and tables. It can be run using a single command `multiqc .`. MultiQC is especially valuable in large-scale sequencing projects where comparing QC metrics across hundreds of samples quickly is essential for identifying outliers and ensuring data quality before downstream analyses.

For instructions on installing multiQC, check [here](https://docs.seqera.io/multiqc/getting_started/installation).

What worked earlier for me on cluster: 

```
srun --job-name "InteractiveJob" --cpus-per-task 4 --mem=64G --time 24:00:00 --pty bash
source $HOME/.bashrc
conda create -n multiqc -c conda-forge -c bioconda multiqc

```

To summarize the sample reports, `cd` to the folder with the raw fastq or trimmed files and run `multiqc .`.  

Example: Run `fastqc` on multiple fastq.gz samples and then summarize using `multiqc`:

``` 
# fastqc
for m in  *fastq.gz; do 
  fastqc -o fastqc $m; 
done

# multiqc
conda activate multiqc
which multiqc
multiqc --version

multiqc .

```

`multiqc` will automatically detect the `.fastqc.html` and `.fastqc.zip` files for the R1 and R2 files. The output will include an interactive html file that summarizes stats from all samples and a folder with data tables based on which the plots in the html report are generated.

## Issues

```
conda activate multiqc
which multiqc
multiqc --version
```

The last command throws and error: `OpenBLAS error: Memory allocation still failed after 10 retries, giving up.`

Limit OpenBLAS threads manually. Use the following commands then try `multiqc --version`.

```
export OPENBLAS_NUM_THREADS=1
export OMP_NUM_THREADS=1
```

You should see output like: `multiqc, version 1.19`. You're preventing excessive memory allocation and thread spawning failures.
