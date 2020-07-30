#Merging the Non-responsive & Responsive pools
In order to merge Pool_1-Pool_2 and Pool_3-Pool_4 according to their NLP-responsiveness; 


```sh
source samtools-1.9
```

to see the headers; 

```sh
samtools view -H Pool_1.rmdup.merged.bam | less
```
```
samtools view -H Pool_1.rmdup.merged.bam > Header_pool_1.txt
samtools view -H Pool_2.rmdup.merged.bam > Header_pool_2.txt
samtools view -H Pool_3.rmdup.merged.bam > Header_pool_3.txt
samtools view -H Pool_4.rmdup.merged.bam > Header_pool_4.txt
```

Then change the SM: (Sample name);

Pool_1 --> Non-responsive

Pool_2 --> Non-responsive

Pool_3 --> Responsive

Pool_4 --> Responsive

To Change the headers of the bam files (rehead them);

```
samtools reheader Header_pool_1.txt Pool_1.rmdup.merged.bam > Pool_1_reheaded_non_responsive.rmdup.merged.bam
samtools reheader Header_pool_2.txt Pool_2.rmdup.merged.bam > Pool_2_reheaded_non_responsive.rmdup.merged.bam
samtools reheader Header_pool_3.txt Pool_3.rmdup.merged.bam > Pool_3_reheaded_responsive.rmdup.merged.bam
samtools reheader Header_pool_4.txt Pool_4.rmdup.merged.bam > Pool_4_reheaded_responsive.rmdup.merged.bam
```
