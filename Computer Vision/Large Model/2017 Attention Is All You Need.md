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
