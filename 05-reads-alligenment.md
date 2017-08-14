转录组入门(5)： 序列比对
比对软件很多，首先大家去收集一下，因为我们是带大家入门，请统一用hisat2，并且搞懂它的用法。
直接去hisat2的主页下载index文件即可，然后把fastq格式的reads比对上去得到sam文件。
接着用samtools把它转为bam文件，并且排序(注意N和P两种排序区别)索引好，载入IGV，再截图几个基因看看！
顺便对bam文件进行简单QC，参考直播我的基因组系列。

hisat2:https://ccb.jhu.edu/software/hisat2/index.shtml

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5/1.JPG)

获取hg19和mm10的index。

`cd reference && mkdir index && cd index`

`axel -a ftp://ftp.ccb.jhu.edu/pub/infphilo/hisat2/data/hg19.tar.gz`

`axel -a ftp://ftp.ccb.jhu.edu/pub/infphilo/hisat2/data/mm10.tar.gz`

#解压缩文件

`tar -zxvf *.tar.gz`

#移除压缩包
`rm -rf *.tar.gz`

hisat2比对
-----------

基本用法：` hisat2 [options]* -x <ht2-idx> {-1 <m1> -2 <m2> | -U <r> | --sra-acc <SRA accession number>} [-S <sam>]`

* <ht2-idx>  Index filename prefix (minus trailing .X.ht2).                                                                
* <m1>       Files with #1 mates, paired with files in <m2>.                                                                                            Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).                                              
* <m2>       Files with #2 mates, paired with files in <m1>.                                                                            
            Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).                                             
* <r>        Files with unpaired reads.                                                                                                
           Could be gzip'ed (extension: .gz) or bzip2'ed (extension: .bz2).                                             
* <SRA accession number>        Comma-separated list of SRA accession numbers, e.g. --sra-acc SRR353653,SRR353654.        
* <sam>      File for SAM output (default: stdout)                                                                                       
<m1>, <m2>, <r> can be comma-separated lists (no whitespace) and can be specified many times.  E.g. '-U file1.fq,file2.fq -U file3.fq'.

RNA-Seq数据中，56、57、58是homo，60、61、62是mus，所以需要分别进行比对：

`mkdir aligned && cd aligned`

>for i in `seq 56 58· `

`do`

`hisat2 -t -x /mnt/d/rna_seq/data/reference/index/hg19/genome -1 /mnt/d/rna_seq/data/SRR358994{i}_1.fastq.gz -2 /mnt/d/rna_seq/data/SRR35899${i}_2.fastq.gz -S /mnt/d/rna_seq/aligned/SRR35899${i}.sam &`

`done`

>for i in `seq 59 62` 

`do `

`hisat2 -t -x /mnt/d/rna_seq/data/reference/index/mm10/genome -1 /mnt/d/rna_seq/data/SRR358994{i}_1.fastq.gz -2 /mnt/d/rna_seq/data/SRR35899${i}_2.fastq.gz -S /mnt/d/rna_seq/aligned/SRR35899${i}.sam &`

`done`

这样会得到7个.sam文件(笔记本能力有限，让同学帮跑的)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5/30.png)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5/31.png)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5/32.png)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5/33.png)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5/34.png)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5/35.png)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5/36.png)

samtools的使用
---------

SAM(sequence alignment/mapping)数据是目前高通量测序中存放对比数据的标准格式。目前处理SAM格式的工具主要是SAMtools。

`samtools --help`:最常用的就是格式转换`view`、排序`sort`、索引`index`。

`view`命令参数 -S 输入sam文件， -b 输出文件为bam。之后重定向写入bam文件。

`for ((i=56;i<=62;i++));do samtools view -S SRR35899${i}.sam -b > SRR35899${i}.bam;done`

`for ((i=56;i<=62;i++));do samtools sort SRR35899${i}.bam -o SRR35899${i}.sorted.bam;done`

`for ((i=56;i<=62;i++));do samtools index SRR35899${i}_sorted.bam;done`



