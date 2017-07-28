这是RNA-Seq学习的第三篇
转录组入门(3)：了解fastq测序数据
需要用安装好的sratoolkit把sra文件转换为fastq格式的测序文件，并且用fastqc软件测试测序文件的质量！
作业，理解测序reads，GC含量，质量值，接头，index，fastqc的全部报告，搜索中文教程，并发在论坛上面。

看在前面：https://zhuanlan.zhihu.com/p/20714540

在第二篇中已经下载了SRR3589956-62的*.sra数据。要把.sra转换为.fastq,要使用的工具是`fastq-dump`,首先查看帮助：
* INPUT：-A|--accession 序列号
* PROCESS：Read Splitting, Full Spot Filters, Common Filters, Filters based on alignments, Filters for individual reads。 基本都是些过滤参数。不太常用
* OUTPUT：-O|--outdir输出文件夹；-Z|--sudout输出到标准输出；--gzip/--bzip输出为压缩格式（实践证明空间占用差六倍左右）
* Multiple File Options ：常用--split-3
所以用法就是：`fastq-dump --gzip --split-3 *.sra`。
因为本人生物学出身，对编程了解太少，做法就是创建一个文件夹处理相应的数据，这样省事，却不利于学习啊。

` SRR3589960_1.fastq.gz  SRR3589960_2.fastq.gz`然后就会将每个.sra转换为两个.fastq.gz。
压缩的文件不需要解压就可以查看的，如zcat，zgrep等。

`zcat SRR3589956_1.fastq.gz | head -4` 可以看到如下四行：

`@SRR3589956.1 D5VG2KN1:224:C4VAYACXX:5:1101:1159:2173 length=51`

`GGCGAGTGTAGGGCTGGCGCTGCCGGACGCGGTGCTAGTCGCCGGATGAAG`
   
`+SRR3589956.1 D5VG2KN1:224:C4VAYACXX:5:1101:1159:2173 length=51`
  
`B<BFBFBF0BFFFBFFBBFFIF<FFI<7<<BF<FFFFFFBB<BBBBBBBBB `

fastq文件每个序列通常有四行，分别是：
* Line 1 begins with a '@' character and is followed by a sequence identifier and an optional description (like a FASTA title line).
* Line 2 is the raw sequence letters.
* Line 3 begins with a '+' character and is optionally followed by the same sequence identifier (and any description) again.
* Line 4 encodes the quality values for the sequence in Line 2, and must contain the same number of symbols as letters in the sequence.

 FastQC - A high throughput sequence QC analysis tool，高通量测序质量控制分析工具。
 FastQC reads a set of sequence files and produces from each one a quality control report consisting of a number of different modules, each one of which will help to identify a different potential type of problem in your data.    
 If no files to process are specified on the command line then the program will start as an interactive graphical application.If files are provided on the command line then the program will run with no user interaction required.  In this mode it is suitable for inclusion into a standardised analysis pipeline.
 
 选项：
 * -o:输出途径
 * --extract：输出文件是否需要自动解压，默认是--nonextract
 * -t:线程：每个线程需要250M内存
 * -c:测序中可能会有污染，比如混入其它物种
 * -a:接头
 * -q:安静模式
 
 FastQC两种方式分析压缩的fastq文件：
 `zcat SRR3589956_1.fastq.gz | fastqc -t 4 stdin` 
 或者 `fastqc SRR3589956_1.fastq.gz`


` SRR3589956_1_fastqc.html  SRR3589958_2_fastqc.html  SRR3589961_1_fastqc.html    SRR3589956_1_fastqc.zip   SRR3589958_2_fastqc.zip   SRR3589961_1_fastqc.zip     SRR3589956_1.fastq.gz     SRR3589958_2.fastq.gz     SRR3589961_1.fastq.gz       SRR3589956_2_fastqc.html  SRR3589959_1_fastqc.html  SRR3589961_2_fastqc.html    SRR3589956_2_fastqc.zip   SRR3589959_1_fastqc.zip   SRR3589961_2_fastqc.zip     SRR3589956_2.fastq.gz     SRR3589959_1.fastq.gz     SRR3589961_2.fastq.gz       SRR3589957_1_fastqc.html  SRR3589959_2_fastqc.html  SRR3589962_1_fastqc.html    SRR3589957_1_fastqc.zip   SRR3589959_2_fastqc.zip   SRR3589962_1_fastqc.zip     SRR3589957_1.fastq.gz     SRR3589959_2.fastq.gz     SRR3589962_1.fastq.gz       SRR3589957_2_fastqc.html  SRR3589960_1_fastqc.html  SRR3589962_2_fastqc.html    SRR3589957_2_fastqc.zip   SRR3589960_1_fastqc.zip   SRR3589962_2_fastqc.zip     SRR3589957_2.fastq.gz     SRR3589960_1.fastq.gz     SRR3589962_2.fastq.gz       SRR3589958_1_fastqc.html  SRR3589960_2_fastqc.html  SRR_Acc_List.txt            SRR3589958_1_fastqc.zip   SRR3589960_2_fastqc.zip   work                        SRR3589958_1.fastq.gz     SRR3589960_2.fastq.gz `

然后发现每个fastq.gz对应一个.zip和一个.html。.html可以直接用浏览器打开。

![image](http://upload-images.jianshu.io/upload_images/2013053-9aa706f4aca9d37b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

结果有绿色的PASS，黄色的WARN，红色的FAIL。
 
 basic statistics
* Encoding指测序平台的版本和相应的编码版本号，这个在计算Phred反推error P的时候有用，如果不明白可以参考之前的文章。
* Total Sequences记录了输入文本的reads的数量
* Sequence length 是测序的长度
* %GC 是我们需要重点关注的一个指标，这个值表示的是整体序列中的GC含量，这个数值一般是物种特意的，比如人类细胞就是42%左右。

per base sequence quality
* 横轴是测序的碱基序列
 
* 纵轴是质量得分，Q = -10 * log10（error P）即20表示1%的错误率，30表示0.1%

* 图中每1个boxplot，都是该位置的所有序列的测序质量的一个统计，上面的bar是90%分位数，下面的bar是10%分位数，箱子的中间的横线是50%分位数，箱子的上边是75%分位数，下边是25%分位数

* 图中蓝色的细线是各个位置的平均值的连线

* 一般此图要求所有位置的10%分位数大于20，即Q20过滤

* warning：有任何碱基质量低于10，或任何中位数低于25

* failure：如果任何碱基质量低于5，或者任何中位数低于20

per tile sequece quality:

* 横轴表示碱基序列，纵轴表示tile的index编号

* 蓝色表示测序质量高，暖色表示质量不高，可以在后续分析中把这些去除

per sequence quality scores

* 序列每个碱基位置Q的平均值是这条reads的质量值

* 横轴表示Q 值

* 纵轴表示每个值对应的reads数目

* 数据中，测序结果集中在高分中，证明测序质量良好。

per base sequence content

* 横轴是碱基序列，纵轴是百分比

* 四条线表示ATGC在没个位子的平均含量

* 理论上A=T,G=C，一般测序开始时测序仪不稳定，前一部分需要cut

per sequence GC content

* 横轴0-100%，纵轴是每条序列GC含量对应的数量

* 蓝色线是理论值，红色线是真实值，二者应该比较接近

* 红色出现双峰的话肯定混入其他物种DNA序列

sequence length distribution

* 长度理论上应该完全相等，但总会有偏差，如果严重的话，测序数据不可信

adapter content

* 衡量序列中两端adapter的情况，分析前是要去adapter的

kmer content

* 统计序列中某些特征的短序列重复出现的次数，可能是adapter没有去处干净，可以cut


更多信息参考：
http://yanshouyu.blog.163.com/blog/static/214283182201302835744453/ 

http://jingyan.baidu.com/article/49711c6149e27dfa441b7c34.html 

https://zhuanlan.zhihu.com/p/20731723

为了避免一个个打开.html的麻烦，可以使用multiQC工具。

参考：http://fbb84b26.wiz03.com/share/s/3XK4IC0cm4CL22pU-r1HPcQQ1iRTvV2GwkwL2AaxYi2fXHP7

`conda install -c bioconda multiqc`安装方便。

`multiqc.`进行测试。

`multiqc *fastqc.zip --pdf` 处理我们的结果。

`multiqc_report.html ` 得到一个.html。同样在浏览器打开。

这一部分主要学习三位师兄师姐的教程，表示感谢。
http://www.biotrainee.com/thread-1831-1-1.html

http://fbb84b26.wiz03.com/share/s/3XK4IC0cm4CL22pU-r1HPcQQ2irG2836uQYm2iZAyh1Zwf3_  （师兄这链接最后是个下划线，读不出来了：|）

https://mp.weixin.qq.com/s/1eaNhzj1R5pQgn7uy8Y7OA

脚本的话，可能还要慢慢修行的哦。github插入图片的我还是得再试试，毕竟一图胜千言！
