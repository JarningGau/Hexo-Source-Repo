---
title: R变量名称和字符串的转换
date: 2019-06-07 14:20:15
tags:
  - R
categories:
 - 搬砖随笔
description:
---

## 应用场景

- 数据

```R
data <- data.frame(x=1:5, y=3:7)
data$z=sin(data$x+data$y) 
data$a=sin(data$x*data$y)
data$b=cos(data$x+data$y)
data$d=cos(data$x*data$y)
data
  x y          z          a          b          d
1 1 3 -0.7568025  0.1411200 -0.6536436 -0.9899925
2 2 4 -0.2794155  0.9893582  0.9601703 -0.1455000
3 3 5  0.9893582  0.6502878 -0.1455000 -0.7596879
4 4 6 -0.5440211 -0.9055784 -0.8390715  0.4241790
5 5 7 -0.5365729 -0.4281827  0.8438540 -0.9036922
```

- 需求：批量画散点图

```R
col_names <- colnames(data)
plot_list <- lapply(3:6, function(x) {
    ggplot(data=data, aes(x=x, y=y, color=col_names[x])) + geom_point() + ggtitle(col_names[x])
})
```

- 这样其实是不行的，因为无法传递字符串给color

## get

- 解决方案

```
col_names <- colnames(data)[3:length(col_names)]
plot_list <- lapply(col_names, function(x) {
    ggplot(data=data, aes(x=x, y=y, color=x)) + geom_point() + ggtitle(x)
})
cowplot::plot_grid(plotlist = plot_list, ncol = 2)
```

## assign

assign是get的逆运算，比如，

```
assign("a", 1) 
# 等价于
a <- 1
```

目前还没有遇到需要assign函数的场景，忘了它吧。

## Reference

[R学习笔记（二）变量名称和字符串的转换](https://zhuanlan.zhihu.com/p/30383865)

