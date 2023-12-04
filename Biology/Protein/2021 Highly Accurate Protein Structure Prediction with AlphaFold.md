# Highly Accurate Protein Structure Prediction with AlphaFold

## 0 Abstract

作者首先表明了蛋白质的重要性以及理解它们结构的价值（化学、生物的核心：结构决定性质）。接着，作者指出了目前蛋白质结构研究中的一个难题，即蛋白质折叠问题。该问题受实验方法的繁琐所限制，对计算方法存在需求。为此提出了 AlphaFold2，首个能够以原子精度（即 1 $\AA$ 的精度）预测蛋白质结构的计算方法，尤其是在没有已知类似结构的情况下。最后，AlphaFold2 在 CASP14 做了验证。

## 1 Introduction

第一段的明确指出了蛋白质结构预测的两条主要途径：热力学相互作用和蛋白质的进化历史。热力学方法计算量巨大，准确性和规模受限；进化历史方法通过分析蛋白质的进化历史、同源结构和进化相关性来推断结构。在缺乏密切同源结构的情况下，这两种方法通常无法达到实验水平的准确性，展现了现有方法的局限性。

第二段提出了 AlphaFold2，介绍了 CASP 大赛。

第三段表示说，在 CASP14 中，AlphaFold2 的结构预测结果远超其他竞争方法， 不仅在领域结构（domain structures）上非常精确，还能在主链非常准确的情况下产生高度精确的侧链。即使在有强模板可用的情况下，它的表现也大大优于基于模板的方法。AlphaFold2 能够处理非常长的蛋白质，准确预测领域和领域间的相互作用。最后，该模型能够提供每个残基精确的可靠性估计，从而使这些预测可以被可靠地使用。

| Method | Median Backbone Accuracy | All-atom Accuracy |
|:-:|:-:|:-:|
| AlphaFold2 | $0.96\AA$ | $1.5\AA$ |
| AlphaFold2 | $2.8\AA$ | $3.5\AA$ |

第四段表示说，AlphaFold2 在新的数据上效果仍然不错。介绍了 3 种 metrics：
1. **Predicted local-distance difference test (pLDDT)**
2. **C$\alpha$ local-distance difference test (lDDT-C$\alpha$)**
3. **Template modelling score (TM-score)**

## 2 The AlphaFold Network

![alphafold2](images/alphafold2.png)

| Symbols | Descriptions |
|:-:|:-:|
| $s$ | Number of Sequences |
| $r$ | Number of Residuals in Sequences aligned to the same length |
| $c$ | Number of Features |

AlphaFold2 的输入是一段氨基酸序列，输出每个氨基酸在三维空间中的位置。它由三部分组成，Feature Extratcor、Encoder 和 Decoder。
1. Feature Extractor 按照给定的输入生成以下特征：
	1. 从基因数据库中抽取同源蛋白质序列组成多序列比对（Multi-Sequence Alignment, MSA），形成一个 $[s,r,c]$ 大小的矩阵。对于位于 $(s_i,r_j)$ 的一个特征，表示它和第 $i$ 条序列以及第 $j$ 个氨基酸残基的相对关系。
	2. 形成一个 Pair 矩阵表示输入序列内部的关系，形成一个 $[r,r,c]$ 大小的矩阵。对于位于 $(r_i,r_j)$ 的一个特征，表示第 $i$ 条氨基酸残基以及第 $j$ 个氨基酸残基的相对关系。
	3. 从结构数据库中抽取模版，即相似结构中氨基酸的空间距离。
2. Encoder 由 48 个 Evoformer 块组成，每个块类似于一个 Transformer Encoder 块，输入和输出大小一致。详细见 [Evoformer](#3%20Evoformer)。
3. Decoder 由 8 个 Structure Module 块组成，。
4. Recycling 机制。将 Encoder 和 Decoder 的输出再次送入 Encoder，相当于将网络复制了多次。

## 3 Evoformer

![evoformer](images/evoformer.png)

Evoformer 将蛋白质结构预测问题看作是 3D 空间中的图推理问题，图中的边由相邻的残基定义。

### 3.1 MSA Row-wise Gated Self-Attention with Pair Bias

![algorithm 7](images/algorithm-7.png)

```python
def MSA_row_attention_with_pair_bias(
    msa: torch.Tensor,
    pair: torch.Tensor
):
    # msa: [s, r, c_m]
    # pair: [r, r, c_z]
    msa = 
```

## 4 End-to-end Structure Prediction

## 5 Training with Labelled and Unlabelled Data

## 6 Interpreting the Neural Network

## 7 MSA Depth and Cross-chain Contacts

