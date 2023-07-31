# Attention Is All You Need

Transformer 处理序列输入，序列输出。其中输入和输出的可以不等长。

自注意力机制 Self-Attention Mechanism

用自注意力机制处理序列中每个输入之间的关系，用全连接层处理单个输出。
$$
Q=W^qA, K=W^kA, V=W^vA\\
O=\mathrm{Softmax}(Q^TK)V
$$
多头自注意力机制 Multi-Head Self-Attention Mechanism

多个输出，再过一次全连接得到单个输出。

位置编码 Positional Encoding

给自注意力加入位置信息

Encoder 由多个 Attention Block 组成

Autoregressive

Decoder 输出时候仿照 RNN 的思想，只让前面的序列预测后面的序列。

Non-Autoregressive

交叉注意力机制 Cross-Attention Mechanism

连接 Transformer Encoder 和 Decoder 的模块。

Query 来自于 Decoder，Key 和 Value 来自于 Encoder。

## 0 Abstract

我们提出了一种新的简单网络架构——Transformer，它完全基于注意力机制，无需循环和卷积。
We propose a new simple network architecture, the Transformer, based solely on attention mechanisms, dispensing with recurrence and convolutions entirely.

## 1 Introduction

循环模型通常按照输入和输出序列的符号位置进行计算。它们将位置与计算时间的步长对齐，生成隐藏状态 $h_{t}$ 的序列，作为前一个隐藏状态 $h_{t-1}$ 和位置 $t$ 的输入的函数。**这种固有的序列性质排除了在训练实例中进行并行化的可能性，而在序列长度较长的情况下，这一点变得至关重要，因为内存约束限制了跨实例的批处理。**
Recurrent models typically factor computation along the symbol positions of the input and output sequences. Aligning the positions to steps in computation time, they generate a sequence of hidden states $h_{t}$, as a function of the previous hidden state $h_{t−1}$ and the input for position $t$. **This inherently sequential nature precludes parallelization within training examples, which becomes critical at longer sequence lengths, as memory constraints limit batching across examples.**

在各种任务中，注意力机制已成为引人注目的序列建模和转导模型的一个组成部分，它可以对依赖关系进行建模，而无需考虑其在输入或输出序列中的距离。然而，除了少数情况，这种注意机制都是与循环网络结合使用的。
Attention mechanisms have become an integral part of compelling sequence modeling and transduction models in various tasks, allowing modeling of dependencies without regard to their distance in the input or output sequences. In all but a few cases, however, such attention mechanisms are used in conjunction with a recurrent network.

## 2 Background

## 3 Model Architecture


