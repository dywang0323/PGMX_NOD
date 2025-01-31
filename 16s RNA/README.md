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
 --o-visualization /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/demux-paired.qzv \
 --verbose
```
## Denoise and Remove Chimeras
```
export TMPDIR=/ourdisk/hpc/prebiotics/dywang/tmp
mkdir -p $TMPDIR
 TMPDIR=/ourdisk/hpc/prebiotics/dywang/tmp qiime dada2 denoise-paired \
  --i-demultiplexed-seqs /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/demux-paired.qza \
  --p-trunc-len-f 250 \
  --p-trunc-len-r 200 \
  --o-table /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/table.qza \
  --o-representative-sequences /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/rep-seqs.qza \
  --o-denoising-stats /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/denoising-stats.qza \
  --p-n-threads 20 \
  --p-n-reads-learn 100000 \
  --verbose
```
# Taxonomic Classification  
## 1). Download SILVA Classifier:  
```
wget https://data.qiime2.org/2018.11/common/gg-13-8-99-515-806-nb-classifier.qza
```
## 2). Classify Sequences: 
```
TMPDIR=/ourdisk/hpc/prebiotics/dywang/tmp qiime feature-classifier classify-sklearn \
  --i-classifier /ourdisk/hpc/prebiotics/dywang/Software/silva-132-99-515-806-nb-classifier.qza \
  --i-reads /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/rep-seqs.qza \
  --o-classification /ourdisk/hpc/prebiotics/dywang/Projects/HSC/Genome/16S/QIIME2/taxonomy.qza
```
