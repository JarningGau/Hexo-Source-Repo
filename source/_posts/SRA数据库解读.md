---
title: SRA数据库解读
date: 2019-05-24 12:54:44
tags:
  - 公共数据库
categories:
  - 公共数据挖掘
description:
---

目前，所有的测序数据都存储在国际核酸序列数据库联盟(INSDC)里，包括SRA，EBI，DDBJ。上传到任何一个数据库中的数据都会彼此共享。但是维护最好的当属SRA数据库。本文只涉及SRA数据库。以下是NCBI对SRA数据库的简介：

> The SRA is NIH's primary archive of high-throughput sequencing data and is part of the International Nucleotide Sequence Database Collaboration (INSDC) that includes at the NCBI Sequence Read Archive (SRA), the European Bioinformatics Institute (EBI), and the DNA Database of Japan (DDBJ). Data submitted to any of the three organizations are shared among them.

<!--more-->

## 从GSE ID说起

通常，我们读到一篇文章，想用一下里面的数据，我们能从文章中得到的信息一般是`GSE ID`，该ID和`GEO`数据库关联，一般直接google就能检索到`GSE ID`下的详细信息。

GEO Datasets有三个ID，分别是

- `GPL`，Platform ID。这个ID记录了测序或芯片平台，对于芯片平台，其包含了探针-基因对应表。一个`GPL ID`对应一个平台。一个`GPL ID`对应多个`GSM ID`或`GSE ID`。这个ID由数据上传者提供。但是对应的记录应该由厂商提供。
- `GSM`，Sample ID。这个ID记录了一个处于特定处理条件的样本以及对应的建库测序方法。一个`GSM ID`对应一个`GPL ID`，但是对应多个`GSE ID`。这个ID中的记录由数据上传者提供，包括样本处理方式，建库的方法，数据处理方式等。
- `GSE`，Series ID。这个ID将所有关联的样本联系起来，形成一个有目的的研究。这个ID对应的记录由数据上传者提供，包括文章的摘要（如果发表有的话），整体实验设计思路等。

一篇文章内可能包含多个`GSE ID`。有一些是作者自己贡献的数据，有些是已经发表的数据。一般来讲，每个`GSE ID`记录提供了处理后的数据，可以直接用。但是通常，因为数据处理方法不一样，数据不好放在一起比较。所以我们希望得到原始数据。这个时候，就需要访问SRA数据库了。

## SRA数据库

每一个`GSE ID`都能对应到SRA数据库的`SRP ID`。在Relations term下面，直接给出了超链接。

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190524/GEO_1.png)

点击链接进入SRA数据库，有时候一个Series下面有很多样本，不方便查看。我们可以使用`SRA Run Selector`浏览。

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190524/SRA_1.png)

点击`RunInfo Table`可以获得每个样本的所有信息；

点击`Accession List`可以获得所有的`SRR ID` ，用`prefetch`就可以下载对应的`.sra`文件了

![](https://raw.githubusercontent.com/JarningGau/blog_images/master/20190524/SRA_2.png)

我把`sratoolkit`用python做了个简单的封装。可以直接通过[这个脚本](<https://github.com/JarningGau/sra_download>)下载相应数据。

---

以上简单介绍了如何通过`GSE ID`下载对应的原始数据。下面简单介绍一下SRA数据库中几个常用的ID。

- `SRA`，用户进行一次有效的数据上传就会产生一个`SRA ID`，通常我们不使用这个ID。
- `SRX`，这是SRA数据库的最小记录单位。一个`SRX`编号对应一次实验。和`GSM ID`对应。
- `SRP`，SRA study，和`GSE ID`对应。
- `SRR`，这个ID对应真实的测序数据。每一个实验可能对应多个`SRR ID`，因为有时候测序仪RUN一次无法产生足够的数据，比如HiC数据。

下表时SRA数据库的官方解释。

|                  |                          |                                                              |                                                              |
| ---------------- | ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Accession Prefix | Accession Name           | Definition                                                   | Example                                                      |
| SRA              | SRA submission accession | The submission accession represents a virtual container that holds the [object](http://en.wikipedia.org/wiki/Object_%28object-oriented_programming%29)s represented by the other five accessions and is used to track the submission in the archive. | Since the SRA accession number is an artificial packaging construct, there is no example available since the SRA accession number has no specific response page |
| SRP              | SRA study accession      | A Study is an [object](http://en.wikipedia.org/wiki/Object_%28object-oriented_programming%29) that contains the project metadata describing a sequencing study or project. Imported from BioProject. | [HTML](http://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?study=SRP000124) |
| SRX              | SRA experiment accession | An Experiment is an [object](http://en.wikipedia.org/wiki/Object_%28object-oriented_programming%29) that contains the metadata describing the library, platform selection, and processing parameters involved in a particular sequencing experiment. | [HTML](https://www.ncbi.nlm.nih.gov/sra/SRX000193?report=full) |
| SRR              | SRA run accession        | A Run is an [object](http://en.wikipedia.org/wiki/Object_%28object-oriented_programming%29) that contains actual sequencing data for a particular sequencing experiment. Experiments may contain many Runs depending on the number of sequencing instrument runs that were needed. | [HTML](http://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?cmd=viewer&m=data&s=viewer&run=SRR001030) |
| SRS              | SRA sample accession     | A Sample is an [object](http://en.wikipedia.org/wiki/Object_%28object-oriented_programming%29) that contains the metadata describing the physical sample upon which a sequencing experiment was performed. Imported from BioSample. | [HTML](https://www.ncbi.nlm.nih.gov/biosample/?term=119)     |
| SRZ              | SRA analysis accession   | An analysis is an [object](http://en.wikipedia.org/wiki/Object_%28object-oriented_programming%29) that contains a sequence data analysis BAM file and the metadata describing the sequence analysis. |                                                              |

【备注】

- 人的基因组数据通常是限制访问的，一般需要通过`dbGap`数据库来获取访问权限。

## Reference

[About GEO DataSets](<https://www.ncbi.nlm.nih.gov/geo/info/datasets.html>)

[GEO DataSets的数据组织形式](<https://www.ncbi.nlm.nih.gov/geo/info/overview.html#org>)

[SRA Overview](<https://www.ncbi.nlm.nih.gov/sra/docs/>)

[SRA Handbook](<https://www.ncbi.nlm.nih.gov/books/NBK56913/>)