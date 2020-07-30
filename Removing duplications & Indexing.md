#Removing Duplications & Indexing

To remove the duplcations in the alignment and to index;


```sh
source samtools-1.9
```


```sh
#!/bin/bash
#SBATCH -J samtools_rmdup&index_Ning1
#SBATCH -p jic-medium
#SBATCH -c 1
#SBATCH -N 1
#SBATCH --mem=10000
#SBATCH -o slurm.%j.out
#SBATCH -e slurm.%j.out
#SBATCH --mail-typ=END,FAIL
#SBATCH --mail-user=Hicret.Yalcin@jic.ac.uk


input_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/Alingments/bwa/merged_files

sample1=Ning_1
sample2=Ning_7
sample3=Pool_1
sample4=Pool_2
sample5=Pool_3
sample6=Pool_4


bam1=${sample1}.unique.merged.bam
bam2=${sample2}.unique.merged.bam
bam3=${sample3}.unique.merged.bam
bam4=${sample4}.unique.merged.bam
bam5=${sample5}.unique.merged.bam
bam6=${sample6}.unique.merged.bam


b1=$input_folder/$bam1
b2=$input_folder/$bam2
b3=$input_folder/$bam3
b4=$input_folder/$bam4
b5=$input_folder/$bam5
b6=$input_folder/$bam6



output_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/Alingments/bwa/merged_files/rmdup_bam_files


out1=${sample1}.rmdup.merged.bam
out2=${sample2}.rmdup.merged.bam
out3=${sample3}.rmdup.merged.bam
out4=${sample4}.rmdup.merged.bam
out5=${sample5}.rmdup.merged.bam
out6=${sample6}.rmdup.merged.bam



echo $b1
echo $out1


samtools rmdup $b1 $out1
samtools index $out1


cho $b2
echo $out2


samtools rmdup $b2 $out2
samtools index $out2


cho $b3
echo $out3


samtools rmdup $b3 $out3
samtools index $out3



cho $b4
echo $out4


samtools rmdup $b4 $out4
samtools index $out4



cho $b5
echo $out5


samtools rmdup $b5 $out5
samtools index $out5



cho $b6
echo $out6


samtools rmdup $b6 $out6
samtools index $out6
```

