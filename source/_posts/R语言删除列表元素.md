---
title: R语言删除列表元素
date: 2019-06-05 15:10:45
tags:
  - R
  - R basic
categories:
  - 搬砖随笔
description:
---

```R
l <- list(
    "a" = 1:6,
    "b" = rep("hello", 6)
)
l$a <- NULL
```

