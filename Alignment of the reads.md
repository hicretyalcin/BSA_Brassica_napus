#Alignment of the reads

Only the script for Ningyou1 shown as example;

```sh
source bwa-0.7.17
source samtools-1.9
```

```sh
#!/bin/bash
#SBATCH -J bwa_unique
#SBATCH -p jic-long
#SBATCH -c 1
#SBATCH -N 1
#SBATCH --mem=100000
#SBATCH -o slurm.%j.out
#SBATCH -e slurm.%j.out
#SBATCH --mail-typ=END,FAIL
#SBATCH --mail-user=Hicret.Yalcin@jic.ac.uk
```

```sh
reference=/jic/scratch/groups/Chris-Ridout/hyalcin/private/Bn/Brassica_napus_v4.1.chromosomes.fa 
raw_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/rawdata_brassica/Uncompressed_data/C101HW18101353_1/raw_data
sample=Ning_1
lib=DSW66579-V_HTGLMCCXY

read1=Ning_1_${lib}_L1_1.fq.gz
read2=Ning_1_${lib}_L1_2.fq.gz
read3=Ning_1_${lib}_L2_1.fq.gz
read4=Ning_1_${lib}_L2_2.fq.gz
read5=Ning_1_${lib}_L3_1.fq.gz
read6=Ning_1_${lib}_L3_2.fq.gz
read7=Ning_1_${lib}_L4_1.fq.gz
read8=Ning_1_${lib}_L4_2.fq.gz


output_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/Alingments/bwa/Unique_read_bam_files

r1=$raw_folder/$sample/$read1
r2=$raw_folder/$sample/$read2
r3=$raw_folder/$sample/$read3
r4=$raw_folder/$sample/$read4
r5=$raw_folder/$sample/$read5
r6=$raw_folder/$sample/$read6
r7=$raw_folder/$sample/$read7
r8=$raw_folder/$sample/$read8



out=$output_folder/$sample.L1.bwa.sam 
out_bam=$output_folder/$sample.L1.bwa.unique.bam 
out_sorted=$output_folder/$sample.L1.bwa.unique.sorted

out2=$output_folder/$sample.L2.bwa.sam 
out_bam2=$output_folder/$sample.L2.bwa.unique.bam 
out_sorted2=$output_folder/$sample.L2.bwa.unique.sorted

out3=$output_folder/$sample.L3.bwa.sam 
out_bam3=$output_folder/$sample.L3.bwa.unique.bam 
out_sorted3=$output_folder/$sample.L3.bwa.unique.sorted

out4=$output_folder/$sample.L4.bwa.sam 
out_bam4=$output_folder/$sample.L4.bwa.unique.bam 
out_sorted4=$output_folder/$sample.L4.bwa.unique.sorted



echo $r1
echo $r2
echo $reference
echo $out
echo $out_bam
echo $out_sorted



bwa mem -t 4 -R "@RG\tID:${read1}\tSM:${sample}\tLB:{lib}" $reference $r1 $r2 > $out 
samtools view -b -q 42 -T $reference $out > $out_bam
samtools sort -O bam -T $out_sorted -o $out_sorted.bam $out_bam


echo $r3
echo $r4
echo $reference
echo $out2
echo $out_bam2
echo $out_sorted2



bwa mem -t 4 -R "@RG\tID:${read3}\tSM:${sample}\tLB:{lib}" $reference $r3 $r4 > $out2 
samtools view -b -q 42 -T $reference $out2 > $out_bam2
samtools sort -O bam -T $out_sorted2 -o $out_sorted2.bam $out_bam2



echo $r5
echo $r6
echo $reference
echo $out3
echo $out_bam3
echo $out_sorted3



bwa mem -t 4 -R "@RG\tID:${read5}\tSM:${sample}\tLB:{lib}" $reference $r5 $r6 > $out3 
samtools view -b -q 42 -T $reference $out3 > $out_bam3
samtools sort -O bam -T $out_sorted3 -o $out_sorted3.bam $out_bam3



echo $r7
echo $r8
echo $reference
echo $out4
echo $out_bam4
echo $out_sorted4



bwa mem -t 4 -R "@RG\tID:${read7}\tSM:${sample}\tLB:{lib}" $reference $r7 $r8 > $out4 
samtools view -b -q 42 -T $reference $out4 > $out_bam4
samtools sort -O bam -T $out_sorted4 -o $out_sorted4.bam $out_bam4

```





#Merging the lanes

To merge the aligned reads coming from different lanes but belongs to same sample;


```sh
source samtools-1.9
```


```sh
#!/bin/bash
#SBATCH -J samtools_merge
#SBATCH -p jic-medium
#SBATCH -c 1
#SBATCH -N 1
#SBATCH --mem=64000
#SBATCH -o slurm.%j.out
#SBATCH -e slurm.%j.out
#SBATCH --mail-typ=END,FAIL
#SBATCH --mail-user=Hicret.Yalcin@jic.ac.uk
```
```sh
input_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/Alingments/bwa/Unique_read_bam_files/unique_sorted_files

sample1=Pool_1
sample2=Pool_2
sample3=Pool_3
sample4=Pool_4
sample5=Ning_1
sample6=Ning_7

read1=${sample1}.L1.bwa.unique.sorted.bam
read2=${sample1}.L2.bwa.unique.sorted.bam
read3=${sample1}.L3.bwa.unique.sorted.bam
read4=${sample1}.L4.bwa.unique.sorted.bam
read5=${sample2}.L1.bwa.unique.sorted.bam
read6=${sample2}.L2.bwa.unique.sorted.bam
read7=${sample2}.L3.bwa.unique.sorted.bam
read8=${sample2}.L4.bwa.unique.sorted.bam
read9=${sample3}.L1.bwa.unique.sorted.bam
read10=${sample3}.L2.bwa.unique.sorted.bam
read11=${sample3}.L3.bwa.unique.sorted.bam
read12=${sample3}.L4.bwa.unique.sorted.bam
read13=${sample4}.L1.bwa.unique.sorted.bam
read14=${sample4}.L2.bwa.unique.sorted.bam
read15=${sample4}.L3.bwa.unique.sorted.bam
read16=${sample4}.L4.bwa.unique.sorted.bam
read17=${sample5}.L1.bwa.unique.sorted.bam
read18=${sample5}.L2.bwa.unique.sorted.bam
read19=${sample5}.L3.bwa.unique.sorted.bam
read20=${sample5}.L4.bwa.unique.sorted.bam
read21=${sample6}.L1.bwa.unique.sorted.bam
read22=${sample6}.L4.bwa.unique.sorted.bam
read23=${sample6}.L5.bwa.unique.sorted.bam
read24=${sample6}.L6.bwa.unique.sorted.bam


output_folder=/jic/scratch/groups/Chris-Ridout/hyalcin/private/BSA/Alingments/bwa/Unique_read_bam_files/unique_merged_files


r1=$input_folder/$read1
r2=$input_folder/$read2
r3=$input_folder/$read3
r4=$input_folder/$read4
r5=$input_folder/$read5
r6=$input_folder/$read6
r7=$input_folder/$read7
r8=$input_folder/$read8
r9=$input_folder/$read9
r10=$input_folder/$read10
r11=$input_folder/$read11
r12=$input_folder/$read12
r13=$input_folder/$read13
r14=$input_folder/$read14
r15=$input_folder/$read15
r16=$input_folder/$read16
r17=$input_folder/$read17
r18=$input_folder/$read18
r19=$input_folder/$read19
r20=$input_folder/$read20
r21=$input_folder/$read21
r22=$input_folder/$read22
r23=$input_folder/$read23
r24=$input_folder/$read24


out1=$output_folder/$sample1.unique.merged.bam
out2=$output_folder/$sample2.unique.merged.bam
out3=$output_folder/$sample3.unique.merged.bam
out4=$output_folder/$sample4.unique.merged.bam
out5=$output_folder/$sample5.unique.merged.bam
out6=$output_folder/$sample6.unique.merged.bam

echo $r1
echo $r2
echo $r3
echo $r4
echo $out1



samtools merge $out1 $r1 $r2 $r3 $r4


echo $r5
echo $r6
echo $r7
echo $r8
echo $out2


samtools merge $out2 $r5 $r6 $r7 $r8



echo $r9
echo $r10
echo $r11
echo $r12
echo $out3


samtools merge $out3 $r9 $r10 $r11 $r12



echo $r13
echo $r14
echo $r15
echo $r16
echo $out4


samtools merge $out4 $r13 $r14 $r15 $r16


echo $r17
echo $r18
echo $r19
echo $r20
echo $out5


samtools merge $out5 $r17 $r18 $r19 $r20



echo $r21
echo $r22
echo $r23
echo $r24
echo $out6


samtools merge $out6 $r21 $r22 $r23 $r24
```


