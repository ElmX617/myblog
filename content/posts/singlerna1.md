+++
title = "单细胞分析单样本分析流程"
date = "2026-08-27"
draft = false

categories = ["单细胞RNA-seq"]
tags = ["Seurat", "R", "SCTransform"]

description = "单细胞分析单样本的流程细节及代码函数解释"
+++

# 原始数据读取

## 概念

原始测序结果raw data导入R环境，以后续Seurat分析。

10xGenomics Chromium scRNA-seq是单细胞领域最常见的平台

单细胞测序产生的原始数据不是一个Seurat对象，而是一堆文件，例如

barcodes.tsv.gz，是细胞条形码cell barcode，每个细胞条形码不同——多少细胞就有多少barcode

features.tsv.gz，基因名称

matrix.mtx.gz，是表达矩阵，每个细胞里面检测到了多少UMI，而不是RNA数量

UMI 是一小段随机的核苷酸序列（比如由10-12 个随机碱基组成，如AGCT...），在构建测序文库时，研究人员会把它连接到每一个 mRNA 片段上。因为序列是随机合成的，理论上每一个 mRNA 分子拿到的“二维码”都是不同的。在PCR扩增后，UMI让我们能够去重，得到原始样品中真实的某mRNA绝对分子计数。

## 操作

下载并加载Seurat包

首先读取10x数据scRNA.counts <- Read10X（data.dir = "路径"）,得到dgCMatrix稀疏矩阵，例如20000genes × 5000cell

![读取后的文件内容](static/singlerna1photo/singlerna1photo1.png)

## nCount_RNA

nCount_RNA 表示一个细胞检测到的 RNA 总量。

简单来说，就是：

> 一个细胞中所有基因对应的 UMI 数量之和。

## nFeature_RNA

nFeature_RNA 表示：

> 一个细胞中检测到的基因数量。

因此：

**nCount_RNA ≈ RNA/UMI总量**

**nFeature_RNA ≈ 检测到的基因种类数**

## 两者的区别

一个细胞可能：

- nCount_RNA 很高
- nFeature_RNA 也很高

说明这个细胞通常具有较高的 RNA 捕获量。

但是如果：

nCount_RNA 特别高，

就需要考虑是否存在 doublet 等情况。

```r
scRNA <- NormalizeData(scRNA)

scRNA <- FindVariableFeatures(
  scRNA,
  selection.method = "vst",
  nfeatures = 2000
)
```