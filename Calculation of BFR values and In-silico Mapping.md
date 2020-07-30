# Calculation of BFR & *In-silico* Mapping

```sh
Q2000_parental_filtered_merged_pools <- read.table(gzfile("/Volumes/LaCie/BSA/SNP_calls/freebayes/Unique_map_chr_2nd_call_merged_pools/concatenated/All_concat_merged_pools_rmdup_unique_bwa_filter_Q2000_D20_Parent.txt"), header = F, stringsAsFactors = F)

```

For putting the proper header;

```sh
colnames(Q2000_parental_filtered_merged_pools)


names(Q2000_parental_filtered_merged_pools) <- c("CHROM", "POS", "REF", "ALT", "QUAL", "TYPE", "SAMPLE", "GT", "RO", "AO", "POS0", "DP")

```

Define them as numeric;

```sh
Q2000_parental_filtered_merged_pools$AO <- as.numeric(Q2000_parental_filtered_merged_pools$AO)

Q2000_parental_filtered_merged_pools$RO <- as.numeric(Q2000_parental_filtered_merged_pools$RO)

Q2000_parental_filtered_merged_pools$DP <- as.numeric(Q2000_parental_filtered_merged_pools$DP)
```

```sh
library(data.table)
```


```dcast```; this script transforms the table to get the all the related information each SNP (based on its position) on the same row 





```sh
Q2000_parental_filtered_merged_pools_dcast <- dcast(setDT(Q2000_parental_filtered_merged_pools), CHROM + POS + REF + ALT + QUAL +TYPE + POS0 ~ SAMPLE, value.var = c("RO", "AO", "DP"))


Q2000_parental_filtered_merged_pools <- NULL



Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast <- Q2000_parental_filtered_merged_pools_dcast[,c("CHROM", "POS", "REF", "ALT", "QUAL", "POS0", "RO_Pool_Non_responsive", "AO_Pool_Non_responsive", "DP_Pool_Non_responsive", "RO_Pool_Responsive", "AO_Pool_Responsive", "DP_Pool_Responsive")]
```

Calculation of the BFR values. The formula for each position is:

$R_{index}=\frac{R0}{DP}$

$A_{index}=\frac{A0}{DP}$

$BFR_{responsive}=\frac{R_{index}}{A_{index}} $

$BFR_{nonResponsive}=\frac{A_{index}}{R_{index}} $



```sh

Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Pool_Non_responsive_index_AO <- Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$AO_Pool_Non_responsive / Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$DP_Pool_Non_responsive

Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Pool_Responsive_index_AO <- Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$AO_Pool_Responsive / Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$DP_Pool_Responsive

Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Pool_Non_responsive_index_RO <- Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$RO_Pool_Non_responsive / Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$DP_Pool_Non_responsive

Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Pool_Responsive_index_RO <- Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$RO_Pool_Responsive / Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$DP_Pool_Responsive

Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Bulk_ratio_AO <- Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Pool_Responsive_index_AO / Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Pool_Non_responsive_index_AO

Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Bulk_ratio_RO <- Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Pool_Responsive_index_RO / Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$Pool_Non_responsive_index_RO

```

To only plot chromosomes;

```sh

All_chr_plot <- c("chrA01", "chrA02", "chrA03", "chrA04", "chrA05", "chrA06", "chrA07", "chrA08", "chrA09", "chrA10", "chrC01", "chrC02", "chrC03", "chrC04", "chrC05", "chrC06", "chrC07", "chrC08", "chrC09")

Plot_Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast <- Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast[Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast$CHROM %in% All_chr_plot, ]
```


# Manhattan plots

To create the Manhattan plots represented in Figure 4.12;

```sh
library(ggplot2)
```





```sh

Plot_ol_AO <- ggplot(Plot_Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast, aes(x=POS, y=Bulk_ratio_AO)) + geom_point(aes(color=CHROM)) + facet_wrap(~CHROM, nrow = 1, scales = "free_x") + theme(axis.text.y = element_text(size = 20) , strip.text = element_text(size = 25, face = "bold"), axis.title.y = element_text(size = 20, face = "bold"), axis.title.x = element_text(size = 20, face = "bold"), legend.title = element_text(size = 18, face = "bold"), legend.text = element_text(size = 15)) 

ggsave(Plot_ol_AO, file="/Users/hyalcin/Desktop/R_markdown/Merged_pools/BFRs_Responsive_Nonresponsive/Parental_Q2000/BFR_AO_Pool_Responsive_Non_responsive_Plot_parental_Q2000_arranged_filtered.png", width=50, height=12, limitsize = FALSE)




Plot_ol_RO <- ggplot(Plot_Pool_Responsive_Non_responsive_Q2000_parental_filtered_merged_pools_dcast, aes(x=POS, y=Bulk_ratio_RO)) + geom_point(aes(color=CHROM)) + facet_wrap(~CHROM, nrow = 1, scales = "free_x") + theme(axis.text.y = element_text(size = 20) , strip.text = element_text(size = 25, face = "bold"), axis.title.y = element_text(size = 20, face = "bold"), axis.title.x = element_text(size = 20, face = "bold"), legend.title = element_text(size = 18, face = "bold"), legend.text = element_text(size = 15)) 

ggsave(Plot_ol_RO, file="/Users/hyalcin/Desktop/R_markdown/Merged_pools/BFRs_Responsive_Nonresponsive/Parental_Q2000/BFR_RO_Pool_Responsive_Non_responsive_Plot_parental_Q2000_arranged_filtered.png", width=50, height=12, limitsize = FALSE)





```



