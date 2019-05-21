---
title: Anaconda的安装和配置
date: 2019-04-26 22:22:38
tags:
  - python
categories:
  - 工作环境
description:
---

## Anaconda的安装

anaconda的安装包可以在[官网](https://www.anaconda.com/download/)上选择对应版本下载，如果官网网速太慢，可以选择[USTC镜像](https://mirrors.ustc.edu.cn/anaconda/archive/)下载对应版本。

<!-- more -->

![ANACONDA下载界面](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190426/anaconda1.JPG)



随后，将anaconda安装包（我这里用的是`Anaconda2-5.1.0-Linux-x86_64.sh`）放在对应服务器的任意目录下，授予其可执行权限并执行，进入安装界面：

```shell
chmod +x ./Anaconda2-5.1.0-Linux-x86_64.sh && ./Anaconda2-5.1.0-Linux-x86_64.sh
```

![安装界面](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190426/anaconda2.JPG)

- 一开始会让你阅读license，直接一路回车。

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190426/anaconda3.JPG)

- 问你是否同意license，当然是yes。

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190426/anaconda4.JPG)

- 这一步在指定安装路径，一般默认路径就可以了，直接回车。

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190426/anaconda5.JPG)

- 是否要将Anaconda安装路径写入环境变量？当然是yes。

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190426/anaconda6.JPG)

- 至此，Anaconda已经安装完毕。这里问你是否还要安装Microsoft VSCode？虽然我并不懂这里为啥要装它，但是装了也无妨，输入yes。

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190426/anaconda7.JPG)

- Anaconda安装完毕！此时输入conda应该会看到conda的使用说明，显示命令没有找到，则退出命令行重新登录。

------

由于利用Anaconda安装软件的过程中，国外镜像会经常遇见网络问题，因此，添加Anaconda USTC源到channel。详见 https://mirrors.ustc.edu.cn/help/anaconda.html

------



## Conda的基本使用

- 查看本地包和搜索包

```shell
conda list # 列出安装的软件包
conda search <package ambigious name> # 当我们想安装某个包却不知道具体名字，可以用这个命令获取其完整名字以及各种版本号
---
例如： conda search numpy # *表示当前已经安装的包
---------------------------------------------------------------------------------------
Fetching package metadata ...............
numpy                        1.7.2           py27_blas_openblas_201  conda-forge     [blas_openblas]
                             1.7.2           py27_blas_openblas_202  conda-forge     [blas_openblas]
                             1.12.0                   py36_0  defaults        
                             1.12.0             py36_nomkl_0  defaults        [nomkl]
                          *  1.12.1                   py27_0  defaults        
                             1.12.1             py27_nomkl_0  defaults        [nomkl]
                             1.13.1                   py36_0  defaults        
                             1.13.1             py36_nomkl_0  defaults        [nomkl]
numpy-indexed                0.3.2                    py27_0  conda-forge                
                             1.0.47                   py35_0  conda-forge     
                             1.0.47                   py36_0  conda-forge     
numpy_groupies               0.9.6                    py27_0  conda-forge     
                             0.9.6                    py35_0  conda-forge     
                             0.9.6                    py36_0  conda-forge     
numpy_sugar                  1.0.6                    py27_0  conda-forge     
                             1.0.6                    py34_0  conda-forge        
numpydoc                     0.6.0                    py27_0  conda-forge     
                             0.6.0                    py34_0  conda-forge             
xnumpy                       0.0.1                    py27_0  conda-forge     
---------------------------------------------------------------------------------------
```

- 安装包

```shell
conda install <package name> # 安装软件包
conda install <package name>=<version> # 安装特定版本的软件包
conda remove <package name> # 移除软件包
```

- 安装R

```shell
# 具体见下面
conda install -c r r-essentials # 安装R,及80多个常用的数据分析包, 包括idplyr, shiny, ggplot2, tidyr, caret 和 nnet
# 安装单个包
conda install -c https://conda.binstar.org/bokeh ggplot 
```

- 获取帮助

```shell
conda -h # 查看conda可用的命令
conda install -h #查看install子命令的帮助
```

------

## Conda的channel

Conda默认的源访问速度有些慢，可以增加国内的源；另外还可以增加几个源，以便于安装更多的软件，尤其是`bioconda`安装生信类工具。`conda-forge`通道是Conda社区维护的包含很多不在默认通道里面的通用型软件。`r`通道是向后兼容性通道，尤其是使用R3.3.1版本时会用到。后加的通道优先级更高，因此一般用下面列出的顺序添加。

```shell
conda config --add channels conda-forge # Lowest priority
conda config --add channels r # Optional
conda config --add channels defaults
conda config --add channels bioconda 
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ # Anocanda科大镜像
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/ 
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/ # 科大通道, 最高优先级
conda config --set show_channel_urls yes

# 显示已有的通道
conda config --get channels
---------------------------------------------------------------------------------------
--add channels 'defaults'   # lowest priority
--add channels 'https://mirrors.ustc.edu.cn/anaconda/pkgs/free/'
--add channels 'https://mirrors.ustc.edu.cn/anaconda/pkgs/main/'
--add channels 'https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/'
--add channels 'https://mirrors.ustc.edu.cn/anaconda/cloud/msys2/'
--add channels 'https://mirrors.ustc.edu.cn/anaconda/cloud/bioconda/'
--add channels 'https://mirrors.ustc.edu.cn/anaconda/cloud/menpo/'   # highest priority
---------------------------------------------------------------------------------------
# 移除冗余的通道(示例)
conda config --remove channels 'https://mirrors.ustc.edu.cn/anaconda/pkgs/free/'
```

conda通道的配置文件一般在`~/.condarc`里面，内容如下。全局控制conda的安装在`conda_path/.condarc`，具体操作见<https://conda.io/docs/user-guide/configuration/admin-multi-user-install.html>

【补充】

最近Anaconda清华和科大的镜像都挂了，改用腾讯的镜像

```shell
conda config --add channels https://mirrors.cloud.tencent.com/anaconda/pkgs/free/
conda config --add channels https://mirrors.cloud.tencent.com/anaconda/cloud/bioconda/
conda config --add channels https://mirrors.cloud.tencent.com/anaconda/cloud/msys2/
conda config --add channels https://mirrors.cloud.tencent.com/anaconda/cloud/menpo/
conda config --add channels https://mirrors.cloud.tencent.com/anaconda/cloud/peterjc123/
conda config --add channels https://mirrors.cloud.tencent.com/anaconda/pkgs/main/
conda config --add channels https://mirrors.cloud.tencent.com/anaconda/cloud/conda-forge/
conda config --add channels https://mirrors.cloud.tencent.com/anaconda/cloud/pytorch/
conda config --set show_channel_urls yes
```



------

## 创建不同的软件运行环境

这是`Conda`最有特色的地方，可以通过创建不同的环境，同时运行不同软件的多个版本。

新创建的软件环境的目录为`anaconda_path/envs/enrironment_name`

- 新建环境

```shell
conda create -n <env_name> python=<version> # 指定python版本号是可选的，不指定的话，会安装到默认的python版本号下，即在命令行中输入python所进入的那个版本
conda create -n bsc_bioinfo # 例如，新建一个叫bsc_bioinfo的环境，用户安装生信分析所需要的软件
---------------------------------------------------------------------------------------
Solving environment: done


==> WARNING: A newer version of conda exists. <==
  current version: 4.4.10
  latest version: 4.5.0

Please update conda by running

    $ conda update -n base conda



## Package Plan ##

  environment location: /root/anaconda2/envs/bsc_bioinfo


Proceed ([y]/n)?
---------------------------------------------------------------------------------------
# 输入 y
```

- 查看所有环境

```shell
conda env list # *是当前环境
## 注意，尽量不要把各种包都一块安装到base环境下。
---------------------------------------------------------------------------------------
# conda environments:
#
base                  *  /root/anaconda2
bsc_bioinfo              /root/anaconda2/envs/bsc_bioinfo
---------------------------------------------------------------------------------------
```

- 指定环境安装包

```shell
conda install -n <env_name> -c <channel> <packange_name> # -n和-c为可选参数，不指定-n则安装在当前环境中，不指定-c则会按优先级依次搜索包
```

- 进入新的环境

```shell
source activate bsc_bioinfo
```

- 退出新的环境

```shel
source deactivate
```



------

## Conda配置R

> 在添加了不同的源之后，有些源更新快，有些更新慢，经常会碰到版本不一的问题。而且软件版本的优先级，低于源的优先级。保险期间，先做下搜索，获得合适的版本号，然后再选择安装。

```shell
conda search r-essentials
---------------------------------------------------------------------------------------
Loading channels: done
Name                       Version                   Build  Channel
r-essentials               1.0                    r3.2.1_0  defaults
r-essentials               1.0                    r3.2.1_0  r
r-essentials               1.0                   r3.2.1_0a  defaults
r-essentials               1.0                   r3.2.1_0a  r
r-essentials               1.1                    r3.2.1_0  defaults
r-essentials               1.1                    r3.2.1_0  r
r-essentials               1.1                   r3.2.1_0a  defaults
r-essentials               1.1                   r3.2.1_0a  r
r-essentials               1.1                    r3.2.2_0  defaults
r-essentials               1.1                    r3.2.2_0  r
r-essentials               1.1                   r3.2.2_0a  defaults
r-essentials               1.1                   r3.2.2_0a  r
r-essentials               1.1                    r3.2.2_1  defaults
r-essentials               1.1                    r3.2.2_1  r
r-essentials               1.1                   r3.2.2_1a  defaults
r-essentials               1.1                   r3.2.2_1a  r
r-essentials               1.4                           0  defaults
r-essentials               1.4                           0  r
r-essentials               1.4.1                  r3.3.1_0  defaults
r-essentials               1.4.1                  r3.3.1_0  r
r-essentials               1.4.2                         0  defaults
r-essentials               1.4.2                         0  r
r-essentials               1.4.2                  r3.3.1_0  defaults
r-essentials               1.4.2                  r3.3.1_0  r
r-essentials               1.4.3                  r3.3.1_0  defaults
r-essentials               1.4.3                  r3.3.1_0  r
r-essentials               1.5.0                         0  defaults
r-essentials               1.5.0                         0  r
r-essentials               1.5.1                         0  defaults
r-essentials               1.5.1                         0  r
r-essentials               1.5.2                  r3.3.2_0  https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge
r-essentials               1.5.2                  r3.3.2_0  defaults
r-essentials               1.5.2                  r3.3.2_0  r
r-essentials               1.5.2                  r3.4.1_0  defaults
r-essentials               1.5.2                  r3.4.1_0  r
r-essentials               1.6.0                  r3.4.1_0  defaults
r-essentials               1.6.0                  r3.4.1_0  r
r-essentials               1.7.0            r342hf65ed6a_0  defaults
r-essentials               1.7.0            r342hf65ed6a_0  r
r-essentials               3.4.1                  r3.4.1_0  https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge
r-essentials               3.4.3                  mro343_0  defaults
r-essentials               3.4.3                  mro343_0  r
r-essentials               3.4.3                    r343_0  defaults
r-essentials               3.4.3                    r343_0  r
---------------------------------------------------------------------------------------

conda install -c r -n bsc_bioinfo r-essentials=3.4.3
# 需要安装的包太多了，可能会出现网络超时，此时只要重新运行命令，已经下载的包会自动跳过。
```



------

## 利用Conda安装转录组分析工具

> 当初利用编译安装samtools的时候，坑实在太多，需要安装很多依赖的包。如今利用conda，可以一键安装所有需要的生信软件，省去编译和设置环境变量的麻烦。

```shell
conda install samtools bowtie bowtie2 bwa tophat hisat2 star cufflinks
```

