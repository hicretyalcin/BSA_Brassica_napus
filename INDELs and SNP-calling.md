#SNP Calling 

The variations from each chromosome were  called separately.


```sh
source freebayes-1.1.0.46
```


```sh
#!/bin/bash
#SBATCH -J freebayes_chromosome
#SBATCH -p jic-medium
#SBATCH -c 1
#SBATCH -N 1
#SBATCH --mem=100000
#SBATCH -o slurm._\%A_\%a.out
#SBATCH -e slurm._\%A_\%a.out
#SBATCH --mail-typ=END,FAIL
#SBATCH --mail-user=Hicret.Yalcin@jic.ac.uk
#SBATCH --array=0-40


reference=/jic/scratch/groups/Chris-Ridout/hyalcin/private/Bn/Brassica_napus_v4.1.chromosomes.fa
input_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/Alingments/bwa/merged_files/rmdup_bam_files

i=$SLURM_ARRAY_TASK_ID

declare -a regions=(
"chrA01:1-23267856"
"chrA02:1-24793737"
"chrA03:1-29767490"
"chrA04:1-19151660"
"chrA05:1-23067598"
"chrA06:1-24396386"
"chrA07:1-24006521"
"chrA08:1-18961941"
"chrA09:1-33865340"
"chrA10:1-17398227"
"chrC01:1-38829317"
"chrC02:1-46221804"
"chrC03:1-60573394"
"chrC04:1-48930237"
"chrC05:1-43185227"
"chrC06:1-37225952"
"chrC07:1-44770477"
"chrC08:1-38477087"
"chrC09:1-48508220"
"chrA01_random:1-2687285"
"chrA02_random:1-1623597"
"chrA03_random:1-6015112"
"chrA04_random:1-1481564"
"chrA05_random:1-3017581"
"chrA06_random:1-2286780"
"chrA07_random:1-2069713"
"chrA08_random:1-2120946"
"chrA09_random:1-4134372"
"chrA10_random:1-2278402"
"chrC01_random:1-4437620"
"chrC02_random:1-5147596"
"chrC03_random:1-6509817"
"chrC04_random:1-4434705"
"chrC05_random:1-3728275"
"chrC06_random:1-3366307"
"chrC07_random:1-2949052"
"chrC08_random:1-4518928"
"chrC09_random:1-4437882"
"chrAnn_random:1-48658326"
"chrCnn_random:1-80671874"
"chrUnn_random:1-8317898"
)


declare -a chromosomes=(
"chrA01"
"chrA02"
"chrA03"
"chrA04"
"chrA05"
"chrA06"
"chrA07"
"chrA08"
"chrA09"
"chrA10"
"chrC01"
"chrC02"
"chrC03"
"chrC04"
"chrC05"
"chrC06"
"chrC07"
"chrC08"
"chrC09"
"chrA01_random"
"chrA02_random"
"chrA03_random"
"chrA04_random"
"chrA05_random"
"chrA06_random"
"chrA07_random"
"chrA08_random"
"chrA09_random"
"chrA10_random"
"chrC01_random"
"chrC02_random"
"chrC03_random"
"chrC04_random"
"chrC05_random"
"chrC06_random"
"chrC07_random"
"chrC08_random"
"chrC09_random"
"chrAnn_random"
"chrCnn_random"
"chrUnn_random"
)



sample1=Ning_1
sample2=Ning_7
sample3=Pool_1
sample4=Pool_2
sample5=Pool_3
sample6=Pool_4

bam1=${sample1}.rmdup.merged.bam
bam2=${sample2}.rmdup.merged.bam
bam3=${sample3}_reheaded_non_responsive.rmdup.merged.bam
bam4=${sample4}_reheaded_non_responsive.rmdup.merged.bam
bam5=${sample5}_reheaded_responsive.rmdup.merged.bam
bam6=${sample6}_reheaded_responsive.rmdup.merged.bam


output_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/SNP_calls/freebayes/Unique_map_chr_2nd_call_merged_pools


b1=$input_folder/$bam1
b2=$input_folder/$bam2
b3=$input_folder/$bam3
b4=$input_folder/$bam4
b5=$input_folder/$bam5
b6=$input_folder/$bam6


region=${regions[$i]}
chromosome=${chromosomes[$i]}


out=$output_folder/All_${chromosome}_merged_pools_rmdup_unique_bwa.vcf



echo $b1
echo $b2
echo $b3
echo $b4
echo $b5
echo $b6
echo $out



freebayes -q 20 -m 42 --min-coverage 20 -r $region -f $reference $b1 $b2 $b3 $b4 $b5 $b6 > $out


```


# Concatenation of the *vcf* files

Obtained Variant Call Format (*vcf*) files were concatenated using *bcftools v1.8*



```sh
source bcftools-1.8
```


```sh
#!/bin/bash
#SBATCH -J bcftools_concat
#SBATCH -p jic-medium
#SBATCH -c 1
#SBATCH -N 1
#SBATCH --mem=100000
#SBATCH -o slurm.%j.out
#SBATCH -e slurm.%j.out
#SBATCH --mail-typ=END,FAIL
#SBATCH --mail-user=Hicret.Yalcin@jic.ac.uk


input_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/SNP_calls/freebayes/Unique_map_chr_2nd_call_merged_pools


file1=All_chrA01_merged_pools_rmdup_unique_bwa.vcf
file2=All_chrA02_merged_pools_rmdup_unique_bwa.vcf
file3=All_chrA03_merged_pools_rmdup_unique_bwa.vcf
file4=All_chrA04_merged_pools_rmdup_unique_bwa.vcf
file5=All_chrA05_merged_pools_rmdup_unique_bwa.vcf
file6=All_chrA06_merged_pools_rmdup_unique_bwa.vcf
file7=All_chrA07_merged_pools_rmdup_unique_bwa.vcf
file8=All_chrA08_merged_pools_rmdup_unique_bwa.vcf
file9=All_chrA09_merged_pools_rmdup_unique_bwa.vcf
file10=All_chrA10_merged_pools_rmdup_unique_bwa.vcf
file11=All_chrC01_merged_pools_rmdup_unique_bwa.vcf
file12=All_chrC02_merged_pools_rmdup_unique_bwa.vcf
file13=All_chrC03_merged_pools_rmdup_unique_bwa.vcf
file14=All_chrC04_merged_pools_rmdup_unique_bwa.vcf
file15=All_chrC05_merged_pools_rmdup_unique_bwa.vcf
file16=All_chrC06_merged_pools_rmdup_unique_bwa.vcf
file17=All_chrC07_merged_pools_rmdup_unique_bwa.vcf
file18=All_chrC08_merged_pools_rmdup_unique_bwa.vcf
file19=All_chrC09_merged_pools_rmdup_unique_bwa.vcf
file20=All_chrA01_random_merged_pools_rmdup_unique_bwa.vcf
file21=All_chrA02_random_merged_pools_rmdup_unique_bwa.vcf
file22=All_chrA03_random_merged_pools_rmdup_unique_bwa.vcf
file23=All_chrA04_random_merged_pools_rmdup_unique_bwa.vcf
file24=All_chrA05_random_merged_pools_rmdup_unique_bwa.vcf
file25=All_chrA06_random_merged_pools_rmdup_unique_bwa.vcf
file26=All_chrA07_random_merged_pools_rmdup_unique_bwa.vcf
file27=All_chrA08_random_merged_pools_rmdup_unique_bwa.vcf
file28=All_chrA09_random_merged_pools_rmdup_unique_bwa.vcf
file29=All_chrA10_random_merged_pools_rmdup_unique_bwa.vcf
file30=All_chrC01_random_merged_pools_rmdup_unique_bwa.vcf
file31=All_chrC02_random_merged_pools_rmdup_unique_bwa.vcf
file32=All_chrC03_random_merged_pools_rmdup_unique_bwa.vcf
file33=All_chrC04_random_merged_pools_rmdup_unique_bwa.vcf
file34=All_chrC05_random_merged_pools_rmdup_unique_bwa.vcf
file35=All_chrC06_random_merged_pools_rmdup_unique_bwa.vcf
file36=All_chrC07_random_merged_pools_rmdup_unique_bwa.vcf
file37=All_chrC08_random_merged_pools_rmdup_unique_bwa.vcf
file38=All_chrC09_random_merged_pools_rmdup_unique_bwa.vcf
file39=All_chrAnn_random_merged_pools_rmdup_unique_bwa.vcf
file40=All_chrCnn_random_merged_pools_rmdup_unique_bwa.vcf
file41=All_chrUnn_random_merged_pools_rmdup_unique_bwa.vcf


f1=$input_folder/$file1
f2=$input_folder/$file2
f3=$input_folder/$file3
f4=$input_folder/$file4
f5=$input_folder/$file5
f6=$input_folder/$file6
f7=$input_folder/$file7
f8=$input_folder/$file8
f9=$input_folder/$file9
f10=$input_folder/$file10
f11=$input_folder/$file11
f12=$input_folder/$file12
f13=$input_folder/$file13
f14=$input_folder/$file14
f15=$input_folder/$file15
f16=$input_folder/$file16
f17=$input_folder/$file17
f18=$input_folder/$file18
f19=$input_folder/$file19
f20=$input_folder/$file20
f21=$input_folder/$file21
f22=$input_folder/$file22
f23=$input_folder/$file23
f24=$input_folder/$file24
f25=$input_folder/$file25
f26=$input_folder/$file26
f27=$input_folder/$file27
f28=$input_folder/$file28
f29=$input_folder/$file29
f30=$input_folder/$file30
f31=$input_folder/$file31
f32=$input_folder/$file32
f33=$input_folder/$file33
f34=$input_folder/$file34
f35=$input_folder/$file35
f36=$input_folder/$file36
f37=$input_folder/$file37
f38=$input_folder/$file38
f39=$input_folder/$file39
f40=$input_folder/$file40
f41=$input_folder/$file41



output_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/SNP_calls/freebayes/Unique_map_chr_2nd_call_merged_pools/concatenated



out=$output_folder/All_concat_merged_pools_rmdup_unique_bwa.vcf



bcftools concat $f1 $f2 $f3 $f4 $f5 $f6 $f7 $f8 $f9 $f10 $f11 $f12 $f13 $f14 $f15 $f16 $f17 $f18 $f19 $f20 $f21 $f22 $f23 $f24 $f25 $f26 $f27 $f28 $f29 $f30 $f31 $f32 $f33 $f34 $f35 $f36 $f37 $f38 $f39 $f40 $f41 > $out

```
