转录组入门(7)： 差异基因分析
这个步骤推荐在R里面做，载入表达矩阵，然后设置好分组信息，统一用DEseq2进行差异分析，当然也可以走走edgeR或者limma的voom流程。
基本任务是得到差异分析结果，进阶任务是比较多个差异分析结果的异同点。

R包安装：试了几次没成功，提示不可读，就用管理员身份运行,而且需要较多的依赖包，在不同的软件库，可以逐个安装。

#try http:// if https:// URLs are not supported

>source("https://bioconductor.org/biocLite.R")

>biocLite("DESeq2")

1.构建读取表达矩阵
---
` options(stringsAsFactors = FALSE)`

`control1 <- read.table("D:/rna_seq/data/count/SRR3589959.count", sep = "\t", col.names = c("gene_id", "control1"))`

`control2 <- read.table("D:/rna_seq/data/count/SRR3589961.count", sep= "\t", col.names = c("gene_id", "control2"))`

`rep1 <- read.table("D:/rna_seq/data/count/SRR3589960.count", sep="\t", col.names = c("gene_id", "akap951"))`

`rep2 <- read.table("D:/rna_seq/data/count/SRR3589962.count", sep="\t", col.names = c("gene_id", "akap952"))`

#合并数据并删除无用行。

`raw_count <- merge(merge(control1, control2, by="gene_id"), merge(rep1,rep2, by="gene_id"))`

`raw_count_filt <- raw_count[-48823:-48825, ]`

`raw_count_filter <- raw_count_filt[-1:-5, ]`

`ENSEMBL <- gsub("\\.\\d*", "", raw_count_filter$gene_id)`

`row.names(raw_count_filter) <- ENSEMBL`

`raw_count_filter <- raw_count_filter[ ,-1]`

` write.table(raw_count_filter, file = "C:/Users/刁朝良/Desktop/SRR3.txt", sep = "\t")`

`read.table("C:/Users/刁朝良/Desktop/SRR3.txt")`

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/07/701.png)

2.构建dds对象
----
#DESeqDataSetFromMatrix(countData, colData, design, tidy = FALSE,
  ignoreRank = FALSE, ...)
* countData:	
for matrix input: a matrix of non-negative integers
* colData:
for matrix input: a DataFrame or data.frame with at least a single column. Rows of colData correspond to columns of countData
* design:
a formula which expresses how the counts for each gene depend on the variables in colData. Many R formula are valid, including designs with multiple variables, e.g., ~ group + condition, and designs with interactions, e.g., ~ genotype + treatment + genotype:treatment. See results for a variety of designs and how to extract results tables. By default, the functions in this package will use the last variable in the formula for building results tables and plotting. ~ 1 can be used for no design, although users need to remember to switch to another design for differential testing.
* tidy:
for matrix input: whether the first column of countData is the rownames for the count matrix

`condition <- factor(c(rep("control",2),rep("akap95",2)), levels = c("control","akap95"))`

`countData <- raw_count_filter[,1:4]`

`colData <- data.frame(row.names=colnames(raw_count_filter), condition)`

`dds <- DESeqDataSetFromMatrix(countData, colData, design= ~ condition)`

`head(dds)`

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/07/702.JPG)
