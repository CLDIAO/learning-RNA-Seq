
# 系统准备

①[install linux on windows](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)（稳定性？）

②虚拟机安装：
 
  [Virtualbox](http://download.virtualbox.org/virtualbox/5.2.4/VirtualBox-5.2.4-119785-Win.exe)

  [ubuntu](http://mirrors.shu.edu.cn/ubuntu-releases/16.04.3/ubuntu-16.04.3-desktop-amd64.iso) (原生系统更稳定)
  
  ### 设置root 账户
  
  ![image](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/02/20180305001.PNG)
  
  操作如图。

为了提高下载速度，可以替换系统默认像[ustc参考]（http://mirrors.ustc.edu.cn/help/ubuntu.html）
>#备份

>cd /etc/apt/

>sudo cp source.list source.list.bk

>#替换

>sudo sed -i 's/http/https/g' sources.list

>sudo sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' sources.list

>sudo sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' sources.list

>#更新

>sudo apt-get update

>sudo apt-get upgrade

# [另一种参考](http://www.gutils.com/2016/07/28/linux/ubuntu-ali-source/)

`diaocl@AmazingD:~$ lsb_release -a` #查看系统

`LSB modules are available.` 

`Distributor ID: Ubuntu`  

`Description:    Ubuntu 16.04.3 LTS` 

`Release:        16.04`  

`Codename:       xenial` #我的系统状态

[确认阿里源支持](http://mirrors.aliyun.com/ubuntu/dists/)

`diaocl@AmazingD:~$ cd /etc/apt`                                                                                                                                          
`diaocl@AmazingD:/etc/apt$ sudo mv sources.list sources.list_bak`#备份系统源

`diaocl@AmazingD:/etc/apt$ sudo vi sources.list`#添加新的源文件

`deb http://mirrors.aliyun.com/ubuntu/ xenial main multiverse restricted universe`

`deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main multiverse restricted universe `

`deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main multiverse restricted universe`

`deb http://mirrors.aliyun.com/ubuntu/ xenial-security main multiverse restricted universe`

`deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main multiverse restricted universe`

`deb-src http://mirrors.aliyun.com/ubuntu/ xenial main multiverse restricted universe`

`deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main multiverse restricted universe`

`deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main multiverse restricted universe`

`deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main multiverse restricted universe`

`deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main multiverse restricted universe`                                                                                          

`diaocl@AmazingD:/etc/apt$ sudo apt-get update`#进行更新

`sudo apt-get upgrade`

# 软件安装
## anaconda安装
[download anaconda](https://www.anaconda.com/download/#linux)版本差别不大，自己选择就好。

[install anaconda](https://docs.anaconda.com/anaconda/install/linux)

安装完成之后，需要设置路径：

`echo 'export PATH="~/anaconda2/bin:$PATH"' >> ~/.bashrc`

#更新bashrc以立即生效
`source ~/.bashrc`

运行`conda`查看使用介绍。

更换conda源：
>conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

>conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

>conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/

>conda config --add channels bioconda

>conda config --set show_channel_urls yes

