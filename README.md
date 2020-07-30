# Bulk Segregant Analysis - *Brassica napus*
This repository including scripts for Bulk Segregant Analysis with B.napus

```Alingment of the reads```

first the reads coming from each sample were aligned to B. napus reference genome – Darmor-bzh ([http://www.genoscope. cns.fr/brassicanapus/data]()) using *bwa v0.7.1*. From each alignment a corresponding SAM file is created. Then, SAM files converted to BAM files and sorted using *samtools v1.9* 


```Removing Duplications & Indexing```

The duplications are removed from the created BAM files and indexing completed using *samtools v1.9*  


```Merging the pools```

Then the Non-responsive pools (Pool1+Pool2) and Responsive pools (Pool3+Pool4) are merged. 


```INDELs and SNP-calling```

After the indexing the BAM files, they were ready to run for SNP calling using *freebayes v1.1.0.46*. Because the processing of SNP calling took too long time and had been killed by the cluster, the SNPs from each chromosome had been called separately. Then, the obtained Variant Call Format (VCF) files were concatenated and filtered using *bcftools v1.8*. After that, VCF file had been obtained with all SNPs. 

```SnpEff```

Next, for further evaluation of the variations obtained through the pipeline, SnpEff ([http://snpeff.sourceforge.net/]()), a functional effect prediction toolbox had been used. It annotates and predicts the effects of genetic variants on genes and proteins.

```SnpSift```

To analyse the obtained data through SnpEff, the relevant data including the annotations are extrasted using SnpSift; [http://snpeff.sourceforge.net/SnpSift.html](). Then those obtained data was ready for data-mining.