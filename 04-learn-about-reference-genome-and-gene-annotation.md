转录组入门(4)：了解参考基因组及基因注释
在UCSC下载hg19参考基因组，我博客有详细说明，从gencode数据库下载基因注释文件，并且用IGV去查看你感兴趣的基因的结构，比如TP53,KRAS,EGFR等等。
作业，截图几个基因的IGV可视化结构！还可以下载ENSEMBL，NCBI的gtf，也导入IGV看看，截图基因结构。了解IGV常识。

参考基因组
=======
在UCSC下载hg19参考基因组: http://genome.ucsc.edu/index.html
-----

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/1.jpg)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/2.jpg)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/3.jpg)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/4.jpg)

`chromFa.tar.gz - The assembly sequence in one file per chromosome.
    Repeats from RepeatMasker and Tandem Repeats Finder (with period
    of 12 or less) are shown in lower case; non-repeating sequence is
    shown in upper case.`

参考基因组介绍：http://www.bio-info-trainee.com/1985.html

`cd /mnt/d/rna_seq/data`

`mkdir reference && cd reference`

`mkdir -p genome/hg19 && cd genome/hg19`

`nohup wget http://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/chromFa.tar.gz &` #这一步我直接在网站上点击下载了，再移入相应文件夹内。`nohup`静默下载。

`tar -zvf chromFa.tar.gz` #解压

`cat *.fa > hg19.fa` #`cat > `表示将文件合并为

`rm chr*`

注释信息
------
参考基因组需要注释文件来解读，下载基因组注释文件：http://www.gencodegenes.org/

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/5.jpg)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/6.jpg)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/7.jpg)
