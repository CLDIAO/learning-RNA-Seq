
# 系统准备

## ①[install linux on windows](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)（稳定性？）

## ②虚拟机安装：
 
  [Virtualbox](http://download.virtualbox.org/virtualbox/5.2.4/VirtualBox-5.2.4-119785-Win.exe)

  [ubuntu](http://mirrors.shu.edu.cn/ubuntu-releases/16.04.3/ubuntu-16.04.3-desktop-amd64.iso) (原生系统更稳定)
  
### 设置root 账户
  
  ![image](https://github.com/CLDIAO/learning-RNA-Seq/blob/master/graph/02/20180305001.PNG)
  
  操作如图。
  
### 安装virtualbox tools
  
  virtualbox tools是增强工具，安装后可以设置屏幕分辨率，实现全屏，自由切换鼠标，主机和虚拟机之间的
  拖放传数据，设置共享文件夹等。步骤：点击virtual box 软件菜单-设备-安装增强功能-run安装工具-输入root
  账户密码-enter完成安装-重启系统。
  
### 设置共享文件夹
  
  设备-共享粘贴板/拖放-双向。
  
  在主机上创建一个文件夹，设备-共享文件夹-固定分配-点击文件夹图标-选择路径并勾选自动挂载和固定分配
  -重启虚拟机。
  
  重启后文件夹里会有个自己命名的文件夹，表示挂载成功。这时普通用户没有权限使用，默认为root用户才能使用。
  
 `su`切换到root账户，`chmod -R 777 sf_share`，设置权限，是普通用户也能读写。还要将普通账户添加到vboxsf组中
 才能让普通用户使用共享文件夹，这个步骤很重要。`usermod -a -G vboxsf cldiao`。
 
 重启系统，进入media,可以使用sf_share目录。
 

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

当命令行前面出现(base)的时候说明现在已经在conda的环境中了。

更换conda源：
>conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

>conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

>conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/

>conda config --add channels bioconda

>conda config --set show_channel_urls yes

# 参考
[conda的安装与使用](https://www.jianshu.com/p/edaa744ea47d?tdsourcetag=s_pctim_aiomsg)
