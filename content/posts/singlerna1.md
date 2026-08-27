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

10xGenomics Chromium scRNA-seq是单细胞领域最常见的测序平台

单细胞测序产生的原始数据不是Seurat对象，是文件集合，例如有

barcodes.tsv.gz，是细胞条形码cell barcode，每个细胞条形码不同——多少细胞就有多少barcode

features.tsv.gz，基因名称

matrix.mtx.gz，是表达矩阵，每个细胞里面检测到了多少UMI，而不是RNA数量

UMI 是一小段随机的核苷酸序列（比如由10-12 个随机碱基组成，如AGCT...），在构建测序文库时，研究人员会把它连接到每一个 mRNA 片段上。因为序列是随机合成的，理论上每一个 mRNA 分子拿到的“二维码”都是不同的。在PCR扩增后，UMI让我们能够去重，得到原始样品中真实的某mRNA绝对分子计数。

## 操作

下载并加载Seurat包

首先读取10x数据

```r
scRNA.counts <- Read10X（data.dir = "C:/Users/29133/Desktop/Source/GSE152048_BC21.matrix/BC21"）
```

得到dgCMatrix稀疏矩阵，例如20000genes × 5000cell

![读取后的文件内容](/singlerna1photo/singlerna1photo1.png)


> 一

**n

- n

```r
scRNA <- NormalizeData(scRNA)

scRNA <- FindVariableFeatures(
  scRNA,
  selection.method = "vst",
  nfeatures = 2000
)
```