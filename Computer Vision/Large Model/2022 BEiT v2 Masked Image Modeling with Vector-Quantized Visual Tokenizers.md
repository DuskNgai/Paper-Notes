# BEiT v2: Masked Image Modeling with Vector-Quantized Visual Tokenizers

## 0 Abstract

BEiT 想利用图像的高阶语义。BEiT 提出，使用语义信息丰富的视觉 token 作为掩码预测的重构目标。具体来说，BEiT

1. 用向量量化知识蒸馏来训练 tokenizer（tokenizer 将连续的语义空间离散化为紧凑空间）
2. 通过预测掩码图像块的原始视觉标记来预训练 ViT。
3. 提出图像块聚合策略。

## 1 Introduction

BEiT 认为，当前的重建目标有三种

1. 低级图像语义：原始图像像素。
2. 手工设计特征：图像梯度。
3. 视觉 token：VQ-VAE 的潜空间。

都是为了让模型从低语义信息中学到高语义信息。但是 NLP 里面到是从高语义到高语义的学习。

BEiT 提出的贡献点是

1. 向量量化知识蒸馏（VQ-KD）来离散化语义空间，将低级语义升级为高级语义，作为 ViT 的训练目标。
2. 图像块聚合策略。

## 2 Methodology

### 2.1 Image Representation

|                                           Symbols                                            |                   Descriptions                   |
| :------------------------------------------------------------------------------------------: | :----------------------------------------------: |
|                        $\mathbf{x}\in\mathbb{R}^{H\times W\times C}$                         |                      image                       |
| $\left\{\mathbf{p}_{i}\in\mathbb{R}^{N\times (P^2\times C)}\right\}_{i=1}^{N=H\times W/P^2}$ |                  image patches                   |
|                                $\{\mathbf{h}_{i}\}_{i=1}^{N}$                                |             image vectors after ViT              |
|                        $\left\{z_{i}\in\mathbb{Z}\right\}_{i=1}^{N}$                         |              index of visual tokens              |
|            $\mathcal{V}=\left\{\mathbf{v}_{k}\in\mathbb{R}^{D}\right\}_{k=1}^{K}$            | visual vocabulary, $K$ alphabet, each length $D$ |

### 2.2 Training Visual Tokenizer

Visual Tokenizer 包含了一个 ViT 和量化器。ViT 先把图像 patch $\mathbf{p}_{i}$ 映射成向量 $\mathbf{h}_{i}$，然后量化器去 $\mathcal{V}$ 之中找到最接近的离散编码 $\mathbf{v}_{j}$：
$$
z_{i}=\mathop{\arg\min}_{j}\|\ell_2(\mathbf{h}_{i})-\ell_2(\mathbf{v}_{j})\|
$$
$\ell_2$ 是二范数归一化。以上的公式也等价于用 cos 相似性找最近的编码。随后，把二范数归一化的离散编码 $\{\ell_2(\mathbf{v}_{z_i})\}_{i=1}^{N}$  送入解码器，输出 $\{\mathbf{o}_{i}\}_{i=1}^{N}$，解码器也是个多层 Transformer。输出目的是拟合教师模型（DINO、CLIP）的语义特征，用 cos 相似性作为 loss。其他的训练方式类似原版 VQ-VAE：
$$
\max\sum_{\mathbf{x}\in\mathcal{D}}\sum_{i=1}^{N}\cos(\mathbf{o}_{i},\mathbf{o}_{i}')-\|\mathrm{sg}[\ell_2(\mathbf{h}_{i})]-\ell_2(\mathbf{v}_{z_i})\|-\|\ell_2(\mathbf{h}_{i})-\mathrm{sg}[\ell_2(\mathbf{v}_{z_i})]\|
$$
Codebook 的维度设置为 $K=8192,D=32$。解码器的第一层会把低维离散嵌入映射到高维。此外还用了 EMA。

### 2.3 Pretraining BEiT v2

![](images/beitv2.png)

40% 的 mask 比例，送入网络的有 image patch 、可学习的 mask token、class token。最后一层是一个分类头，分类最后属于哪个 visual token。

为了捕获图像的全局信息，还预训练了 class token。为了预训练最后一层的 class token，把它和 ViT 第 $l$ 层的输出结合送入到一个两层 ViT + 同一个分类头做一次分类任务。因为丢弃了后续层的 token，但是输出头还是同一个，就迫使 class token 学习更多全局的信息。这里的假设是越接近输入层，信息越局部。
