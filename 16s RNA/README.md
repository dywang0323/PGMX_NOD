# 16s rRNA workflow
## Import Raw Data into QIIME 2
```
module load Python/3.8.6-GCCcore-10.2.0
module load QIIME2/2018.11-Miniconda3
qiime tools import \
  --type 'SampleData[PairedEndSequencesWithQuality]' \
  --input-path /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/Sample_list.csv \
  --output-path /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/demux-paired.qza \
  --input-format PairedEndFastqManifestPhred33
```
## Quality Control and Visualization
```
qiime demux summarize \
 --i-data /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/demux-paired.qza \
 --o-visualization /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/demux-paired.qzv
 --verbose
```

