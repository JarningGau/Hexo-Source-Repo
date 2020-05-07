---
title: win10安装velocyto-R
date: 2020-05-06 20:50:59
tags:
categories:
description:
---



## 问题描述

velocyto.R在win10下的安装会遇到以下错误：

<!-- more -->

C:/Rtools/mingw_64/bin/g++ -shared -s -static-libgcc -o velocyto.R.dll tmp.def RcppExports.o points_within.o routines.o -lboost_filesystem -lboost_system -lstdc++ -LC:/PROGRA~1/R/R-36~1.3/bin/x64 -lRlapack -LC:/PROGRA~1/R/R-36~1.3/bin/x64 -lRblas -fopenmp -lgfortran -lm -lquadmath -LC:/PROGRA~1/R/R-36~1.3/bin/x64 -lR
C:/Rtools/mingw_64/bin/../lib/gcc/x86_64-w64-mingw32/4.9.3/../../../../x86_64-w64-mingw32/bin/ld.exe: cannot find -lboost_filesystem
C:/Rtools/mingw_64/bin/../lib/gcc/x86_64-w64-mingw32/4.9.3/../../../../x86_64-w64-mingw32/bin/ld.exe: cannot find -lboost_system
collect2.exe: error: ld returned 1 exit status
no DLL was created
ERROR: compilation failed for package 'velocyto.R'

removing 'C:/Users/Windows User/Documents/R/win-library/packages/velocyto.R'
Error: Failed to install 'velocyto.R' from GitHub:
(converted from warning) installation of package ‘C:/Users/WINDOW~1/AppData/Local/Temp/Rtmpq259Wk/file33e07f9224a3/velocyto.R_0.6.tar.gz’ had non-zero exit status

**目前这个错误在github上仍然是open的，无人解决。**

<https://github.com/velocyto-team/velocyto.R/issues/86>

从报错信息上看，这个错误似乎是gcc编译的时候找不到lboost_system和lboost_filesystem这两个library导致的。

作者给出的解决方案是用docker安装。

If you are having trouble installing the package on your system, you can build a docker instance that can be used on a wide range of systems and cloud environments. To install docker framework on your system see [installation instruction](https://github.com/wsargent/docker-cheat-sheet#installation). After installing the docker system, use the following commands to build a velocyto.R docker instance:

```
cd velocyto.R/dockers/debian9
docker build -t velocyto .
docker run --name velocyto -it velocyto
```



## 下面我给出自己的解决方案

### 首先我们需要安装docker。

#### 开启hyper-V

右键`开始`菜单，选择`应用和功能`

![](https://www.runoob.com/wp-content/uploads/2017/12/1513668234-4363-20171206211136409-1609350099.png)

点击`程序和功能`

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20200506/001.png)

点击`启用或关闭Windows功能`

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20200506/002.jpg)

选中`Hyper-V`

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20200506/003.jpg)

重启系统。

【注意】如果启用hyper-V，你的虚拟机和安卓模拟器将无法正常运行，这时候关闭hyper-V，重启即可。

#### 下载并安装docker

直接点击[这个链接](https://download.docker.com/win/stable/Docker%20Desktop%20Installer.exe)下载

或者在这个页面选择下载stable版本：<https://hub.docker.com/editions/community/docker-ce-desktop-windows>

下载好后安装即可。

用win+r cmd打开命令行

```powershell
docker version
```

```
Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b
 Built:             Wed Mar 11 01:23:10 2020
 OS/Arch:           windows/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b
  Built:            Wed Mar 11 01:29:16 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

则说明安装成功。

### 利用docker安装velocyto.R

在当前目录新建一个`Dockerfile`，将以下内容复制到`Dockerfile`中。

```dockerfile
FROM rocker/r-ver:3.6
MAINTAINER Peter Kharchenko "peter_kharchenko@hms.harvard.edu"

RUN apt-get update --yes && apt-get install --no-install-recommends --yes \
  build-essential \
  cmake \
  git \
  libbamtools-dev \
  libboost-dev \
  libboost-iostreams-dev \
  libboost-log-dev \
  libboost-system-dev \
  libboost-test-dev \
  libssl-dev \
  libcurl4-openssl-dev \
  libxml2-dev \
  libz-dev \
  curl \
  libhdf5-cpp-100 \ 
  libarmadillo7 \
  libarmadillo-dev \
  libbz2-dev \
  liblzma-dev

RUN \
  R -e 'install.packages(c("devtools", "Rcpp","RcppArmadillo", "Matrix", "mgcv", "abind","igraph","Rtsne","cluster","data.table"), repos="https://mirrors.tuna.tsinghua.edu.cn/CRAN/")'

RUN \
  R -e 'install.packages(c("h5","BiocManager"))'

RUN \
  R -e 'BiocManager::install(c("pcaMethods","edgeR","GenomeInfoDb"))'

RUN \
  R -e 'BiocManager::install(c("Rsamtools","GenomicAlignments","Biostrings"))'

RUN useradd -m user
USER user
ENTRYPOINT ["/bin/bash"]
WORKDIR "/home/user"

RUN \
  git clone https://github.com/velocyto-team/velocyto.R && \
  mkdir -p ~/R/x86_64-redhat-linux-gnu-library/3.6

RUN \
  echo '.libPaths(c("~/R/x86_64-redhat-linux-gnu-library/3.6", .libPaths()))' > .Rprofile && \
  R -e 'devtools::install_local("~/velocyto.R/",dep=T,upgrade_dependencies=F)'


ENV  LD_LIBRARY_PATH=/usr/local/lib/R/lib/:$LD_LIBRARY_PATH \
  R_PROFILE=~/.Rprofile

```

我们创建一个叫velocyto的镜像

```
docker build -t velocyto .
```

```
docker images
```

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
velocyto            latest              d555a68234b9        18 hours ago        1.43GB
<none>              <none>              e504d12ae7f5        19 hours ago        1.38GB
ubuntu              latest              1d622ef86b13        12 days ago         73.9MB
hello-world         latest              bf756fb1ae65        4 months ago        13.3kB
rocker/r-ver        3.6                 afe7688ca972        10 months ago       606MB
```

然后我们重启velocyto并将本地`FigureYa177RNAvelocity`挂载到`/home/user/FigureYa177RNAvelocity`文件夹下

```shell
# 从velocyto镜像启动一个名为velocyto的容器
docker run --name velocyto -it velocyto
# 创建一个挂载的锚点，当前在/home/user下
mkdir FigureYa177RNAvelocity
# 退出容器
exit
# 查看刚才的容器编号
docker ps -a 
# 将该容器转化为一个名为velocyto_figureya的新镜像
docker commit df73c896107a velocyto_figureya
# 从velocyto_figureya镜像启动一个名为velocyto的容器，并挂载本地文件
docker run --name velocyto_figureya -itv /path/to/FigureYa177RNAvelocity:/home/user/FigureYa177RNAvelocity velocyto
```

### 利用velocyto.R进行RNA动力学分析

在`s6_trajectory.Rmd`的最后增加这样一段代码

```r
cell_cluster <- as.character(pData(monocle_cds)$Cluster)
names(cell_cluster) <- colnames(monocle_cds)
colors <- c("#F8766D", "#C49A00", "#53B400", "#00C094", "#00B6EB", "#A58AFF", "#FB61D7")
names(colors) <- 1:7
cell.colors <- plyr::mapvalues(cell_cluster, names(colors), colors)

saveRDS(t(reducedDimS(monocle_cds)), "tmp/s6-DDRTree_embeddings.rds")
saveRDS(cell_cluster, "tmp/s6-cell_cluster.rds")
saveRDS(cell.colors, "tmp/s6-cell_colors.rds")
```

执行`s7_RNAVelocity.R`

```shell
cd FigureYa177RNAvelocity/scripts
Rscript s7_RNAVelocity.R
```

以下是`s7_RNAVelocity.R`的代码

```r
library(velocyto.R)

cell_cluster <- readRDS("tmp/s6-cell_cluster.rds")
cell_colors <- readRDS("tmp/s6-cell_colors.rds")
cell_emb <- readRDS("tmp/s6-DDRTree_embeddings.rds")

# 读取velocyto.py的结果
message("loading loom files.")
ldat <- list(
  old1 = read.loom.matrices("../data/loom/old1.loom"),
  old2 = read.loom.matrices("../data/loom/old2.loom"),
  old3 = read.loom.matrices("../data/loom/old3.loom"),
  young1 = read.loom.matrices("../data/loom/young1.loom"),
  young2 = read.loom.matrices("../data/loom/young2.loom"),
  young3 = read.loom.matrices("../data/loom/young3.loom")
)

# 合并数据
message("merging files.")
matrix.name <- names(ldat$old1)
ldat <- lapply(matrix.name, function(x){
  dat.list <- lapply(ldat, function(y){
    y[[x]]
  })
  dat.merged <- do.call(cbind, dat.list)
  dat.merged
})
names(ldat) <- matrix.name

# 重新对细胞命名
message("rename matrix.")
ldat <- lapply(ldat, function(x) {
  colnames(x) <- sub("x", "", sub(":", "_", colnames(x)))
  x
})

# 保留fibroblasts
message("extracting fibroblast.")
cells.id <- names(cell_cluster)
ldat <- lapply(ldat, function(x) {
  x[, cells.id]
})
lapply(ldat, dim)

# 保留表达量高于一定阈值的基因
message("selecting features.")
emat <- ldat$spliced # exonic read (spliced) expression matrix
nmat <- ldat$unspliced # intronic read (unspliced) expression matrix

# 根据最小最大细胞类均值对基因进行过滤
# min.max.cluster.average：不同细胞类型中，基因均值最大值大于该阈值，则该基因保留
emat <- filter.genes.by.cluster.expression(emat, cell_cluster, min.max.cluster.average = 0.05)
nmat <- filter.genes.by.cluster.expression(nmat, cell_cluster, min.max.cluster.average = 0.05)
length(intersect(rownames(emat), rownames(nmat)))

# RNA Velocity分析
### 参数设置
fit.quantile = 0.05 # 官方教程设定为 0.05
deltaT = 1 # default: 1
kCells = 10 # default: 10
### RNA velocity分析
message("perferm RNA velocity analysis.")
rvel.qf <- gene.relative.velocity.estimates(emat, nmat, deltaT = deltaT, kCells = kCells, fit.quantile = fit.quantile)

# RNA Velocity可视化

### 参数设定
emb = cell_emb # DDRTree坐标
vel = rvel.qf # velocity estimates (上一步的结果)
n = 100 # 最邻近细胞的数量
scale = "sqrt" # scale方法
# 散点的颜色（用以区分不同的细胞状态）
cell.colors = cell_colors
cell.alpha = 0.5 # 散点颜色的透明度
cell.cex = .5 # 散点的尺寸
arrow.scale = 1 # 箭头的长度
arrow.lwd = 1 # 箭头的粗细
grid.n = 60 # grids的数量

### plot
message("plotting.")
png(filename = "RNA_velocity.png", width = 8, height = 6, units = "in", res = 300)
show.velocity.on.embedding.cor(emb, vel, n, scale=scale, 
                               cell.colors = ac(cell.colors, alpha = cell.alpha),
                               cex = cell.cex, arrow.scale = arrow.scale, 
                               show.grid.flow = TRUE, min.grid.cell.mass = 1,
                               grid.n = grid.n, arrow.lwd = arrow.lwd)
dev.off()
```



## 参考

[docker win10安装](https://www.runoob.com/docker/windows-docker-install.html)

[零基础，一小时入门docker](https://zhuanlan.zhihu.com/p/23599229)

