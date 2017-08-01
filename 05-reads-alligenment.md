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

移除压缩包
`rm -rf *.tar.gz`

