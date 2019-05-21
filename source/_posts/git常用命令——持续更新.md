---
title: git常用命令——持续更新
date: 2019-05-21 21:20:42
tags:
  - git
categories:
  - 版本控制
description:
---

## Git协议切换

在远程仓库和本地之间，有两种通信协议：`SSH`和`HTTPS`

<!-- more -->

### 由https切换为SSH

#### 配置SSH密钥和git公钥

首先在本地机器上要生成自己的SSH密钥

```shell
cd ~/.ssh
ssh-keygen -t rsa -C "XX@XX.mail" #随便输一个邮箱地址
```

随后程序会提示你输入文件名前缀，一路回车即可。系统默认的文件名前缀是`id_sra`

这一步完成后，你的目录下应该有以下文件

```shell
id_rsa  id_rsa.pub  known_hosts
```

```shell
cat id_sra.pub
```

打开github，`setting`=>`SSH and GPG keys`=>`New SSH key`

title可以输一个有意义的名字，方便你识别本地机器。（因为我们可能会在多个本地机器上进行开发）

将`id_sra.pub`的文件内容复制粘贴到key中，点击`Add SSH key`即可

#### 切换协议

```shell
cd /path/repository
# step1. 查看当前协议
git remote -v # 以https开头
# step2. 切换为SSH协议(切换为https协议反之即可)
git remote set-url origin git@github.com:username/repository.git
```

