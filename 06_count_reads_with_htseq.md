转录组入门(6)： reads计数
==========

实现这个功能的软件也很多，还是烦请大家先自己搜索几个教程，入门请统一用htseq-count，对每个样本都会输出一个表达量文件。
需要用脚本合并所有的样本为表达矩阵。参考：生信编程直播第四题：多个同样的行列式文件合并起来
对这个表达矩阵可以自己简单在excel或者R里面摸索，求平均值，方差。
看看一些生物学意义特殊的基因表现如何，比如GAPDH,β-ACTIN等等。

#安装

`conda install htseq`

#Usage: htseq-count [options] alignment_file gff_file  

`htseq-count -r pos -f bam /mnt/e/dealing/SRR3589956.sorted.bam  /mnt/e/dealing/data/reference/genome/hg19/gencode.v26lift37.annotation.sorted.gtf > SRR3589956.count`

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/06/61.JPG)

* -f bam/sam： 指定输入文件格式，默认SAM
* -r name/pos: 你需要利用samtool sort对数据根据read name或者位置进行排序，默认是name
* -s yes/no/reverse: 数据是否来自于strand-specific assay。DNA是双链的，所以需要判断到底来自于哪条链。如果选择了no， 那么每一条read都会跟正义链和反义链进行比较。默认的yes对于双端测序表示第一个read都在同一个链上，第二个read则在另一条链上。
* -a 最低质量， 剔除低于阈值的read
* -m 模式 union（默认）, intersection-strict and intersection-nonempty。一般而言就用默认的，作者也是这样认为的。
* -i id attribute: 在GTF文件的最后一栏里，会有这个基因的多个命名方式（如下）， RNA-Seq数据分析常用的是gene_id， 当然你可以写一个脚本替换成其他命名方式。

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/06/62.JPG)

出现warnIng：

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/06/63.JPG)

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/06/67.JPG)

大意是有数据丢失吧。

得到.count文件

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/06/64.JPG)

预览一下：基因名+reads数。

![](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/06/65.JPG)

Jimmy如是说：
----

我们这次分析是人类mRNA-Seq测序的结果，但是我们其实只下载了3个sra文件。一般而言RNA-Seq数据分析都至少要有2个重复，所以必须要有4个sra文件才行。我在仔细读完文章的方法这一段以后，发现他们有一批数据用的是其他课题组的： For 293 cells, the mRNA-seq results of the control samples include (1) those from the doxycycline-treated parental Flp-In T-REx 293 cells by us and (2) those from the doxycycline-treated control Flp-In T-REx 293 cells performed by another group unrelated to us (sample GSM1095127 in GSE44976)。 然后和Jimmy交流之后，他也承认自己只分析了小鼠的数据，而没有分析人类的数据。所以我们需要根据文章提供的线索下载另外一份数据，才能进行下一步的分析。

这个时候就有一个经常被问到的问题：不同来源的RNA-Seq数据能够直接比较吗？甚至说如果不同来源的RNA-seq数据的构建文库都不一样该如何比较?不同来源的RNA-Seq结果之间比较需要考虑 批次效应（batch effect) 的影响。

处理批次效应，根据我搜索的结果，是不能使用FPKM/RPKM，关于这个标准化的吐槽，我在biostars上找到了如下观点：

FPKM/RPKM 不是标准化的方法，它会引入文库特异的协变量
FPKM/RPKM has never been peer-reviewed, it has been introduced as an ad-hoc measure in a supplementary 没有同行评审
One of the authors of this paper states, that it should not be used because of faulty arithmetic 作者说算法有问题
All reviews so far have shown it to be an inferior scale for DE analysis of genes Length normalization is mostly dispensable imo in DE analysis because gene length is constant
有人建议使用一个Bioconductor包http://www.bioconductor.org/packages/devel/bioc/html/sva.html 我没有具体了解，有生之年去了解补充。

还有人引用了一篇文献 IVT-seq reveals extreme bias in RNA-sequencing 证明不同文库的RNA-Seq结果会存在很大差异。

结论： 可以问下原作者他们是如何处理数据的，居然有一个居然没有重复的分析也能过审。改用小鼠数据进行分析。或者使用无重复的分析方法，或者模拟一份数据出来，先把流程走完。

那么就把小鼠的四组数据进行统计
