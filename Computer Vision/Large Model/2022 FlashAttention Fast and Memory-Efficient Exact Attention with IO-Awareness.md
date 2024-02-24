# FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness

## 0 Abstract

Transformer 的 Attention 在时间和内存的复杂度上是 $O(N^2)$，其中 $N$ 是序列长度。作者认为提速的一个要点是考虑 GPU 的内存层级，即 IO-awareness。为此提出了考虑 IO 的标准 Attention 计算方法，通过分块来减少 GPU 高带宽内存（HBM）和 GPU 片上 SRAM 之间的内存读/写次数。FlashAttention
1. 访问 HBM 的次数比原版 Attention 少。
2. 适用于不同的 SRAM 大小。
3. 可以扩展到其他的 Attention 近似算法，如 Block Sparse Attention。

|                   Task                   | Speedup |
| :--------------------------------------: | :-----: |
|        BERT-large（512 序列长度）        |  1.15   |
|          GPT-2（1024 序列长度）          |    3    |
| Long Range Arena（1024 - 4096 序列长度） |   2.4   |

FlashAttention 在不同模型和任务上证明了其在加速训练和提升推理效率方面的实际效果。尤其是在处理极长序列的任务上，FlashAttention 不仅提高了处理速度，还让模型能够处理更长的上下文，提升了模型在复杂任务上的表现。

## 1 Introduction

![flash-attention](images/flash-attention.png)

各种 Attention 的近似算法，不管是基于稀疏近似还是基于低秩近似，都不减少实际的运行时间。这是因为它们的关注点在于减少 FLOP 而不是更直接的减少计算时间，甚至可能增加了 IO 开销。

因此 FlashAttention 减少了 HBM 的访问。具体来说，
1. 计算 Softmax 时候无需访问整个输入（？）
2. 不为反向传播存储巨大的 Attention 中间结果。

对此在实现层面，FlashAttention
1. 实现矩阵的分块处理，增量式 Softmax 计算。
2. 用重新计算代替访问 HBM 来获取中间结果。

- 标准 Attention 的 HBM 访问量：$\Omega(Nd +N^2)$
- FlashAttention 的 HBM 访问量：$O(N^2d^2M^{-1})$
- $N$ token 数量，$d$ head 维度，$M$ SRAM 大小。

## 3 FlashAttention: Algorithm, Analysis, and Extensions

### 3.1 An Efficient Attention Algorithm With Tiling and Recomputation





