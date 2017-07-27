这是RNA-Seq学习的第三篇
转录组入门(3)：了解fastq测序数据
需要用安装好的sratoolkit把sra文件转换为fastq格式的测序文件，并且用fastqc软件测试测序文件的质量！
作业，理解测序reads，GC含量，质量值，接头，index，fastqc的全部报告，搜索中文教程，并发在论坛上面。

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

结果有绿色的PASS，黄色的WARN，红色的FAIL。更多信息参考：
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
http://fbb84b26.wiz03.com/share/s/3XK4IC0cm4CL22pU-r1HPcQQ2irG2836uQYm2iZAyh1Zwf3_
https://mp.weixin.qq.com/s/1eaNhzj1R5pQgn7uy8Y7OA

脚本的话，可能还要慢慢修行的哦。github插入图片的我还是得再试试，毕竟一图胜千言！
