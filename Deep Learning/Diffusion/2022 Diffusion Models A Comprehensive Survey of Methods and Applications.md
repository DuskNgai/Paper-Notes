# 2022 Diffusion Models: A Comprehensive Survey of Methods and Applications

## 2 Foundations of Diffusion Models

### 2.1 Denoising Diffusion Probabilistic Models (DDPMs)

从数据分布 $q(\mathbf{x}_{0})$ 中采样一个初始样本 $\mathbf{x}_{0}$，前向 Markov 过程通过转移核 $q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})$ 生成一系列随机变量 $\mathbf{x}_{1}, \ldots, \mathbf{x}_{T}$。基于条件概率公式和 Markov 过程的性质，可以得到生成随机变量的联合概率密度函数：
$$
q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0}) = \prod_{t=1}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1}).
$$

DDPM 是手动设计 $q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})$ 的：
$$
q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1}) = \mathcal{N}(\mathbf{x}_{t}; \sqrt{1-\beta_{t}}\mathbf{x}_{t-1}, \beta_{t}\mathbf{I}),
$$
其中 $\beta_{t}\in(0,1)$ 是提前设定的超参数。此外，定义 $\alpha_{t} = 1-\beta_{t}$ 以及 $\bar{\alpha}_{t} = \prod_{s=0}^{t}\alpha_{s}$，则：
$$
q(\mathbf{x}_{t}\mid\mathbf{x}_{0}) = \mathcal{N}(\mathbf{x}_{t}; \sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0}, (1-\bar{\alpha}_{t})\mathbf{I}).
$$

因此给定 $\mathbf{x}_{0}$，可以直接得到 $\mathbf{x}_{t}$ 的采样：
$$
\mathbf{x}_{t} = \sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0} + \sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon},
$$
其中 $\boldsymbol{\epsilon}\sim\mathcal{N}(\mathbf{0}, \mathbf{I})$。

当 $\bar{\alpha}_{T}\approx0$ 时，$\mathbf{x}_{T}$ 的分布接近于标准正态分布，即：
$$
q(\mathbf{x}_{T}\mid\mathbf{x}_{0}) = \int q(\mathbf{x}_{T}\mid\mathbf{x}_{0})q(\mathbf{x}_{0})\mathrm{d}\mathbf{x}_{0} \approx \mathcal{N}(\mathbf{x}_{T}; \mathbf{0}, \mathbf{I}).
$$

反向 Markov 过程由先验分布 $p(\mathbf{x}_{T}) \sim \mathcal{N}(\mathbf{0}, \mathbf{I})$ 以及可学习的转移核 $p_{\theta}(\mathbf{x}_{t-1}\mid\mathbf{x}_{t})$ 来建模，其中 $\theta$ 是模型参数。该转移核的形式通常为：
$$
p_{\theta}(\mathbf{x}_{t-1}\mid\mathbf{x}_{t}) = \mathcal{N}(\mathbf{x}_{t-1}; \mu_{\theta}(\mathbf{x}_{t}, t), \Sigma_{\theta}(\mathbf{x}_{t}, t)),
$$

这一采样过程成功的关键在于训练了与正向 Markov 链的具体时间步反转相匹配的反向 Markov 链。即 $p_{\theta}(x_{0:T}) = p(x_{T})\prod_{t=1}^{T}p_{\theta}(x_{t-1}\mid x_{t})$ 与 $q(x_{0:T}) = q(x_{0})\prod_{t=1}^{T}q(x_{t}\mid x_{t-1})$ 之间的匹配。为了做到这一点，可以通过最小化二者之间的 KL 散度来训练模型：
$$
\begin{align*}
\mathrm{KL}(q(\mathbf{x}_{0:T})\|p_{\theta}(\mathbf{x}_{0:T})) &= -\mathbb{E}_{q(\mathbf{x}_{0:T})}\left[\log p(\mathbf{x}_{0:T})\right] + \text{const} \\
&= -\mathbb{E}_{q(\mathbf{x}_{0:T})}\left[\log p(\mathbf{x}_{T}) + \sum_{t=1}^{T}\log\frac{p_{\theta}(\mathbf{x}_{t-1}\mid\mathbf{x}_{t})}{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right] + \text{const} \\
&\ge -\mathbb{E}_{q(\mathbf{x}_{0:T})}\left[\log p_{\theta}(\mathbf{x}_{0})\right] + \text{const} \\
\end{align*}
$$
其中 $\text{const}$ 是与 $\theta$ 无关的常数。最后一步是由于 Jensen 不等式。

### 2.2 Score-Based Generative Models (SGMs)

定义 (Stein) score 函数为 $\nabla_{\mathbf{x}}\log p_{\theta}(\mathbf{x})$，其中 $\log p_{\theta}(\mathbf{x})$ 是模型的对数概率密度函数。

### 2.3 Stochastic Differential Equations (Score SDEs)

$$
\mathrm{d}\mathbf{x}=\mathbf{f}(\mathbf{x}, t)\mathrm{d}t + g(t)\mathrm{d}\mathbf{w},
$$

对于 DDPM，对应的 SDE 为：
$$
\mathrm{d}\mathbf{x} = -\frac{1}{2}\beta(t)\mathbf{x}\mathrm{d}t + \sqrt{\beta(t)}\mathrm{d}\mathbf{w},
$$
其中 $\beta(t/T)=T\beta_{t}$ 当 $T\to\infty$ 时。

对于 SGM，对应的 SDE 为：
$$
\mathrm{d}\mathbf{x} = \sqrt{\frac{\mathrm{d}[\sigma(t)^2]}{\mathrm{d}t}}\mathrm{d}\mathbf{w},
$$
其中 $\sigma(t/T)=\sigma_{t}$ 当 $T\to\infty$ 时。

