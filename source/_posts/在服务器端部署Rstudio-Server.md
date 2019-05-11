---
title: 在服务器端部署Rstudio Server
date: 2019-05-11 13:04:28
tags:
  - R
  - Rstudio
categories:
  - 工作环境
description:
---

> 服务器OS是Centos7，以root用户的身份进行更新

<!-- more -->

## 在centos上安装和配置RStudio Server

### 检查服务器端R版本

> R版本为R-3.2.2，太过老旧，进行升级。

### 首先修改yum源 (optional)

```shell
# 备份
cd /etc/yum.repos.d/
mv CentOS-Base.repo CentOS-Base.repo.backup
# 修改yum源为USTC镜像，centos7
wget -O CentOS-Base.repo https://lug.ustc.edu.cn/wiki/_export/code/mirrors/help/centos?codeblock=3
# 运行yum makecache生成缓存
yum makecache
# 更新系统
yum -y update
```

### 更新R

```
wget https://mirrors.njupt.edu.cn/epel/7/x86_64/Packages/r/R-3.5.2-2.el7.x86_64.rpm
yum install R-3.5.2-2.el7.x86_64.rpm
```

### 安装RStudio

```shell
wget https://download2.rstudio.org/server/centos6/x86_64/rstudio-server-rhel-1.2.1335-x86_64.rpm
yum install rstudio-server-rhel-1.2.1335-x86_64.rpm
```

RStudio默认的端口是8787，输入ip+端口号即可访问

### 创建用户

首先创建用户组，然后创建用户。例如

```shell
groupadd hadoop
useradd hadoop_usr -d $usr_dir -g hadoop
passwd hadoop_usr
```

这样，我们就能以`hadoop_usr`用户登录了。



## 参考资料

- [Centos镜像使用帮助](<https://lug.ustc.edu.cn/wiki/mirrors/help/centos>)
- [RStudio下载页面](<https://www.rstudio.com/products/rstudio/download-server/>)
- [在Linux服务器部署RStudio Server](<https://www.cnblogs.com/litao1105/p/4780686.html>)