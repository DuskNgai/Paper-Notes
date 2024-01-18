# Diffusion

|     Symbol     |               Description               |
| :------------: | :-------------------------------------: |
|      $q$       | probabilty function in forward process  |
|      $p$       | probabilty function in reversed process |
| $\mathbf{x}_{0}$ |                raw data                 |

其他记号同 [Variational Auto-Encoder](variational%20auto-encoder.md)。

---

Diffusion 模型可以看作 Markovian Hierarchical Variational Auto-Encoder 的特例，即
1. 每个潜空间的维度都是相同的
2. 每个潜空间的先验分布都是线性变换的高斯分布
3. 每个潜空间的先验分布参数不是固定的，这是为了保证最后的潜空间的分布是一个标准高斯分布

### Forward Process

Diffusion 模型的前向过程是**数据逐步加噪**的过程。Diffusion 模型将前向过程看作是一个**马尔可夫链**，即每一步获得的数据都只与前一步的数据有关。给定一份无噪声的数据 $\mathbf{x}_{0}\sim q(\mathbf{x}_{0})$，通过总共 $T$ 步的加 Gaussian 噪声过程，得到一系列的加噪数据 $\left\{\mathbf{x}_t\right\}_{t=1}^T$。前向过程设计成非学习的过程。
$$
q(\mathbf{x}_{1:T}\mid\mathbf{x_{0}})=\prod_{t=1}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})
$$
每一步的加噪过程的方差为 $\{\beta_t\}_{t=1}^T$，得到
$$
\begin{align*}
q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})&=\mathcal{N}(\mathbf{x}_{t};\sqrt{1-\beta_{t}}\mathbf{x}_{t-1},\beta_{t}I)\\
&=\mathcal{N}(\mathbf{x}_{t};\sqrt{\alpha_{t}}\mathbf{x}_{t-1},(1-\alpha_{t})I)
\end{align*}
$$
这里的 $\beta_t$ 是一个超参数，可以是一个常数，也可以是一个随着 $t$ 变化的函数，$\alpha_{t}=1-\beta_{t}$。这样设计加噪过程的目的是，噪声的方差保持不变，这样可以保证模型的稳定性。

前向过程的重要性质是，$\mathbf{x}_t$ 的分布可以由 $\mathbf{x}_{0}$ 直接得到，即
$$
q(\mathbf{x}_{t}\mid\mathbf{x}_{0})=\mathcal{N}(\mathbf{x}_{t};\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0},(1-\bar{\alpha}_{t})I)
$$
其中 $\bar{\alpha}_{t}=\prod_{s=1}^{t}\alpha_{s}$。

证明如下：
$$
\begin{align*}
\mathbf{x}_{t}&=\sqrt{\alpha_{t}}\mathbf{x}_{t-1}+\sqrt{1-\alpha_{t}}\boldsymbol{\epsilon}_{t-1}\\
&=\sqrt{\alpha_{t}}\left(\sqrt{\alpha_{t-1}}\mathbf{x}_{t-2}+\sqrt{1-\alpha_{t-1}}\hat{\boldsymbol{\epsilon}}_{t-2}\right)+\sqrt{1-\alpha_{t}}\boldsymbol{\epsilon}_{t-1}\\
&=\sqrt{\alpha_{t}}\sqrt{\alpha_{t-1}}\mathbf{x}_{t-2}+\sqrt{\alpha_{t}}\sqrt{1-\alpha_{t-1}}\hat{\boldsymbol{\epsilon}}_{t-2}+\sqrt{1-\alpha_{t}}\boldsymbol{\epsilon}_{t-1}&&\text{Add of two Gaussian}\\
&=\sqrt{\alpha_{t}\alpha_{t-1}}\mathbf{x}_{t-2}+\sqrt{\alpha_{t}(1-\alpha_{t-1})+(1-\alpha_{t})}\boldsymbol{\epsilon}_{t-2}\\
&=\sqrt{\alpha_{t}\alpha_{t-1}}\mathbf{x}_{t-2}+\sqrt{1-\alpha_{t}\alpha_{t-1}}\boldsymbol{\epsilon}_{t-2}\\
&=...\\
&=\sqrt{\alpha_{t}\alpha_{t-1}...\alpha_{1}}\mathbf{x}_{0}+\sqrt{1-\alpha_{t}\alpha_{t-1}...\alpha_{1}}\boldsymbol{\epsilon}_{0}\\
&=\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0}+\sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon}_{0}
\end{align*}
$$
其中 $\boldsymbol{\epsilon}_t$ 是在 $t$ 时刻等效的噪声，$\hat{\boldsymbol{\epsilon}}_{t}$ 是原本在 $t$ 时刻加入的噪声。

### Reversed Process

Diffusion 模型的反向过程是**数据逐步去噪**的过程。反向过程设计成模型学习的过程。

$$
p(\mathbf{x}_{0:T};\theta)=p_{\theta}(\mathbf{x}_{T})\prod_{t=1}^{T}p_{\theta}(\mathbf{x}_{t-1}\mid\mathbf{x}_{t})
$$
其中 $p(\mathbf{x}_{T})\sim\mathcal{N}(\mathbf{x}_{T};0,I)$。

这里，我们再次回顾 ELBO 在这套记号下的形式：
$$
\mathrm{ELBO}(q,\mathbf{x};\theta)=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})\prod_{t=1}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{\prod_{t=1}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]
$$
类似于 VAE，我们可以将 ELBO 分解。这里有两种分解方式，首先是存在缺陷的一种分解方式：
$$
\begin{align*}
&\quad\ \mathrm{ELBO}(q,\mathbf{x};\theta)\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})\prod_{t=1}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{\prod_{t=1}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)\prod_{t=2}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{T}\mid\mathbf{x}_{T-1})\prod_{t=1}^{T-1}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)\prod_{t=1}^{T-1}p(\mathbf{x}_{t}\mid\mathbf{x}_{t+1};\theta)}{q(\mathbf{x}_{T}\mid\mathbf{x}_{T-1})\prod_{t=1}^{T-1}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{q(\mathbf{x}_{T}\mid\mathbf{x}_{T-1})}\right]+\sum_{t=1}^{T-1}\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t}\mid\mathbf{x}_{t+1};\theta)}{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})}{q(\mathbf{x}_{T}\mid\mathbf{x}_{T-1})}\right]+\sum_{t=1}^{T-1}\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t}\mid\mathbf{x}_{t+1};\theta)}{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q({\color{red}\mathbf{x}_{1}}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)+\mathbb{E}_{q({\color{red}\mathbf{x}_{T-1:T}}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})}{q(\mathbf{x}_{T}\mid\mathbf{x}_{T-1})}\right]+\sum_{t=1}^{T-1}\mathbb{E}_{q({\color{red}\mathbf{x}_{t-1:t+1}}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t}\mid\mathbf{x}_{t+1};\theta)}{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\underbrace{\mathbb{E}_{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}_{\text{Reconstruction Term}}\\
&-\underbrace{\mathbb{E}_{q(\mathbf{x}_{T-1}\mid\mathbf{x}_{0})}[\mathrm{KL}(q(\mathbf{x}_{T}\mid\mathbf{x}_{T-1})\parallel p(\mathbf{x}_{T}))]}_{\text{Prior Matching Term}}\\
&-\underbrace{\sum_{t=1}^{T-1}\mathbb{E}_{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})q(\mathbf{x}_{t+1}\mid\mathbf{x}_{t})}[\mathrm{KL}(q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})\parallel p(\mathbf{x}_{t}\mid\mathbf{x}_{t+1};\theta))]}_{\text{Consistency Term}}
\end{align*}
$$

1. Reconstruction Term：VAE 中也有这一项，用来衡量模型的重建能力。
2. Prior Matching Term：用来衡量 $q(\mathbf{x}_{T}\mid\mathbf{x}_{T-1})$ 和 $p(\mathbf{x}_{T})$，即标准高斯分布，之间的差异。这一项没有可学习的参数，不能优化。
3. Consistency Term：用来衡量 $q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})$ 和 $p(\mathbf{x}_{t}\mid\mathbf{x}_{t+1};\theta)$ 之间的差异。即让前向过程和反向过程在任意的 $t$ 时刻，或者说 $\mathbf{x}_t$ 的后验分布 $q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})$ 和先验分布 $p(\mathbf{x}_{t}\mid\mathbf{x}_{t+1};\theta)$，都是一致的。这一项是可以优化的。

这种分解方式的缺陷在于，Consistency Term 没有比较好的解析形式，只能靠 Monte Carlo 近似。但是采样的 PDF 是 $q(\mathbf{x}_{T-1}\mid\mathbf{x}_{0})q(\mathbf{x}_{T+1}\mid\mathbf{x}_{T})$，是两个分布的乘积。采样这个分布存在着较大的方差。考虑到 $T$ 的数值比较大，这个方法不利于平稳地优化。

以下介绍实际采用的分解方式，这种分解方式得到的结果和 VAE 的结果非常相似。在这之前，还需要介绍一个技巧。由于前向过程是一个 Markov 链，所以
$$
q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})=q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1},\mathbf{x}_{0})=\frac{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})}
$$
后一个等号是由 Bayes 公式得到的。等价的，可以得到：
$$
q(\mathbf{x}_{t-1:t}\mid\mathbf{x}_{0})=q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})=q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})q(\mathbf{x}_{t}\mid\mathbf{x}_{0})
$$
以下开始分解 ELBO：
$$
\begin{align*}
&\quad\ \mathrm{ELBO}(q,\mathbf{x};\theta)\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})\prod_{t=1}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{\prod_{t=1}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)\prod_{t=2}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})\prod_{t=2}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)\prod_{t=2}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})\prod_{t=2}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}\right]+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\prod_{t=2}^{T}\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}\right]+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\prod_{t=2}^{T}\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)\cancel{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0}})}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\cancel{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{\cancel{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}}\right]+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{\cancel{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}}{q(\mathbf{x}_{T}\mid\mathbf{x}_{0})}\right]+\sum_{t=2}^{T}\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{q(\mathbf{x}_{T}\mid\mathbf{x}_{0})}\right]+\sum_{t=2}^{T}\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})}{q(\mathbf{x}_{T}\mid\mathbf{x}_{0})}\right]+\sum_{t=2}^{T}\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q({\color{red}\mathbf{x}_{1}}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)+\mathbb{E}_{q({\color{red}\mathbf{x}_{T}}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})}{q(\mathbf{x}_{T}\mid\mathbf{x}_{0})}\right]+\sum_{t=2}^{T}\mathbb{E}_{q({\color{red}\mathbf{x}_{t-1:t}}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})}\right]\\
&=\underbrace{\color{red}\mathbb{E}_{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}_{\text{Reconstruction Term}}\\
&-\underbrace{\color{red}\mathrm{KL}(q(\mathbf{x}_{T}\mid\mathbf{x}_{0})\parallel p(\mathbf{x}_{T}))}_{\text{Prior Matching Term}}\\
&-\underbrace{\color{red}\sum_{t=2}^{T}\mathbb{E}_{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}[\mathrm{KL}(q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\parallel p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta))]}_{\text{Denoising Matching Term}}
\end{align*}
$$

1. Reconstruction Term：同上。
2. Prior Matching Term：和 VAE 的第二项一样。用来衡量 $q(\mathbf{x}_{T}\mid\mathbf{x}_{0})$ 和 $p(\mathbf{x}_{T})$ 之间的差异。这一项没有可学习的参数，不能优化。
3. Denoising Matching Term：用来衡量 $q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})$ 和 $p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)$ 之间的差异。指导网络学习去噪的过程。

以下是 Denoising Matching Term 的推导过程。首先，根据 Bayes 公式和 Markov 链的性质，有
$$
q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})=\frac{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1},\mathbf{x}_{0})q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})}{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}=\frac{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})}{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}
$$
在前向过程中，已知 $q(\mathbf{x}_{t}\mid\mathbf{x}_{0})=\mathcal{N}(\mathbf{x}_{t};\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0},(1-\bar{\alpha}_{t})I)$，$q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})=\mathcal{N}(\mathbf{x}_{t};\sqrt{\alpha_{t}}\mathbf{x}_{t-1},(1-\alpha_{t})I)$，所以
$$
\begin{align*}
&\quad\ q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\\
&=\frac{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})}{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}\\
&=\frac{\mathcal{N}(\mathbf{x}_{t};\sqrt{\alpha_{t}}\mathbf{x}_{t-1},(1-\alpha_{t})I)\mathcal{N}(\mathbf{x}_{t-1};\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0},(1-\bar{\alpha}_{t-1})I)}{\mathcal{N}(\mathbf{x}_{t};\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0},(1-\bar{\alpha}_{t})I)}\\
&=C_1\exp\left(-\frac{1}{2}\left(\frac{\left(\mathbf{x}_{t}-\sqrt{\alpha_{t}}\mathbf{x}_{t-1}\right)^{2}}{1-\alpha_{t}}+\frac{\left(\mathbf{x}_{t-1}-\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}\right)^{2}}{1-\bar{\alpha}_{t-1}}-\frac{\left(\mathbf{x}_{t}-\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0}\right)^{2}}{1-\bar{\alpha}_{t}}\right)\right)\\
&=C_1\exp\left(-\frac{1}{2}\left(\frac{\mathbf{x}_{t}^{2}-2\sqrt{\alpha_{t}}\mathbf{x}_{t}\mathbf{x}_{t-1}+\alpha_{t}\mathbf{x}_{t-1}^{2}}{1-\alpha_{t}}+\frac{\mathbf{x}_{t-1}^{2}-2\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{t-1}\mathbf{x}_{0}+\bar{\alpha}_{t-1}\mathbf{x}_{0}^{2}}{1-\bar{\alpha}_{t-1}}-\frac{\mathbf{x}_{t}^{2}-2\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{t}\mathbf{x}_{0}+\bar{\alpha}_{t}\mathbf{x}_{0}^{2}}{1-\bar{\alpha}_{t}}\right)\right)\\
&=C_2\exp\left(-\frac{1}{2}\left(\frac{-2\sqrt{\alpha_{t}}\mathbf{x}_{t}\mathbf{x}_{t-1}+\alpha_{t}\mathbf{x}_{t-1}^{2}}{1-\alpha_{t}}+\frac{\mathbf{x}_{t-1}^{2}-2\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{t-1}\mathbf{x}_{0}}{1-\bar{\alpha}_{t-1}}\right)\right)\\
&=C_2\exp\left(-\frac{1}{2}\left(\left(\frac{\alpha_{t}}{1-\alpha_{t}}+\frac{1}{1-\bar{\alpha}_{t-1}}\right)\mathbf{x}_{t-1}^{2}-2\left(\frac{\sqrt{\alpha_{t}}\mathbf{x}_{t}}{1-\alpha_{t}}+\frac{\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_{t-1}}\right)\mathbf{x}_{t-1}\right)\right)\\
&=C_2\exp\left(-\frac{1}{2}\left(\frac{1-\bar{\alpha}_t}{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\mathbf{x}_{t-1}^{2}-2\left(\frac{\sqrt{\alpha_{t}}\mathbf{x}_{t}}{1-\alpha_{t}}+\frac{\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_{t-1}}\right)\mathbf{x}_{t-1}\right)\right)\\
&=C_2\exp\left(-\frac{1}{2}\frac{1-\bar{\alpha}_t}{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\left(\mathbf{x}_{t-1}^2-2\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t}\mathbf{x}_{t-1}\right)\right)\\
&=C_1\exp\left(-\frac{1}{2}\frac{1-\bar{\alpha}_t}{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\left(\mathbf{x}_{t-1}-\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t}\mathbf{x}_{t-1}\right)^2\right)\\
&={\color{red}\mathcal{N}\left(\mathbf{x}_{t-1};\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t},\frac{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t}I\right)}
\end{align*}
$$
where
$$
\begin{align*}
C_1&=\left(\frac{(1-\bar{\alpha}_{t})}{2\pi(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\right)^{\frac{n}{2}}\\
C_2&=\left(\frac{(1-\bar{\alpha}_{t})}{2\pi(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\right)^{\frac{n}{2}}\exp\left(-\frac{\left(\sqrt{\alpha_{t}}(1-\bar{\alpha}_{t-1})\mathbf{x}_{t}+\sqrt{\bar{\alpha}_{t-1}}(1-\alpha_{t})\mathbf{x}_{0}\right)^2}{2(1-\alpha_{t})(1-\bar{\alpha}_{t-1})(1-\bar{\alpha}_t)}\right)
\end{align*}
$$

---

> Diffusion 的伪代码

```python
def training() -> None:

def sampling() -> None:

```
