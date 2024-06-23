# RoFormer: Enhanced Transformer with Rotary Position Embedding

为了让 Attention 机制能够捕捉到输入的顺序，即区分不同位置的 token，因此我们需要位置编码，破坏 Attention 的对称性。位置编码有两种选择，绝对位置编码和相对位置编码。

## Positional Embedding

### Absolute Positional Embedding

在输入的第 $i$ 个 token $\mathbf{x}_{i}$ 上加入位置向量 $\mathbf{p}_{i}$，其中 $\mathbf{p}_{i}$ 只依赖于位置编号 $i$。常见的绝对位置编码有
1. 可训练的位置编码：朴素，但使得模型丧失外推能力。
2. 三角函数位置编码：Transformer 原版论文中提出的，有一定外推性。

### Relative Positional Embedding

这里我们着重介绍相对位置编码。相对位置编码是通过考虑两个 token 之间的相对距离 $|i-j|$ 来产生位置编码。考虑一般的带绝对位置编码的 Attention：
$$\begin{cases}
\mathbf{q}_i&=(\mathbf{x}_i+\mathbf{p}_i)W_Q\\
\mathbf{k}_i&=(\mathbf{x}_i+\mathbf{p}_i)W_K\\
\mathbf{v}_i&=(\mathbf{x}_i+\mathbf{p}_i)W_V\\
\end{cases}$$
展开 $\mathbf{q}_i\mathbf{k}_j^T$ 得到
$$\begin{aligned}
\mathbf{q}_i\mathbf{k}_j^T&=(\mathbf{x}_i+\mathbf{p}_i)W_QW_K^T(\mathbf{x}_j+\mathbf{p}_j)^T\\
&=(\mathbf{x}_iW_Q+\mathbf{p}_iW_Q)(W_K^T\mathbf{x}_j^T+W_K^T\mathbf{p}_j^T)\\
&=\mathbf{x}_iW_QW_K^T\mathbf{x}_j^T+\mathbf{x}_iW_QW_K^T\mathbf{p}_j^T+\mathbf{p}_iW_QW_K^T\mathbf{x}_j^T+\mathbf{p}_iW_QW_K^T\mathbf{p}_j^T
\end{aligned}$$
因此第一项计算了两个 token 之间的关系，后面三项都和位置有关。因此各种工作都对后面三项的形式作出了保留、舍弃、修改。相关的工作有
1. Transformer-XL：修改为 $\mathbf{x}_iW_QW_K^T\mathbf{x}_j^T+\mathbf{x}_iW_QW_{K,R}^T\mathbf{R}_{i-j}^T+\mathbf{u}W_K^T\mathbf{x}_j^T+\mathbf{v}W_{K,R}^T\mathbf{R}_{i-j}^T$，其中 $\mathbf{R}_{i-j}$ 是相对位置向量，$W_{K,R}$ 是编码位置空间的矩阵，$\mathbf{u},\mathbf{v}$ 是合并了 $\mathbf{p}_iW_Q$ 的可学习向量。
2. T5：修改为 $\mathbf{x}_iW_QW_K^T\mathbf{x}_j^T+\beta_{i,j}$，其中 $\beta_{i,j}$ 是有所设计的可学习向量。
3. DeBERTa：修改为 $\mathbf{x}_iW_QW_K^T\mathbf{x}_j^T+\mathbf{x}_iW_QW_K^T\mathbf{R}_{i,j}^T+\mathbf{R}_{j,i}W_QW_K^T\mathbf{x}_j^T$，其中 $\mathbf{R}_{i,j}$ 是一个二元位置向量。

## Rotary Positional Embedding (RoPE)

RoPE 的目的是通过绝对位置编码的方式实现相对位置编码。

### Formulation

函数 $f:\mathbb{R}^{n}\times\mathbb{Z}\mapsto\mathbb{R}^{n}$ 负责给一个 token 添加绝对位置编码，**RoPE 希望经过内积运算，使得内积运算的结果带有相对位置信息**，即
$$
\langle f(\mathbf{q}_m,m),f(\mathbf{k}_n,n)\rangle=g(\mathbf{q}_m,\mathbf{k}_n,m-n)
$$
任务是求出一个尽可能简单的函数 $g$。为了不和虚数单位 $i$ 起冲突，这里换成 $m,n$。

### 2D Case

假设 $\mathbf{q}_m$ 和 $\mathbf{k}_n$ 是 2D 向量，我们把它看成是两个复数，那么内积就可以表示为
$$
\langle\mathbf{q}_m,\mathbf{k}_n\rangle=\Re[\mathbf{q}_m\mathbf{k}_n^\dagger]
$$
其中 $\dagger$ 表示转置共轭。这个公式表示说：两个二维向量的内积，等于把它们当复数看时，一个复数与另一个复数的共轭的乘积实部。那么就有：
$$
\mathfrak{R}[f(\mathbf{q}_m,m)f^{\dagger}(\mathbf{k}_n,n)]=g(\mathbf{q}_m,\mathbf{k}_n,m-n)
$$
假设存在复数的 $g(\mathbf{q}_m,\mathbf{k}_n,m-n)$，那么可以把 $\mathfrak{R}$ 拿掉，并且改写为复数的幅角形式
$$
\begin{align*}
f(\mathbf{q}_m,m)f^{\dagger}(\mathbf{k}_n,n)&=f_{\text{abs}}(\mathbf{q}_m,m)\exp(if_{\text{angle}}(\mathbf{q}_m,m))f_{\text{abs}}(\mathbf{k}_n,n)\exp(-if_{\text{angle}}(\mathbf{k}_n,n))\\
&=f_{\text{abs}}(\mathbf{q}_m,m)f_{\text{abs}}(\mathbf{k}_n,n)\exp(i[f_{\text{angle}}(\mathbf{q}_m,m)-f_{\text{angle}}(\mathbf{k}_n,n)])\\
&=g_{\text{abs}}(\mathbf{q}_m,\mathbf{k}_n,m-n)\exp(ig_{\text{angle}}(\mathbf{q}_m,\mathbf{k}_n,m-n))
\end{align*}
$$
因此就有：
$$
\begin{align*}
f_{\text{abs}}(\mathbf{q}_m,m)f_{\text{abs}}(\mathbf{k}_n,n)&=g_{\text{abs}}(\mathbf{q}_m,\mathbf{k}_n,m-n)\\
f_{\text{angle}}(\mathbf{q}_m,m)-f_{\text{angle}}(\mathbf{k}_n,n)&=g_{\text{angle}}(\mathbf{q}_m,\mathbf{k}_n,m-n)
\end{align*}
$$
作者在这里想到了一个形式简单的函数 $f(\mathbf{x},m)=\mathbf{x}\exp(im\theta)$，既可以简单的添加位置信息，也可以得到形式简单的 $g$：
$$
\begin{align*}
\langle f(\mathbf{q}_m,m),f(\mathbf{k}_n,n)\rangle&=\langle \mathbf{q}_m\exp(im\theta),\mathbf{k}_n\exp(in\theta)\rangle\\
&=\Re[\mathbf{q}_m\exp(im\theta)\mathbf{k}_n^\dagger\exp(-in\theta)]\\
&=\Re[\mathbf{q}_m\mathbf{k}_n^\dagger\exp(i(m-n)\theta)]\\
&=g(\mathbf{q}_m,\mathbf{k}_n,m-n)
\end{align*}
$$
尽管把 2D 向量看作了复数，但是一切都可以在实数框架下完成。2D 情况下，函数 $f$ 满足
$$
\begin{align*}
f(x,y,m)=(x+iy)\exp(im\theta)&=(x+iy)(\cos m\theta+i\sin m\theta)\\
&=(x\cos m\theta-y\sin m\theta)+i(x\sin m\theta+y\cos m\theta)
\end{align*}
$$
也就是意味着
$$
f(x,y,m)=(x\cos m\theta-y\sin m\theta,x\sin m\theta+y\cos m\theta)=\begin{bmatrix}\cos m\theta&-\sin m\theta\\\sin m\theta&\cos m\theta\end{bmatrix}\begin{bmatrix}x\\y\end{bmatrix}
$$
刚好可以用旋转矩阵表达。

### General Case

拓展到任意维度 $d$ 的时候，创建一个这样的矩阵即可：
$$
R(m; \Theta, d) = \begin{bmatrix}
\cos m\theta_1&-\sin m\theta_1&0&0&\cdots&0&0\\\sin m\theta_1&\cos m\theta_1&0&0&\cdots&0&0\\
0&0&\cos m\theta_2&-\sin m\theta_2&\cdots&0&0\\0&0&\sin m\theta_2&\cos m\theta_2&\cdots&0&0\\
\vdots&\vdots&\vdots&\vdots&\ddots&\vdots&\vdots\\
0&0&0&0&\cdots&\cos m\theta_{d/2}&-\sin m\theta_{d/2}\\0&0&0&0&\cdots&\sin m\theta_{d/2}&\cos m\theta_{d/2}\end{bmatrix}
$$
其中 $\Theta=\{\theta_i=10000^{-2(i-1)/d}\mid i\in\{1,\dots,d/2\}\}$。带入到 $\mathbf{q}_i\mathbf{k}_j^T$ 得到：
$$\begin{aligned} \langle f(\mathbf{q}_m,m),f(\mathbf{k}_n,n)\rangle&=\langle R(\Theta,m,d)\mathbf{q}_m,R(\Theta,n,d)\mathbf{k}_n\rangle\\ &=\mathbf{q}_mR(\Theta,m,d)R(\Theta,n,d)^T\mathbf{k}_n^T\\ &=\mathbf{q}_mR(\Theta,{\color{red}n-m},d)\mathbf{k}_n^T\\ &=g(\mathbf{q}_m,\mathbf{k}_n,m-n) \end{aligned}$$
满足所需的形式。由于这个矩阵非常稀疏，因此最好实现方式是展开相乘。

## Advantages

RoPE 的优势有

1. 灵活的序列长度。
2. 随着相对距离的增加 token 之间依赖性的衰减。（证明略）
3. 为线性自注意力机制配备相对位置编码的能力。（证明略）

RoPE 现已被用在 llama2, glm 等 LLM 上。