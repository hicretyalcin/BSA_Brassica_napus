#*SnpEff* - Prediction of functional effects of variations

To be able to analyse the data on SnpEff, database was built according to the instructions;  http://snpeff.sourceforge.net/download.html
[](http://snpeff.sourceforge.net/download.html)


Only the script for ChrA04 shown as example;


```sh

#!/bin/bash
#SBATCH -J SnpEff_concat
#SBATCH -p jic-medium
#SBATCH -c 1
#SBATCH -N 1
#SBATCH --mem=10000
#SBATCH -o slurm.%j.out
#SBATCH -e slurm.%j.out
#SBATCH --mail-typ=END,FAIL
#SBATCH --mail-user=Hicret.Yalcin@jic.ac.uk



input_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/SNP_calls/freebayes/Unique_map_chr_2nd_call_merged_pools/concatenated


in=$input_folder/ChrA04_merged_pools_rmdup_unique_bwa_filter_parental_Q2000.vcf

output_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/snpEff/Merged_Pools

out=$output_folder/ChrA04_merged_pools_rmdup_unique_bwa_filter_parental_Q2000.vcf



cd /jic/scratch/groups/Chris-Ridout/hyalcin/private/snpEff_latest_core/snpEff

source /ei/software/testing/bin/jdk-11.0.2

java -Xmx4g -jar snpEff.jar Bn_v4.1 $in > $out
```

