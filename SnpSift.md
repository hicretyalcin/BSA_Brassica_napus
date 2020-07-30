#*SnpSift* - Extracting the data

To analyse the obtained data through SnpEff, the relevant data including the annotations are extrasted using SnpSift; [http://snpeff.sourceforge.net/SnpSift.html](http://snpeff.sourceforge.net/SnpSift.html)

Only the script for ChrA04 shown as example;


```sh

#!/bin/bash
#SBATCH -J SnpSift_Extract
#SBATCH -p jic-medium
#SBATCH -c 1
#SBATCH -N 1
#SBATCH --mem=10000
#SBATCH -o slurm.%j.out
#SBATCH -e slurm.%j.out
#SBATCH --mail-typ=END,FAIL
#SBATCH --mail-user=Hicret.Yalcin@jic.ac.uk




output_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/snpEff/Merged_Pools

out=$output_folder/SnpEff_ChrA04_merged_pools_rmdup_unique_bwa_filter_parental_Q2000.txt



cd /jic/scratch/groups/Chris-Ridout/hyalcin/private/snpEff_latest_core/snpEff

source /ei/software/testing/bin/jdk-11.0.2

cat /jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/snpEff/Merged_Pools/ChrA04_merged_pools_rmdup_unique_bwa_filter_parental_Q2000.vcf \
    | ./scripts/vcfEffOnePerLine.pl \
    | java -jar SnpSift.jar extractFields - CHROM POS REF ALT TYPE QUAL RO AO DP "ANN[*].EFFECT" "ANN[*].IMPACT" "ANN[*].GENE" "ANN[*].GENEID" "ANN[*].FEATURE" "ANN[*].FEATUREID" "ANN[*].BIOTYPE" "ANN[*].RANK" "GEN[Pool_Non_responsive].RO" "GEN[Pool_Non_responsive].AO" "GEN[Pool_Non_responsive].DP" "GEN[Pool_Responsive].RO" "GEN[Pool_Responsive].AO" "GEN[Pool_Responsive].DP" "GEN[Ning_1].RO" "GEN[Ning_1].AO" "GEN[Ning_1].DP" "GEN[Ning_7].RO" "GEN[Ning_7].AO" "GEN[Ning_7].DP" > $out


```

