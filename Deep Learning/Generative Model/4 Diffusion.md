# Diffusion

|      Symbol      |                     Description                     |
| :--------------: | :-------------------------------------------------: |
|     $t$/$T$      |                  step/total steps                   |
|       $q$        | probabilistic function in forward/diffusion process |
|       $p$        |     probabilistic function in reversed process      |
| $\mathbf{x}_{0}$ |                      raw data                       |

其他记号同 [Variational Autoencoder](3%20Variational%20Autoencoder.md)

---

Diffusion 模型可以看作 [Markovian Hierarchical Variational Autoencoder](3.5%20Hierachical%20Variational%20Autoencoder.md) 的特例：

1. 每个潜空间的维度都是相同的，且与数据的维度相同。
2. 每个潜空间之间的编码器是非学习的，而是一个 Gaussian 分布。
3. 每个潜空间之间的 Gaussian 分布的方差是不同的，这是为了保证最后的潜空间是一个标准 Gaussian 分布。

## Forward Process / Diffusion Process

Diffusion 模型的前向过程是数据逐步加噪的过程。针对于 Diffusion 模型的第一个特点，根据 MHVAE 的性质，前向过程的联合分布为：
$$
q(\mathbf{x}_{1:T}\mid\mathbf{x_{0}})=\prod_{t=1}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})
$$
其中 $\mathbf{x}_{0}\sim q(\mathbf{x}_{0})$ 是给定的无噪声的数据。

针对于 Diffusion 模型的第二个特点，$\mathbf{x}_{0}$ 通过总共 $T$ 步的加 Gaussian 噪声过程，得到一系列的加噪数据 $\left\{\mathbf{x}_t\right\}_{t=1}^T$。每一步的加噪过程的方差为 $\left\{\beta_t\right\}_{t=1}^T$，因此每一步的加噪过程的分布为：
$$
\begin{align*}
q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})&=\mathcal{N}(\mathbf{x}_{t};\sqrt{1-\beta_{t}}\mathbf{x}_{t-1},\beta_{t}I)\\
&=\mathcal{N}(\mathbf{x}_{t};\sqrt{\alpha_{t}}\mathbf{x}_{t-1},(1-\alpha_{t})I)
\end{align*}
$$
这里的 $\beta_t$ 是一个超参数，$\alpha_{t}:=1-\beta_{t}$。

前向过程的重要性质是，$\mathbf{x}_t$ 的分布可以由 $\mathbf{x}_{0}$ 直接得到，即
$$
q(\mathbf{x}_{t}\mid\mathbf{x}_{0})=\mathcal{N}\left(\mathbf{x}_{t};\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0},(1-\bar{\alpha}_{t})I\right)
$$
其中 $\bar{\alpha}_{t}=\prod_{s=1}^{t}\alpha_{s}$。证明在 [Add Noise in Single Step](appendix/Prove%20Diffusion.md#Add%20Noise%20in%20Single%20Step)。

## Reversed Process

Diffusion 模型的反向过程是数据逐步去噪的过程。针对 Diffusion 模型的第三个特点，我们可以认为解码从 $\mathbf{x}_{T}\sim\mathcal{N}(\mathbf{0},\mathbf{I})$ 开始，反向过程的联合分布为：
$$
p(\mathbf{x}_{0:T};\theta)=p(\mathbf{x}_{T})\prod_{t=1}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)
$$
其中 $p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)$ 是可学习的。

这里，我们可以写出 MHVAE 中的 ELBO 在这套记号下的形式：
$$
\mathrm{ELBO}(q,\mathbf{x};\theta)=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\left[\log\frac{p(\mathbf{x}_{T})\prod_{t=1}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{\prod_{t=1}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]
$$
类似于 VAE，我们可以将 ELBO 分解：
$$
\text{ELBO}(q,\mathbf{x};\theta)=\underbrace{\mathbb{E}_{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}_{\text{Reconstruction Term}}
-\underbrace{\mathrm{KL}(q(\mathbf{x}_{T}\mid\mathbf{x}_{0})\parallel p(\mathbf{x}_{T}))}_{\text{Prior Matching Term}}
-\sum_{t=2}^{T}\underbrace{\mathbb{E}_{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}[\mathrm{KL}(q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\parallel p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta))]}_{\text{Denoising Matching Term}}
$$
具体过程见 [Decomposition of ELBO](appendix/Prove%20Diffusion.md#Decomposition%20of%20ELBO)

1. Reconstruction Term：VAE 中也有这一项，用来衡量模型的重建能力。
2. Prior Matching Term：VAE 中也有这一项，用来衡量 $q(\mathbf{x}_{T}\mid\mathbf{x}_{0})$ 和 $p(\mathbf{x}_{T})$ 之间的差异。这一项没有可学习的参数，不能优化。
3. Denoising Matching Term：用来衡量 $q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})$ 和 $p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)$ 之间的差异。指导网络学习去噪的过程。

### Denoising Matching Term

以下是 Denoising Matching Term 的推导过程。首先，根据 Bayes 公式和 Markov 链的性质，有
$$
q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})=\frac{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1},\mathbf{x}_{0})q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})}{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}=\frac{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})}{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}
$$
在前向过程中，已知 $q(\mathbf{x}_{t}\mid\mathbf{x}_{0})=\mathcal{N}(\mathbf{x}_{t};\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0},(1-\bar{\alpha}_{t})\mathbf{I})$，$q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})=\mathcal{N}(\mathbf{x}_{t};\sqrt{\alpha_{t}}\mathbf{x}_{t-1},(1-\alpha_{t})\mathbf{I})$，所以：
$$
q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\sim\mathcal{N}\left(\mathbf{x}_{t-1};\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t},\frac{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t}\mathbf{I}\right)
$$
推导见 [Denoising Matching Term](appendix/Prove%20Diffusion.md#Denoising%20Matching%20Term)。

设
$$
\boldsymbol{\mu}(t)=\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t}\quad\sigma^2(t)=\frac{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t}
$$
可以发现，$\boldsymbol{\mu}(t)$ 是一个与 $\mathbf{x}_{t}$ 有关的函数，是未知的，但是其他变量都是已知的。$\sigma^2(t)$ 也是完全已知的。因为 $q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})$ 是 Gaussian 分布，因此将 $p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)$ 建模为已知方差的 Gaussian 分布：
$$
p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)=\mathcal{N}(\mathbf{x}_{t-1};\boldsymbol{\mu}(\mathbf{x}_t,t;\theta),\sigma^2(t)I)
$$
其中 $\boldsymbol{\mu}(\mathbf{x}_t,t;\theta)$ 是带优化的均值。再回到 Denoising Matching Term 的 KL 散度上。我们仍然需要找到一组参数 $\theta$，使得其越小越好。因此结合 Gaussian 分布的 KL 散度是解析的特点，可以得到：
$$
\begin{align*}
&\quad\ \ \mathrm{KL}(q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\parallel p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta))\\
&=\mathrm{KL}(\mathcal{N}(\mathbf{x}_{t-1};\boldsymbol{\mu}(t),\sigma^2(t)I)\parallel\mathcal{N}(\mathbf{x}_{t-1};\boldsymbol{\mu}(\mathbf{x}_t,t;\theta),\sigma^2(t)I))\\
&=\frac{1}{2}\left[\log\frac{|\sigma^2(t)I|}{|\sigma^2(t)I|}+\mathrm{trace}((\sigma^2(t)I)^{-1}\sigma^2(t)I)+(\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta))^T(\sigma^2(t)I)^{-1}(\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta))-n\right]\\
&=\frac{1}{2}\left[\log1+n+(\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta))^T(\sigma^2(t)I)^{-1}(\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta))-n\right]\\
&=\frac{1}{2}\left[(\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta))^T(\sigma^2(t)I)^{-1}(\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta))\right]\\
&=\frac{\|\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta)\|^2}{2\sigma^2(t)}
\end{align*}
$$

为了让 $\boldsymbol{\mu}(t)$ 和 $\boldsymbol{\mu}(\mathbf{x}_t,t;\theta)$ 更加接近，就把 $\boldsymbol{\mu}(\mathbf{x}_t,t;\theta)$ 的形式建模的和 $\boldsymbol{\mu}(t)$ 更加接近，即
$$
\boldsymbol{\mu}(\mathbf{x}_t,t;\theta)=\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}(\mathbf{x}_{t},t;\theta)}{1-\bar{\alpha}_t}
$$
带入二者的表达式
$$
\begin{align*}
\mathrm{KL}(q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\parallel p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta))&=\frac{\|\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta)\|^2}{2\sigma^2(t)}\\
&={\color{red}\frac{1}{2\sigma^2(t)}\frac{(1-\alpha_{t})^2\bar{\alpha}_{t-1}}{(1-\bar{\alpha}_t)^2}\left\|\mathbf{x}_{0}-\mathbf{x}(\mathbf{x}_t,t;\theta)\right\|^2}
\end{align*}
$$
所以，只要将网络设计成为**在任意噪声条件下，重建出无噪声数据**即可减少 KL 散度！

带入 $\sigma^2(t)$ 的表达式
$$
\begin{align*}
\mathrm{KL}(q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\parallel p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta))&=\frac{1}{2\sigma^2(t)}\frac{(1-\alpha_{t})^2\bar{\alpha}_{t-1}}{(1-\bar{\alpha}_t)^2}\left\|\mathbf{x}_{0}-\mathbf{x}(\mathbf{x}_t,t;\theta)\right\|^2\\
&=\frac{1}{2}\frac{1-\bar{\alpha}_t}{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\frac{(1-\alpha_{t})^2\bar{\alpha}_{t-1}}{(1-\bar{\alpha}_t)^2}\left\|\mathbf{x}_{0}-\mathbf{x}(\mathbf{x}_t,t;\theta)\right\|^2\\
&=\frac{1}{2}\frac{(1-\alpha_{t})\bar{\alpha}_{t-1}}{(1-\bar{\alpha}_{t-1})(1-\bar{\alpha}_t)}\left\|\mathbf{x}_{0}-\mathbf{x}(\mathbf{x}_t,t;\theta)\right\|^2\\
&=\frac{1}{2}\frac{\bar{\alpha}_{t-1}-\bar{\alpha}_{t}}{(1-\bar{\alpha}_{t-1})(1-\bar{\alpha}_t)}\left\|\mathbf{x}_{0}-\mathbf{x}(\mathbf{x}_t,t;\theta)\right\|^2\\
&=\frac{1}{2}\left(\frac{\bar{\alpha}_{t-1}}{1-\bar{\alpha}_{t-1}}-\frac{\bar{\alpha}_{t}}{1-\bar{\alpha}_t}\right)\left\|\mathbf{x}_{0}-\mathbf{x}(\mathbf{x}_t,t;\theta)\right\|^2
\end{align*}
$$
这个表达式可以做训练，原作尝试发现效果不如后一种。最后，我们把这个表达式凑到论文的形式。已知
$$
q(\mathbf{x}_{t}\mid\mathbf{x}_{0})=\mathcal{N}(\mathbf{x}_{t};\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0},(1-\bar{\alpha}_{t})I)
$$
即
$$
\begin{align*}
\mathbf{x}_{t}&=\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0}+\sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon}_{0}
\end{align*}
$$
带入 $\boldsymbol{\mu}(t)$ 的表达式得到
$$
\begin{align*}
\boldsymbol{\mu}(t)&=\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t}\\
&=\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\frac{\mathbf{x}_{t}-\sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon}_{0}}{\sqrt{\bar{\alpha}_{t}}}}{1-\bar{\alpha}_t}\\
&=\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\frac{\mathbf{x}_{t}-\sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon}_{0}}{\sqrt{\alpha}_{t}}}{1-\bar{\alpha}_t}\\
&=\frac{(1-\bar{\alpha}_{t-1})\alpha_{t}\mathbf{x}_{t}+(1-\alpha_{t})(\mathbf{x}_{t}-\sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon}_{0})}{(1-\bar{\alpha}_t)\sqrt{\alpha}_{t}}\\
&=\frac{(1-\bar{\alpha}_{t-1})\alpha_{t}\mathbf{x}_{t}+(1-\alpha_{t})\mathbf{x}_{t}-(1-\alpha_{t})\sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon}_{0}}{(1-\bar{\alpha}_t)\sqrt{\alpha}_{t}}\\
&=\frac{(1-\bar{\alpha}_{t})\mathbf{x}_{t}-(1-\alpha_{t})\sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon}_{0}}{(1-\bar{\alpha}_t)\sqrt{\alpha}_{t}}\\
&=\frac{1}{\sqrt{\alpha}_{t}}\mathbf{x}_{t}-\frac{(1-\alpha_{t})}{\sqrt{1-\bar{\alpha}_{t}}\sqrt{\alpha}_{t}}\boldsymbol{\epsilon}_{0}
\end{align*}
$$

类似的
$$
\boldsymbol{\mu}(\mathbf{x}_t,t;\theta)=\frac{1}{\sqrt{\alpha}_{t}}\mathbf{x}_{t}-\frac{(1-\alpha_{t})}{\sqrt{1-\bar{\alpha}_{t}}\sqrt{\alpha}_{t}}\boldsymbol{\epsilon}(\mathbf{x}_t,t;\theta)
$$
此时 KL 散度为
$$
\begin{align*}
\mathrm{KL}(q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\parallel p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta))&=\frac{\|\boldsymbol{\mu}(t)-\boldsymbol{\mu}(\mathbf{x}_t,t;\theta)\|^2}{2\sigma^2(t)}\\
&={\color{red}\frac{1}{2\sigma^2(t)}\frac{(1-\alpha_{t})^2}{(1-\bar{\alpha}_{t})\alpha_{t}}\left\|\boldsymbol{\epsilon}_{0}-\boldsymbol{\epsilon}(\mathbf{x}_{t},t;\theta)\right\|^2}
\end{align*}
$$
和论文形式一致。因此模型**只要输入 $t$ 时刻和对应的加噪图像，输出一个噪声去接近 $t=1$ 时刻加入的噪声**即可。

### Reconstruction Term

原作在图像数据上估计 Reconstruction Term。它们将图像数据从 $\{0,\dots,255\}$ 转化到 $[-1,1]$，然后把联合分布分解到各个边缘分布上，计算离散的边缘分布：
$$
\begin{align*}
p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)&=\mathcal{N}(\mathbf{x}_{0};\boldsymbol{\mu}(\mathbf{x}_{1},t;\theta),\sigma^2(1)I)\\
&=\prod_{i=1}^{n}\int_{\delta_{-}(x_{0}^{i})}^{\delta_{+}(x_{0}^{i})}\mathcal{N}(x;\mu^{i}(\mathbf{x}_{1},t;\theta),\sigma^2(1))\mathrm{d}x
\end{align*}
$$
其中 $n$ 是 $\mathbf{x}_{0}$ 的维度，
$$
\delta_{-}(x)=\begin{cases}-\infty&\text{if }x=-1\\x-\frac{1}{255}&\text{if }x>-1\end{cases}\qquad
\delta_{+}(x)=\begin{cases}-\infty&\text{if }x=1\\x+\frac{1}{255}&\text{if }x<1\end{cases}
$$
这一项也可以不加入最后的 loss 计算，影响不大。

---

> Diffusion 的伪代码

```python
def training(x0) -> None:
    t = torch.randint(T)
    noise = torch.randn_like(x0)
    alpha_bar_t = alpha_bar[t]
    xt = sqrt(alpha_bar_t) * x0 + sqrt(1 - alpha_bar_t) * noise
    loss = torch.MSELoss()(noise - model(xt, t))
    return loss

def sampling(N) -> None:
    xt = torch.randn_like(N)
    for t in range(T, 0, -1):
        if t > 1:
            noise = torch.randn_like(xt)
        else:
            noise = 0
        xt = 1 / sqrt(alpha[t]) * (xt - (1 - alpha[t]) / sqrt(1 - alpha_bar[t]) * model(xt, t)) + sigma[t] * noise
        return xt
```
