# Vector Quantized Variational Auto-Encoder

VQ-VAE 的出现需要追溯到自回归（Auto-Regressive）模型。我们可以把一张图像看作是一个 $H\times W\times C$ 长度的句子，句子中词语的词表大小为 $256$。因此我们可以利用自回归模型的方式逐像素逐通道地生成图像，即
$$
\mathbb{Pr}(\mathbf{x})=\mathbb{Pr}(\mathbf{x}_{1})\mathbb{Pr}(\mathbf{x}_{2}\mid\mathbf{x}_{1})\cdots\mathbb{Pr}(\mathbf{x}_{H\times W\times C-1}\mid\mathbf{x}_{1},\dots,\mathbf{x}_{H\times W\times C-2})
$$
其中 $\mathbb{Pr}(\mathbf{x}_{1}),\mathbb{Pr}(\mathbf{x}_{2}\mid\mathbf{x}_{1}),\cdots,\mathbb{Pr}(\mathbf{x}_{H\times W\times C-1}\mid\mathbf{x}_{1},\dots,\mathbf{x}_{H\times W\times C-2})$ 每个都是一个 256 分类问题。

自回归模型在 GPT 系列的 LLM 上已经被证明是非常有效的，但是最致命的缺陷就是 $O(n^2)$ 的计算量和存储量，即慢。LLM 在 2024 年才能做到百万长度的序列的推理，换算成图片的大小却不到 $640\times640\times3$，很显然是不能满足生成高清图片的需求的。

因此 VQ-VAE 针对自回归模型在图像上存在的问题，提出了先降维，然后在隐空间上进行自回归的解决方法。

## Network

VQ-VAE 中，变分分布 $q(z\mid\mathbf{x};\phi)$ 被假设成离散的分布
$$
q(z=k\mid\mathbf{x};\phi)=\begin{cases}1&k=\mathop{\arg\min}_j\|\phi(x)-e_{j}\|\\0&\text{otherwise}\end{cases}
$$
其中 $\{e_{j}\}_{j=1}^{K}$ 是一系列预设的、可训练的编码。相应的，规定先验分布 $p(\mathbf{z};\theta)$ 是离散均匀分布 $U(1,K)$，则它们的 KL 散度为
$$
\mathrm{KL}(q\parallel p)=\sum_{k=1}^{K}q(k)\log\frac{q(k)}{p(k)}=-1\cdot\log\frac{1}{K}+(K-1)\cdot0\cdot\log\frac{1}{K}=\log K
$$
是一个常数。最后送入解码器的 $z_q$ 就是距离编码器得到的结果最近的一个离散编码 $e_k$。

以上的推导中，每个输入只会产生一个对应的离散编码，这提高了网络学习的难度。实际上实现的时候，图像会产生 $(H/P)\times(W/P)$ 个离散编码，此时 $z_q$ 就是对应大小的一个整数矩阵。

## Gradient

VQ-VAE 中，编码器的输出和解码器的输入之间存在 $\mathop{\arg\min}$ 的操作，使得其中的导数链条断开了。