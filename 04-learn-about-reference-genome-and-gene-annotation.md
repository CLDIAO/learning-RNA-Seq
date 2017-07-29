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

GFF/GTF 参照：http://www.ensembl.org/info/website/upload/gff.html

The GTF (General Transfer Format) is identical to GFF version 2.
Fields must be tab-separated. Also, all but the final field in each feature line must contain a value; "empty" columns should be denoted with a '.'

* seqname - name of the chromosome or scaffold; chromosome names can be given with or without the 'chr' prefix. Important note: the * seqname must be one used within Ensembl, i.e. a standard chromosome name or an Ensembl identifier such as a scaffold ID, without any additional content such as species or assembly. See the example GFF output below.
* source - name of the program that generated this feature, or the data source (database or project name)
* feature - feature type name, e.g. Gene, Variation, Similarity
* start - Start position of the feature, with sequence numbering starting at 1.
* end - End position of the feature, with sequence numbering starting at 1.
* score - A floating point value.
* strand - defined as + (forward) or - (reverse).
* frame - One of '0', '1' or '2'. '0' indicates that the first base of the feature is the first base of a codon, '1' that the second base is the first base of a codon, and so on..
* attribute - A semicolon-separated list of tag-value pairs, providing additional information about each feature.

GFF2弃用后使用GFF3：http://gmod.org/wiki/GFF3

两个都下载也是可以的，总共才100M左右吧：smile：

`nohup wget ftp://ftp.sanger.ac.uk/pub/gencode/Gencode_human/release_26/GRCh37_mapping/gencode.v26lift37.annotation.gtf.gz &`
`nohuop wget ftp://ftp.sanger.ac.uk/pub/gencode/Gencode_human/release_26/GRCh37_mapping/gencode.v26lift37.annotation.gff3.gz &`

下载基因组浏览器进行可视化。

IGV， Integrative Genomics Viewer
-------

下载地址为： http://software.broadinstitute.org/software/igv/download 

Windows下：
Download and unzip the zip archive, then double-click the "igv.bat" file to start IGV. A black console window will appear, followed by the IGV application. This option includes a modified Java executable for use with high-resolution screens.

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/8.jpg)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/9.JPG)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/10.JPG)


