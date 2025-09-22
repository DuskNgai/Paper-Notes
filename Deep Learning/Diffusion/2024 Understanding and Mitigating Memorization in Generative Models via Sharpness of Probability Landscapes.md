# Understanding and Mitigating Memorization in Generative Models via Sharpness of Probability Landscapes

[论文链接](https://arxiv.org/pdf/2412.04140) | 机构：延世大学

## Abstract

我们引入了一种几何框架，通过对数概率密度的**锐度（Sharpness）**来分析扩散模式中的记忆。我们从数学角度证明了之前提出的基于分数差异的记忆度量方法在量化锐度方面的有效性。此外，我们还提出了一种新的记忆度量方法，该方法能在 LDM 中图像生成的初始阶段捕捉锐度，从而为潜在的记忆提供早期洞察。利用这一指标，我们开发了一种缓解策略，利用锐度感知正则项优化生成过程的初始噪声。

## Claims

- 通过提出一个基于概率密度锐度的几何框架，分析扩散模型中的记忆化现象。具体来说，通过研究对数概率密度的 Hessian 矩阵的特征值来量化记忆化。
- 提出一种新的记忆化度量方法，能够在图像生成的早期阶段捕捉到潜在的记忆化，从而提供早期的洞察。
- 基于提出的度量方法，开发一种缓解策略，通过优化生成过程的初始噪声来减少记忆化，而无需重新训练模型或修改提示（prompt）。

## Motivations

- Diffusion 模型存在记忆化问题，即模型会复制训练样本而不是生成新的输出。这在模型处理敏感数据时可能导致隐私风险。
- Hessian 的特征向量定义了曲率的主轴，而相应的特征值则描述了沿这些方向的曲率。正特征值表示局部凸起，负特征值表示局部凹陷，而零特征值则表示这些方向的平坦性。特征值的大小反映了曲率的陡峭程度，绝对值越大，表示函数的变化越陡峭。

## Methods

定义：
- $s(x) = \nabla_{x} \log p(x)$，表示对数概率密度函数的梯度，即 score 函数。
- $H(x) = \nabla_{x}^2 \log p(x)$，表示对数概率密度函数的 Hessian 矩阵。

对 Diffusion 模型的网络计算导数，也就是对数概率密度函数的 Hessian，还是计算量太大了。因此转向研究 Hessian 矩阵的迹，有如下的两个引理：

> 对于 $x \sim \mathcal{N}(\mu, \Sigma)$，有
> $$
> \mathbb{E}_{x}[\|s(x)\|^{2}] = -\mathop{\mathrm{tr}}(H(x))
> $$
> 其中 $H(x) = -\Sigma^{-1}$。对于一般的分布，如果满足 $\mathbb{E}_{x}[\|s(x)\|^{2}] < \infty$ 且 $\lim_{\|x\| \to \infty} p(x) s(x) = \mathbf{0}$，则上述引理仍然成立。

以及

> 对于 $x \sim \mathcal{N}(\mu, \Sigma)$ 且 $x \mid c \sim \mathcal{N}(\mu_c, \Sigma_c)$，有
> $$
> \mathbb{E}_{x \sim p(x \mid c)}[\|s(x, c) - s(x)\|^{2}] = \|H(x)(\mu - \mu_c)\|^{2} + \mathop{\mathrm{tr}}((H(x) - H_{c}(x))^{2} H_{c}^{-1}(x))
> $$
> 其中 $H_{c}(x) = -\Sigma_{c}^{-1}$。如果 $\Sigma_{c} \Sigma = \Sigma \Sigma_{c}$，且 $\mu = \mu_{c}$，则结论化简为：
> $$
> \mathbb{E}_{x \sim p(x \mid c)}[\|s(x, c) - s(x)\|^{2}] = \sum_{i = 1}^{d} \frac{(\lambda_i - \lambda_{c,i})^{2}}{\lambda_{c,i}}
> $$
> 其中 $\lambda_i$ 和 $\lambda_{c,i}$ 分别是 $H(x)$ 和 $H_{c}(x)$ 的特征值。

以上是之前衡量模型记忆化的度量方法。本文提出了一种新的度量方法，

> 对于 $x \sim \mathcal{N}(\mu, \Sigma)$，有
> $$
> \mathbb{E}_{x}[\|H(x)s(x)\|^{2}] = -\mathop{\mathrm{tr}}(H(x)^{3})
> $$
> 按照之前的假设，就会有：
> $$
> \mathbb{E}_{x}[\|H(x, c)s(x, c) - H(x)s(x)\|^{2}] = \sum_{i = 1}^{d} \frac{(\lambda_i - \lambda_{c,i})^{4}}{\lambda_{c,i}^{4}}
> $$

相比之前的，放大了特征值的差异，能够更好地捕捉到记忆化。
