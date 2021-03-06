#R final project
## Data Inspection
- read in data
  * snp <- read.table("snp_position.txt", header = TRUE, fill = TRUE)
  * fang <- read.table("fang_et_al_genotypes.txt", header = TRUE, fill = TRUE)
#### fang
- typeof(fang)
  * "list"
- class(fang)
  * "data.frame"
- str(fang)
  * 'data.frame':	2782 obs. of  986 variables:
- dim(fang)
  * 2782 rows 986 columns
#### snp_position
- typeof(snp)
  * "list"
- class(snp)
  * "data.frame"
- str(snp)
  * 'data.frame':	1017 obs. of  15 variables:
- dim(snp)
  * 1017 rows  15 columns
## Data Processing
#### Sorting Maize from Teosinte files
- Unique names in Group column
  * fnames <- unique(fang$Group)
- saved as characters
  * fnames <- as.character(unique(fang$Group))
- Pull out names for maize
  * fznames <- fnames[c(16, 14, 15)]
- Pull out names for teosinte
  * ftnames <- fnames[c(6, 12, 7)]
- Save rows that match maize names 
  * zfang <- fang[as.character(fang$Group) %in% fznames, ]
- Save rows that match teosinte names
  * tfang <- fang[as.character(fang$Group) %in% ftnames, ]
#### Maize file processing
- Save transposed maize file 
  * transz <- t(zfang)
- Save transposed maize file as a data frame with strings
  * transzdf <- as.data.frame(transz, stringsAsFactors = FALSE)
- Create column with row names of maize file
  * transzdf$sample_ID <- rownames(transzdf)
- !!!!
  * names(transzdf) <- as.character(transzdf[1,])
- Remove top 3 lines
  * transzdf <-  transzdf[-(1:3),]
- Merge snp file to maize file
  * zmerge <- merge(snp, transzdf, by.x = "SNP_ID", by.y = "Sample_ID")
- Reorder columns into SNP ID, Chromosome, postition
  * colzmerge <- zmerge [,c(1,3,4,2,5:1588)]
- Seperate files by chromosome
  * ZChr1 <- colzmerge[colzmerge$Chromosome == "1",]
  * ZChr2 <- colzmerge[colzmerge$Chromosome == "2",]
  * ZChr3 <- colzmerge[colzmerge$Chromosome == "3",]
  * ZChr4 <- colzmerge[colzmerge$Chromosome == "4",]
  * ZChr5 <- colzmerge[colzmerge$Chromosome == "5",]
  * ZChr6 <- colzmerge[colzmerge$Chromosome == "6",]
  * ZChr7 <- colzmerge[colzmerge$Chromosome == "7",]
  * ZChr8 <- colzmerge[colzmerge$Chromosome == "8",]
  * ZChr9 <- colzmerge[colzmerge$Chromosome == "9",]  
  * ZChr10 <- colzmerge[colzmerge$Chromosome == "10",] 
- Sort position column in each chromosome file as ascending
  * zChr1ascend <- ZChr1[order(as.numeric(as.character(ZChr1$Position))),]
  * zChr2ascend <- ZChr2[order(as.numeric(as.character(ZChr2$Position))),]
  * zChr3ascend <- ZChr3[order(as.numeric(as.character(ZChr3$Position))),]
  * zChr4ascend <- ZChr4[order(as.numeric(as.character(ZChr4$Position))),]  
  * zChr5ascend <- ZChr5[order(as.numeric(as.character(ZChr5$Position))),]  
  * zChr6ascend <- ZChr6[order(as.numeric(as.character(ZChr6$Position))),]  
  * zChr7ascend <- ZChr7[order(as.numeric(as.character(ZChr7$Position))),]  
  * zChr8ascend <- ZChr8[order(as.numeric(as.character(ZChr8$Position))),]  
  * zChr9ascend <- ZChr9[order(as.numeric(as.character(ZChr9$Position))),] 
  * zChr10ascend <- ZChr10[order(as.numeric(as.character(ZChr10$Position))),] 
- Sort position column in each chromosome file as descending
  * zChr1descend <- ZChr1[order(as.numeric(as.character(ZChr1$Position)) , decreasing = TRUE),]
  * zChr2descend <- ZChr2[order(as.numeric(as.character(ZChr2$Position)) , decreasing = TRUE),]
  * zChr3descend <- ZChr3[order(as.numeric(as.character(ZChr3$Position)) , decreasing = TRUE),]
  * zChr4descend <- ZChr4[order(as.numeric(as.character(ZChr4$Position)) , decreasing = TRUE),] 
  * zChr5descend <- ZChr5[order(as.numeric(as.character(ZChr5$Position)) , decreasing = TRUE),]
  * zChr6descend <- ZChr6[order(as.numeric(as.character(ZChr6$Position)) , decreasing = TRUE),]
  * zChr7descend <- ZChr7[order(as.numeric(as.character(ZChr7$Position)) , decreasing = TRUE),] 
  * zChr8descend <- ZChr8[order(as.numeric(as.character(ZChr8$Position)) , decreasing = TRUE),] 
  * zChr9descend <- ZChr9[order(as.numeric(as.character(ZChr9$Position)) , decreasing = TRUE),]
  * zChr10descend <- ZChr10[order(as.numeric(as.character(ZChr10$Position)) , decreasing = TRUE),]  
- Remove "?" and replace with "-" in descending files
  * zChr1descend[zChr1 == "?/?"] <- "-/-"
  * zChr2descend[zChr2 == "?/?"] <- "-/-"
  * zChr3descend[zChr3 == "?/?"] <- "-/-"
  * zChr4descend[zChr4 == "?/?"] <- "-/-"
  * zChr5descend[zChr5 == "?/?"] <- "-/-"
  * zChr6descend[zChr6 == "?/?"] <- "-/-"
  * zChr7descend[zChr7 == "?/?"] <- "-/-"
  * zChr8descend[zChr8 == "?/?"] <- "-/-"
  * zChr9descend[zChr9 == "?/?"] <- "-/-"
  * zChr10descend[zChr10 == "?/?"] <- "-/-"
- Remove "mulptile" SNP position rows by changing "multiple" to NA and then removing the NAs
  * zChr2ascend[zChr2ascend == "multiple"] <- NA
  * zChr2ascend <- zChr2ascend[!is.na(zChr2ascend$Position),]
  * zChr4ascend[zChr4ascend == "multiple"] <- NA
  * zChr4ascend <- zChr4ascend[!is.na(zChr4ascend$Position),]
  * zChr6ascend[zChr6ascend == "multiple"] <- NA
  * zChr6ascend <- zChr6ascend[!is.na(zChr6ascend$Position),]
  * zChr7ascend[zChr7ascend == "multiple"] <- NA
  * zChr7ascend <- zChr7ascend[!is.na(zChr7ascend$Position),]
  * zChr9ascend[zChr9ascend == "multiple"] <- NA
  * zChr9ascend <- zChr9ascend[!is.na(zChr9ascend$Position),]
  * zChr2descend[zChr2descend == "multiple"] <- NA
  * zChr2descend <- zChr2descend[!is.na(zChr2descend$Position),]
  * zChr4descend[zChr4descend == "multiple"] <- NA
  * zChr4descend <- zChr4descend[!is.na(zChr4descend$Position),]
  * zChr6descend[zChr6descend == "multiple"] <- NA
  * zChr6descend <- zChr6descend[!is.na(zChr6descend$Position),]
  * zChr7descend[zChr7descend == "multiple"] <- NA
  * zChr7descend <- zChr7descend[!is.na(zChr7descend$Position),]
  * zChr9descend[zChr9descend == "multiple"] <- NA
  * zChr9descend <- zChr9descend[!is.na(zChr9descend$Position),]
  - Remove columns that are not SNP ID, chromosome, position, and genotype columns
zChr1ascend <- zChr1ascend[, -c(4:15)]
zChr2ascend <- zChr2ascend[, -c(4:15)]
zChr3ascend <- zChr3ascend[, -c(4:15)]
zChr4ascend <- zChr4ascend[, -c(4:15)]
zChr5ascend <- zChr5ascend[, -c(4:15)]
zChr6ascend <- zChr6ascend[, -c(4:15)]
zChr7ascend <- zChr7ascend[, -c(4:15)]
zChr8ascend <- zChr8ascend[, -c(4:15)]
zChr9ascend <- zChr9ascend[, -c(4:15)]
zChr10ascend <- zChr10ascend[, -c(4:15)]
zChr1descend <- zChr1descend[, -c(4:15)]
zChr2descend <- zChr2descend[, -c(4:15)]
zChr3descend <- zChr3descend[, -c(4:15)]
zChr4descend <- zChr4descend[, -c(4:15)]
zChr5descend <- zChr5descend[, -c(4:15)]
zChr6descend <- zChr6descend[, -c(4:15)]
zChr7descend <- zChr7descend[, -c(4:15)]
zChr8descend <- zChr8descend[, -c(4:15)]
zChr9descend <- zChr9descend[, -c(4:15)]
zChr10descend <- zChr10descend[, -c(4:15)]
- Create text files for each chromosome with ascending and descending SNP positions
write.table(zChr1ascend, "Chromosome1_Maize_ascending.txt", sep = "\t")
write.table(zChr2ascend, "Chromosome2_Maize_ascending.txt", sep = "\t")
write.table(zChr3ascend, "Chromosome3_Maize_ascending.txt", sep = "\t")
write.table(zChr4ascend, "Chromosome4_Maize_ascending.txt", sep = "\t")
write.table(zChr5ascend, "Chromosome5_Maize_ascending.txt", sep = "\t")
write.table(zChr6ascend, "Chromosome6_Maize_ascending.txt", sep = "\t")
write.table(zChr7ascend, "Chromosome7_Maize_ascending.txt", sep = "\t")
write.table(zChr8ascend, "Chromosome8_Maize_ascending.txt", sep = "\t")
write.table(zChr9ascend, "Chromosome9_Maize_ascending.txt", sep = "\t")
write.table(zChr10ascend, "Chromosome10_Maize_ascending.txt", sep = "\t")
write.table(zChr1descend, "Chromosome1_Maize_descending.txt", sep = "\t")
write.table(zChr2descend, "Chromosome2_Maize_descending.txt", sep = "\t")
write.table(zChr3descend, "Chromosome3_Maize_descending.txt", sep = "\t")
write.table(zChr4descend, "Chromosome4_Maize_descending.txt", sep = "\t")
write.table(zChr5descend, "Chromosome5_Maize_descending.txt", sep = "\t")
write.table(zChr6descend, "Chromosome6_Maize_descending.txt", sep = "\t")
write.table(zChr7descend, "Chromosome7_Maize_descending.txt", sep = "\t")
write.table(zChr8descend, "Chromosome8_Maize_descending.txt", sep = "\t")
write.table(zChr9descend, "Chromosome9_Maize_descending.txt", sep = "\t")
write.table(zChr10descend, "Chromosome10_Maize_descending.txt", sep = "\t")

  
#### Teosinte file processing
- Save transposed teosinte file 
  * transt <- t(tfang)
- Save transposed teosinte file as a data frame with strings
  * transtdf <- as.data.frame(transt, stringsAsFactors = FALSE)
- Create column with row names of teosinte file
  * transtdf$sample_ID <- rownames(transtdf)
- !!!!
  * names(transtdf) <- as.character(transtdf[1,])
- Remove top 3 lines
  * transtdf <-  transtdf[-(1:3),]
- Merge snp file to teosinte file
  * tmerge <- merge(snp, transtdf, by.x = "SNP_ID", by.y = "Sample_ID")
- Reorder columns into SNP ID, Chromosome, postition
  * coltmerge <- tmerge [,c(1,3,4,2,5:990)]
- Seperate files by chromosome
  * tChr1 <- coltmerge[coltmerge$Chromosome == "1",]
  * tChr2 <- coltmerge[coltmerge$Chromosome == "2",]
  * tChr3 <- coltmerge[coltmerge$Chromosome == "3",]
  * tChr4 <- coltmerge[coltmerge$Chromosome == "4",]
  * tChr5 <- coltmerge[coltmerge$Chromosome == "5",]
  * tChr6 <- coltmerge[coltmerge$Chromosome == "6",]
  * tChr7 <- coltmerge[coltmerge$Chromosome == "7",]
  * tChr8 <- coltmerge[coltmerge$Chromosome == "8",]
  * tChr9 <- coltmerge[coltmerge$Chromosome == "9",]  
  * tChr10 <- coltmerge[coltmerge$Chromosome == "10",] 
- Sort position column in each chromosome file as ascending
  * tChr1ascend <- tChr1[order(as.numeric(as.character(tChr1$Position))),]
  * tChr2ascend <- tChr2[order(as.numeric(as.character(tChr2$Position))),]
  * tChr3ascend <- tChr3[order(as.numeric(as.character(tChr3$Position))),]
  * tChr4ascend <- tChr4[order(as.numeric(as.character(tChr4$Position))),]  
  * tChr5ascend <- tChr5[order(as.numeric(as.character(tChr5$Position))),]  
  * tChr6ascend <- tChr6[order(as.numeric(as.character(tChr6$Position))),]  
  * tChr7ascend <- tChr7[order(as.numeric(as.character(tChr7$Position))),]  
  * tChr8ascend <- tChr8[order(as.numeric(as.character(tChr8$Position))),]  
  * tChr9ascend <- tChr9[order(as.numeric(as.character(tChr9$Position))),] 
  * tChr10ascend <- tChr10[order(as.numeric(as.character(tChr10$Position))),] 
- Sort position column in each chromosome file as descending
  * tChr1descend <- tChr1[order(as.numeric(as.character(tChr1$Position)) , decreasing = TRUE),]
  * tChr2descend <- tChr2[order(as.numeric(as.character(tChr2$Position)) , decreasing = TRUE),]
  * tChr3descend <- tChr3[order(as.numeric(as.character(tChr3$Position)) , decreasing = TRUE),]
  * tChr4descend <- tChr4[order(as.numeric(as.character(tChr4$Position)) , decreasing = TRUE),] 
  * tChr5descend <- tChr5[order(as.numeric(as.character(tChr5$Position)) , decreasing = TRUE),]
  * tChr6descend <- tChr6[order(as.numeric(as.character(tChr6$Position)) , decreasing = TRUE),]
  * tChr7descend <- tChr7[order(as.numeric(as.character(tChr7$Position)) , decreasing = TRUE),] 
  * tChr8descend <- tChr8[order(as.numeric(as.character(tChr8$Position)) , decreasing = TRUE),] 
  * tChr9descend <- tChr9[order(as.numeric(as.character(tChr9$Position)) , decreasing = TRUE),]
  * tChr10descend <- tChr10[order(as.numeric(as.character(tChr10$Position)) , decreasing = TRUE),] 
- Remove "?" and replace with "-" in descending files
  * tChr1descend[tChr1 == "?/?"] <- "-/-"
  * tChr2descend[tChr2 == "?/?"] <- "-/-"
  * tChr3descend[tChr3 == "?/?"] <- "-/-"
  * tChr4descend[tChr4 == "?/?"] <- "-/-"
  * tChr5descend[tChr5 == "?/?"] <- "-/-"
  * tChr6descend[tChr6 == "?/?"] <- "-/-"
  * tChr7descend[tChr7 == "?/?"] <- "-/-"
  * tChr8descend[tChr8 == "?/?"] <- "-/-"
- Remove "mulptile" SNP position rows by changing "multiple" to NA and then removing the NAs
  * tChr2ascend[tChr2ascend == "multiple"] <- NA
  * tChr2ascend <- tChr2ascend[!is.na(tChr2ascend$Position),]
  * tChr4ascend[tChr4ascend == "multiple"] <- NA
  * tChr4ascend <- tChr4ascend[!is.na(tChr4ascend$Position),]\
  * tChr7ascend[tChr7ascend == "multiple"] <- NA
  * tChr7ascend <- tChr7ascend[!is.na(tChr7ascend$Position),]
  * tChr9ascend[tChr9ascend == "multiple"] <- NA
  * tChr9ascend <- tChr9ascend[!is.na(tChr9ascend$Position),]
  * tChr2descend[tChr2descend == "multiple"] <- NA
tChr2descend <- tChr2descend[!is.na(tChr2descend$Position),]
tChr2descend <- tChr2descend[!is.na(tChr2descend$Position),]
tChr4descend[tChr4descend == "multiple"] <- NA
tChr4descend <- tChr4descend[!is.na(tChr4descend$Position),]
tChr7descend[tChr7descend == "multiple"] <- NA
tChr7descend <- tChr7descend[!is.na(tChr7descend$Position),]
tChr9descend[tChr9descend == "multiple"] <- NA
tChr9descend <- tChr9descend[!is.na(tChr9descend$Position),]
- Remove columns that are not SNP ID, chromosome, position, and genotype columns
tChr1ascend <- tChr1ascend[, -c(4:15)]
tChr2ascend <- tChr2ascend[, -c(4:15)]
tChr3ascend <- tChr3ascend[, -c(4:15)]
tChr4ascend <- tChr4ascend[, -c(4:15)]
tChr5ascend <- tChr5ascend[, -c(4:15)]
tChr6ascend <- tChr6ascend[, -c(4:15)]
tChr7ascend <- tChr7ascend[, -c(4:15)]
tChr8ascend <- tChr8ascend[, -c(4:15)]
tChr9ascend <- tChr9ascend[, -c(4:15)]
tChr10ascend <- tChr10ascend[, -c(4:15)]
tChr1descend <- tChr1descend[, -c(4:15)]
tChr2descend <- tChr2descend[, -c(4:15)]
tChr3descend <- tChr3descend[, -c(4:15)]
tChr4descend <- tChr4descend[, -c(4:15)]
tChr5descend <- tChr5descend[, -c(4:15)]
tChr6descend <- tChr6descend[, -c(4:15)]
tChr7descend <- tChr7descend[, -c(4:15)]
tChr8descend <- tChr8descend[, -c(4:15)]
tChr9descend <- tChr9descend[, -c(4:15)]
tChr10descend <- tChr10descend[, -c(4:15)]

- Create text files for each chromosome with ascending and descending SNP positions
write.table(tChr1ascend, "Chromosome1_Teosinte_ascending.txt", sep = "\t")
write.table(tChr2ascend, "Chromosome2_Teosinte_ascending.txt", sep = "\t")
write.table(tChr3ascend, "Chromosome3_Teosinte_ascending.txt", sep = "\t")
write.table(tChr4ascend, "Chromosome4_Teosinte_ascending.txt", sep = "\t")
write.table(tChr5ascend, "Chromosome5_Teosinte_ascending.txt", sep = "\t")
write.table(tChr6ascend, "Chromosome6_Teosinte_ascending.txt", sep = "\t")
write.table(tChr7ascend, "Chromosome7_Teosinte_ascending.txt", sep = "\t")
write.table(tChr8ascend, "Chromosome8_Teosinte_ascending.txt", sep = "\t")
write.table(tChr9ascend, "Chromosome9_Teosinte_ascending.txt", sep = "\t")
write.table(tChr10ascend, "Chromosome10_Teosinte_ascending.txt", sep = "\t")
write.table(tChr1descend, "Chromosome1_Teosinte_descending.txt", sep = "\t")
write.table(tChr2descend, "Chromosome2_Teosinte_descending.txt", sep = "\t")
write.table(tChr3descend, "Chromosome3_Teosinte_descending.txt", sep = "\t")
write.table(tChr4descend, "Chromosome4_Teosinte_descending.txt", sep = "\t")
write.table(tChr5descend, "Chromosome5_Teosinte_descending.txt", sep = "\t")
write.table(tChr6descend, "Chromosome6_Teosinte_descending.txt", sep = "\t")
write.table(tChr7descend, "Chromosome7_Teosinte_descending.txt", sep = "\t")
write.table(tChr8descend, "Chromosome8_Teosinte_descending.txt", sep = "\t")
write.table(tChr9descend, "Chromosome9_Teosinte_descending.txt", sep = "\t")
write.table(tChr10descend, "Chromosome10_Teosinte_descending.txt", sep = "\t")



## Data Analysis 