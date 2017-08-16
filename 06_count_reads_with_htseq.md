转录组入门(6)： reads计数
==========

实现这个功能的软件也很多，还是烦请大家先自己搜索几个教程，入门请统一用htseq-count，对每个样本都会输出一个表达量文件。
需要用脚本合并所有的样本为表达矩阵。参考：生信编程直播第四题：多个同样的行列式文件合并起来
对这个表达矩阵可以自己简单在excel或者R里面摸索，求平均值，方差。
看看一些生物学意义特殊的基因表现如何，比如GAPDH,β-ACTIN等等。

#安装

`conda install htseq`

#Usage: htseq-count [options] alignment_file gff_file  

`htseq-count -r pos -f bam mnt/e/dealing/SRR3589956.sorted.bam  mnt/e/dealing/data/reference/genome/m10/gencode.v26lift37.annotation.sorted.gtf > SRR3589956.count`



