---
title: 将FPKM转换为TPM
date: 2019-06-04 16:01:17
tags:
  - RNA-seq
categories:
  - 搬砖随笔
description:
mathjax: true
---

$$TPM_i = \frac{X_i}{l_i}\times\frac{1}{\sum{\frac{X_j}{l_j}}}\times10^6$$ 

$$FPKM_i = \frac{X_i}{l_i\times\frac{N}{10^6}} = \frac{X_i}{l_iN}\times10^6$$

- $X_i$：Fragments of gene i

- $l_i$：Length of gene i (KB)

- N：Total fragments

$TPM_i = (\frac{X_i}{l_iN})(\frac{1}{\sum{\frac{X_j}{l_jN}}})  = \frac{FPKM_i}{\sum{FPKM_j}}$ 

```R
# cols are samples and rows are gene features
TPM_mtx <- FPKM_mtx / rep(colSums(FPKM_mtx), each = nrow(FPKM_mtx)) * 1e6
```

<!--more-->

## 参考

<http://www.bio-info-trainee.com/2017.html>

