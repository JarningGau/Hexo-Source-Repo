---
title: 去除data.frame列名中的'X'字符
date: 2019-06-04 12:36:41
tags:
  - R
  - R basic
categories:
  - 搬砖随笔
description:
---

`read.table`函数会自动在以数字开头的列名前加上一个'X'，可以通过如下方式去除

```R
read.table(filename, sep = "\t", header = T, row.names = 1, check.names = F)
```

