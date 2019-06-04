---
title: 拯救那些被EXCEL篡改的基因名
date: 2019-06-04 11:49:22
tags:
  - R
categories:
  - 搬砖随笔
description:
---

>有时候在GEO下载发表的基因表达矩阵的时候，经常遇到如下的基因名被EXCEL自动篡改。这里介绍一个R包：[HGNChelper](<https://waldronlab.io/HGNChelper/>)，可以自动识别这些基因，并进行修正。
>
>| EXCEL ERROR | Corrected Gene Symbol |
>| ----------- | --------------------- |
>| 1-Sep       | SEPT1                 |
>| 10-Sep      | SEPT10                |
>| 11-Sep      | SEPT11                |
>| 12-Sep      | SEPT12                |

<!--more-->

使用起来也很简单：

```R
library(HGNChelper)
human = c("FN1", "tp53", "UNKNOWNGENE","7-Sep", "9/7", "1-Mar", "Oct4", "4-Oct",
      "OCT4-PG4", "C19ORF71", "C19orf71")
checkGeneSymbols(human)
```

```
Human gene symbols should be all upper-case except for the 'orf' in open reading frames. The case of some letters was corrected.x contains non-approved gene symbols             x Approved Suggested.Symbol
1          FN1     TRUE              FN1
2         tp53    FALSE             TP53
3  UNKNOWNGENE    FALSE             <NA>
4        7-Sep    FALSE            SEPT7
5          9/7    FALSE            SEPT7
6        1-Mar    FALSE MARC1 /// MARCH1
7         Oct4    FALSE           POU5F1
8        4-Oct    FALSE           POU5F1
9     OCT4-PG4    FALSE         POU5F1P4
10    C19ORF71    FALSE         C19orf71
11    C19orf71     TRUE         C19orf71
```

`checkGeneSymbols`不光可以教程EXCEL造成的篡改，同样也可以将Alias转换成标准基因名。但是有一些错误是无法解决的，比如`1-Mar`这样的错误，可能会对应多个Gene Symbol。这样的数据只能舍弃了。

