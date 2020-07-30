# Filtering of the variations

In order to exclude the variations with quality lower than 2000, depth lower than 20 and also to exclude the variations coming from Darmor-bzh, a filtering had been applied using *bcftools-1.8*

```sh
source bcftools-1.8
```


```sh
#!/bin/bash
#SBATCH -J bcftools_filter
#SBATCH -p jic-medium
#SBATCH -c 1
#SBATCH -N 1
#SBATCH --mem=10000
#SBATCH -o slurm.%j.out
#SBATCH -e slurm.%j.out
#SBATCH --mail-typ=END,FAIL
#SBATCH --mail-user=Hicret.Yalcin@jic.ac.uk


folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/SNP_calls/freebayes/Unique_map_chr_2nd_call_merged_pools/concatenated 

source bcftools-1.8

bcftools filter -i 'QUAL>2000 && DP>20 && ((GT[0] == "0/0" && GT[2] == "1/1" ) || ( GT[2] == "0/0" && GT[0] == "1/1"  ) || ( GT[0] == "1/1" && GT[2] == "2/2"  ) || ( GT[2] == "1/1" && GT[0] == "2/2"  )) ' -o $folder/All_concat_merged_pools_rmdup_unique_bwa_filter_Q2000_D20_Parent.vcf.gz -O z $folder/All_concat_merged_pools_rmdup_unique_bwa.vcf 
```

