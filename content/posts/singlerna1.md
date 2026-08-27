+++
title = "单细胞分析单样本分析流程"
date = "2026-08-27"
draft = false

categories = ["单细胞RNA-seq"]
tags = ["Seurat", "R", "SCTransform"]

description = "单细胞分析单样本的流程细节及代码函数解释"
+++

# nCount_RNA 到底是什么？

在 Seurat 的单细胞分析中，经常会看到：

- nCount_RNA
- nFeature_RNA

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