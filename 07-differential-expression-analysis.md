转录组入门(7)： 差异基因分析
这个步骤推荐在R里面做，载入表达矩阵，然后设置好分组信息，统一用DEseq2进行差异分析，当然也可以走走edgeR或者limma的voom流程。
基本任务是得到差异分析结果，进阶任务是比较多个差异分析结果的异同点。

R包安装：试了几次没成功，提示不可读，就用管理员身份运行：

#try http:// if https:// URLs are not supported

>source("https://bioconductor.org/biocLite.R")

>biocLite("DESeq2")

1.构建读取表达矩阵
---
` options(stringsAsFactors = FALSE)`

`control1 <- read.table("D:/rna_seq/data/count/SRR3589959.count", sep = "\t", col.names = c("gene_id", "control1"))`

`control2 <- read.table("D:/rna_seq/data/count/SRR3589961.count", sep= "\t", col.names = c("gene_id", "control2"))`

` rep1 <- read.table("D:/rna_seq/data/count/SRR3589960.count", sep="\t", col.names = c("gene_id", "akap951"))`

`rep2 <- read.table("D:/rna_seq/data/count/SRR3589962.count", sep="\t", col.names = c("gene_id", "akap952"))`

` raw_count <- merge(merge(control1, control2, by="gene_id"), merge(rep1,rep2, by="gene_id"))`

`raw_count_filt <- raw_count[-48823:-48825, ]`

` raw_count_filter <- raw_count_filt[-1:-5, ]`

` ENSEMBL <- gsub("\\.\\d*", "", raw_count_filter$gene_id)`

` row.names(raw_count_filter) <- ENSEMBL`

 `raw_count_filter <- raw_count_filter[ ,-1]`

` write.table(raw_count_filter, file = "C:/Users/刁朝良/Desktop/SRR3.txt", sep = "\t")`

`read.table("C:/Users/刁朝良/Desktop/SRR3.txt")`

2.构建dds对象
----
`condition <- factor(c(rep("control",2),rep("akap95",2)), levels = c("control","akap95"))`

`countData <- raw_count_filter[,1:4]`

`colData <- data.frame(row.names=colnames(raw_count_filter), condition)`

`dds <- DESeqDataSetFromMatrix(countData, colData, design= ~ condition)`

`head(dds)`
