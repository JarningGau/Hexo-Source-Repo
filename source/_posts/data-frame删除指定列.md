---
title: data.frame删除指定列
date: 2019-06-04 15:00:47
tags:
  - R
  - R basic
categories:
  - 搬砖随笔
description:
---

```R
df <- data.frame(a1=c(1,2,3,4), a2=c(5,6,7,8))
#删除列a1
subset(df, select = -c(a1)) #按列名
subset(df, select = -c(1)) #按列数
df[, -c(1)] #按列数
```

值得注意的是，`subset`函数总会返回`data.frame`。推荐用`subset`。

