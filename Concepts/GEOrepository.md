# Gene Expression Omnibus (GEO)
The GEO [FAQs](https://www.ncbi.nlm.nih.gov/geo/info/faq.html) provide an overview of the repository, including how to submit data, access them, and how to use GEO tools.

Login to NCBI account. Under `My Submissions`, 
`Start a new submission`:
The option to go with for mRNA fastqs is `Sequence Read Archive`.



1. Prepare Your Metadata
You'll need to create a metadata spreadsheet with information about:

Sample characteristics (tissue type, treatment, time points, etc.)
Library preparation protocol
Sequencing platform and parameters
Experimental design

2. Choose Submission Path
For 32 paired-end samples, use the GEO website submission form rather than email:

Go to https://www.ncbi.nlm.nih.gov/geo/submission/
Log in with your NCBI account (create one if needed)
3. Create Series Record
Fill out the online form with:

Study title and summary
Overall experimental design
Publication information (if applicable)
Contact information

4. Upload Raw FASTQ Files
You have two options:
Option A - FTP Upload (recommended for 32 samples):

NCBI will provide you with a private FTP account
Upload all 64 files (32 R1 + 32 R2) to their server
Files should remain as .fastq.gz (gzipped)

Option B - SRA Submission:

If your data will also go to SRA (Sequence Read Archive), you can link it
This is actually the preferred method for raw sequencing data

5. Complete Sample Information
For each of your 32 samples, provide:

Sample name/ID
Source characteristics
Treatment conditions
Which files belong to which sample (link R1 and R2 pairs)

6. Processing Files (if applicable)
If you have processed data (count matrices, normalized data), include these as supplementary files.
Key tips:

Keep consistent naming conventions (e.g., Sample01_R1.fastq.gz, Sample01_R2.fastq.gz)
Include a README file explaining your file naming scheme
Budget several days for the upload if files are large
GEO staff will review and may request clarifications






-----------

screen session for ftp uploaad

*Didn't work 1*
```
(base) [u237865@mab Loor_exosome]$ lftp geoftp,inAlwokhodAbnib5 ftp-private.ncbi.nlm.nih.gov
lftp: geoftp,inAlwokhodAbnib5: Name or service not known
(base) [u237865@mab Loor_exosome]$ which lftp
/usr/bin/lftp
```

*Didn't work 2*
```
(base) [u237865@mab Loor_exosome]$ lftp -u username ftp-private.ncbi.nlm.nih.gov
Password:
lftp username@ftp-private.ncbi.nlm.nih.gov:~> cd uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq
cd: Login failed: 530 Login incorrect.
lftp username@ftp-private.ncbi.nlm.nih.gov:~> cd uploads/rupa.kanchi@orcid_mv0TgmqI
cd: Login failed: 530 Login incorrect.
lftp username@ftp-private.ncbi.nlm.nih.gov:~> exit
```

*Didn't work 3*
```
(base) [u237865@mab Loor_exosome]$ lftp -u username ftp-private.ncbi.nlm.nih.gov
Password:
lftp username@ftp-private.ncbi.nlm.nih.gov:~> ls
ls: Login failed: 530 Login incorrect.
lftp username@ftp-private.ncbi.nlm.nih.gov:~> exit
```


*Worked*
```
(base) [u237865@mab Loor_exosome]$ lftp ftp-private.ncbi.nlm.nih.gov
lftp ftp-private.ncbi.nlm.nih.gov:~> user geoftp inAlwokhodAbnib5
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:~> ls
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/> cd uploads/rupa.kanchi@orcid_mv0TgmqI
cd ok, cwd=/uploads/rupa.kanchi@orcid_mv0TgmqI
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI> ls
drwxrwxr-x   2 geoftp   geo          4096 Dec  9 07:47 RNAseq
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI> cd RNAseq/
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> ls
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> set net:timeout 300
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> set net:reconnect-interval-base 5
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> set net:max-retries 3
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> lcd /mount/modac4/rupa/GEO_uploads/Loor_exosome
lcd ok, local cwd=/mount/modac4/rupa/GEO_uploads/Loor_exosome
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> put CPM.EVLP_combined_combatseq_counts.06_20_2023.UQ.csv
7846327 bytes transferred in 4 seconds (2.12M/s)
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> 
```

*Check*
```
(base) [u237865@mab Loor_exosome]$ lftp ftp-private.ncbi.nlm.nih.gov
lftp ftp-private.ncbi.nlm.nih.gov:~> user geoftp inAlwokhodAbnib5
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:~> ls
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/> cd uploads/rupa.kanchi@orcid_mv0TgmqI
cd ok, cwd=/uploads/rupa.kanchi@orcid_mv0TgmqI                     
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI> ls
drwxrwxr-x   2 geoftp   geo          4096 Dec  9 09:29 RNAseq
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI> cd RNAseq/
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> ls
-rw-rw-r--   1 geoftp   geo      563037775 Mar  3  2022 1004_Lourdes24_S16_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      774711086 Mar  3  2022 1004_Lourdes24_S16_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      803827566 Mar  3  2022 1005_Lourdes24_S9_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1243389055 Mar  3  2022 1005_Lourdes24_S9_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      379523127 Mar  3  2022 1024_Lourdes24_S10_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      549842019 Mar  3  2022 1024_Lourdes24_S10_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      448777187 Mar  3  2022 1040_Lourdes24_S7_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      507178030 Mar  3  2022 1040_Lourdes24_S7_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      640976295 Mar  3  2022 1100_Lourdes24_S3_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1046930880 Mar  3  2022 1100_Lourdes24_S3_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      315724416 Mar  3  2022 1136_Lourdes24_S11_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      400706376 Mar  3  2022 1136_Lourdes24_S11_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      918514680 Mar  3  2022 1141_Lourdes24_S12_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1483274403 Mar  3  2022 1141_Lourdes24_S12_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      740587747 Mar  3  2022 1280_Lourdes24_S20_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1153152977 Mar  3  2022 1280_Lourdes24_S20_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      870316593 Mar  3  2022 1335_Lourdes24_S23_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1256623150 Mar  3  2022 1335_Lourdes24_S23_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      471646958 Mar  3  2022 1350_Lourdes24_S1_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      656015722 Mar  3  2022 1350_Lourdes24_S1_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1536329160 Mar  3  2022 1388_Lourdes24_S17_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1685919553 Mar  3  2022 1388_Lourdes24_S17_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      414940881 Mar  3  2022 1392_Lourdes24_S13_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      535912081 Mar  3  2022 1392_Lourdes24_S13_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      676736814 Mar  3  2022 1414_Lourdes24_S8_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1030615053 Mar  3  2022 1414_Lourdes24_S8_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      796481743 Mar  3  2022 1437_Lourdes24_S14_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1252656924 Mar  3  2022 1437_Lourdes24_S14_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      960700059 Mar  3  2022 1467_Lourdes24_S18_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1552384397 Mar  3  2022 1467_Lourdes24_S18_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      422370526 Mar  3  2022 1491_Lourdes24_S4_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      551828090 Mar  3  2022 1491_Lourdes24_S4_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      322167942 Mar  3  2022 1564_Lourdes24_S5_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      426057885 Mar  3  2022 1564_Lourdes24_S5_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      691596289 Mar  3  2022 1574_Lourdes24_S21_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1074777043 Mar  3  2022 1574_Lourdes24_S21_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      536334047 Mar  3  2022 1654_Lourdes24_S6_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      604551801 Mar  3  2022 1654_Lourdes24_S6_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      581403130 Mar  3  2022 1703_Lourdes24_S15_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      878603866 Mar  3  2022 1703_Lourdes24_S15_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      272258870 Mar  3  2022 1788_Lourdes24_S24_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      376534527 Mar  3  2022 1788_Lourdes24_S24_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1574181681 Mar  3  2022 1829_Lourdes24_S19_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      2572581063 Mar  3  2022 1829_Lourdes24_S19_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      406082178 Mar  3  2022 1831_Lourdes24_S2_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      622489459 Mar  3  2022 1831_Lourdes24_S2_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      896916683 Mar  3  2022 1887_Lourdes24_S22_L001_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      1190798817 Mar  3  2022 1887_Lourdes24_S22_L001_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo       7846327 Dec  9 08:11 CPM.EVLP_combined_combatseq_counts.06_20_2023.UQ.csv
-rw-rw-r--   1 geoftp   geo      3146476608 May 26  2023 H-42256-ID1055_S1_L000_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3959717548 May 26  2023 H-42256-ID1055_S1_L000_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3502249642 May 26  2023 H-42256-ID1825_S1_L000_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3581058040 May 26  2023 H-42256-ID1825_S1_L000_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      2396409763 May 26  2023 H-42256-ID1972_S1_L000_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      2621575302 May 26  2023 H-42256-ID1972_S1_L000_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      2386897355 May 26  2023 H-42256-ID2040114_S1_L000_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      2379313722 May 26  2023 H-42256-ID2040114_S1_L000_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      2838297276 May 26  2023 H-42256-ID2040311_S1_L000_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3033355532 May 26  2023 H-42256-ID2040311_S1_L000_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3842136075 May 26  2023 H-42256-ID20422013_S1_L000_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      4004681998 May 26  2023 H-42256-ID20422013_S1_L000_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3135549905 May 26  2023 H-42256-ID20522018_S1_L000_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3499829791 May 26  2023 H-42256-ID20522018_S1_L000_R2_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3312165501 May 26  2023 H-42256-ID2060819_S1_L000_R1_001.fastq.gz
-rw-rw-r--   1 geoftp   geo      3708934086 May 26  2023 H-42256-ID2060819_S1_L000_R2_001.fastq.gz
lftp geoftp@ftp-private.ncbi.nlm.nih.gov:/uploads/rupa.kanchi@orcid_mv0TgmqI/RNAseq> bye
```

