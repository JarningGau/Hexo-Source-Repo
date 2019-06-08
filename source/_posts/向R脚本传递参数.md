---
title: 向R脚本传递参数
date: 2019-06-07 10:47:46
tags:
  - R
categories:
  - 搬砖随笔
description:
---

## commandArgs

是R自带的参数传递函数，属于位置参数。

- 一个例子

```shell
touch "test.R"
```

```R
args = commandArgs(TRUE)
print(args[1])
print(args[2])
print(args[3])
```

<!--more-->

```shell
Rscript test.R hello 123 world
[1] "hello"
[1] "123"
[1] "world"
```

- 注意，`cmmandArgs()`如果不加参数TRUE，

```R
Args = commandArgs()
cat("Args[1]=",Args[1],"\n")
cat("Args[2]=",Args[1],"\n")
cat("Args[3]=",Args[3],"\n")
cat("Args[4]=",Args[4],"\n")
cat("Args[5]=",Args[5],"\n")
cat("Args[6]=",Args[6],"\n")
cat("Args[7]=",Args[7],"\n")
cat("Args[8]=",Args[8],"\n")
```

```shell
Rscript test.R hello 123 world
Args[1]= /usr/lib/R/bin/exec/R
Args[2]= /usr/lib/R/bin/exec/R
Args[3]= --no-restore
Args[4]= --file=test.R
Args[5]= --args
Args[6]= hello # 输入参数从这里开始
Args[7]= 123
Args[8]= world
```

- 实际上TRUE是传给`trailingOnly`的，官方文档里的解释：If `TRUE`, only arguments after `--args` are returned.



## getopt

需要先安装`getopt`包

```R
getopt(spec = NULL, opt = commandArgs(TRUE), command = get_Rscript_filename(), 
       usage = FALSE,debug = FALSE)
```

spec：一个4-5列的矩阵，里面包括了参数信息，前四列是必须的，第五列可选。

第一列：参数的longname，多个字符。

第二列：参数的shortname，一个字符。

第三列：参数是必须的，还是可选的，数字：0代表不接参数 ；1代表必须有参数；2代表参数可选。

第四列：参数的类型。logical；integer；double；complex；character；numeric

第五列：注释信息，可选。

usage：默认为FALSE, 这时参数无实际意义，而是以用法的形式输出。

```R
library(getopt)
spec = matrix(c(
  'verbose', 'v', 2, "integer",
  'help'   , 'h', 0, "logical",
  'count'  , 'c', 1, "integer",
  'mean'   , 'm', 1, "double"
), byrow=TRUE, ncol=4)
opt = getopt(spec)
print(opt$count)
print(opt$mean)
```

```shell
Rscript test.R -c 12 -m 2
[1] 12
[1] 2
```



## 参考

[commandArgs](https://www.rdocumentation.org/packages/R.utils/versions/2.8.0/topics/commandArgs)

<https://blog.csdn.net/u011596455/article/details/79753788>