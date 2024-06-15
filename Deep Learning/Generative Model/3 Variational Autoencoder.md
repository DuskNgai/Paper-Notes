# Variational Autoencoder

|                 Symbol                  |                    Description                    |
| :-------------------------------------: | :-----------------------------------------------: |
|              $\theta,\phi$              |                 model parameters                  |
| $\mathbf{x}$/$\mathbf{X}$/$\mathcal{X}$ |    data sample/data random variable/data space    |
| $\mathbf{z}$/$\mathbf{Z}$/$\mathcal{Z}$ | latent vector/latent random variable/latent space |

---

回顾一下 EM 算法中的核心公式：
$$
\log p(\mathbf{x};\theta) = \mathrm{ELBO}(q,\mathbf{x};\theta) + \mathrm{KL}[q(\mathbf{z}) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]
$$
为了获取最优的变分分布 $q(\mathbf{z})$，我们需要精确推断 $p(\mathbf{z} \mid \mathbf{x})$。由于其复杂性，需要近似推断。

VAE 是一种使用神经网络近似分布的模型，分别建模了两个复杂的条件概率函数：

1. 用**推断网络（编码器）** $\mathrm{enc}(\mathbf{x}; \phi)$ 估计变分分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$。理论上 $q(\mathbf{z})$ 是独立于 $\mathbf{x}$ 的，但由于最优的变分分布是 $p(\mathbf{z}\mid\mathbf{x})$，因此需要建模为条件概率。
2. 用**生成网络（解码器）** $\mathrm{dec}(\mathbf{z}; \theta)$ 估计条件概率 $p(\mathbf{x} \mid \mathbf{z}; \theta)$。

因此，VAE 对数据的建模实际上是：
$$
\log p(\mathbf{x}; \theta) = \mathrm{ELBO}(q,\mathbf{x};\theta,\phi) + \mathrm{KL}[q(\mathbf{z} \mid \mathbf{x}; \phi) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]
$$

## Inference Network

推断网络的目标是找到网络参数 $\phi^*$，最小化变分分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$ 与 $p(\mathbf{z} \mid \mathbf{x}; \theta)$ 之间的 KL 散度，即
$$
\phi^* = \arg\min_{\phi} \mathrm{KL}[q(\mathbf{z} \mid \mathbf{x}; \phi) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]
$$
实际上，这等价于最大化 ELBO：
$$
\phi^* = \arg\max_{\phi} \mathrm{ELBO}(q, \mathbf{x}; \theta, \phi)
$$
这一过程类似于 EM 算法中的 E 步。

## Generative Network

生成网络的目标是找到网络参数 $\theta^*$，最大化 ELBO：
$$
\theta^* = \arg\max_{\theta} \mathrm{ELBO}(q, \mathbf{x}; \theta, \phi)
$$
这一过程类似于 EM 算法中的 M 步。

## Cost Function

将两个目标函数结合起来，模型的目标是找到参数 $\theta^*, \phi^*$ 使得 ELBO 最大：
$$
\begin{align*}
\theta^*, \phi^* &= \arg\max_{\theta, \phi} \mathrm{ELBO}(q, \mathbf{x}; \theta, \phi) \\
&= \arg\max_{\theta, \phi} \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z} \mid \mathbf{x}; \phi)} \left[\log \frac{p(\mathbf{x} \mid \mathbf{z}; \theta) p(\mathbf{z}; \theta)}{q(\mathbf{z} \mid \mathbf{x}; \phi)} \right] \\
&= \arg\max_{\theta, \phi} \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z} \mid \mathbf{x}; \phi)} [\log p(\mathbf{x} \mid \mathbf{z}; \theta)] - \mathrm{KL}[q(\mathbf{z} \mid \mathbf{x}; \phi) \parallel p(\mathbf{z}; \theta)]
\end{align*}
$$

### Reconstruction Term

对于第一项，VAE 通过采样近似。对于每个样本 $\mathbf{x}$，根据变分分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$ 采样 $M$ 个样本 $\mathbf{z}^{(m)}$，用 Monte Carlo 估计：
$$
\mathbb{E}_{\mathbf{z} \sim q(\mathbf{z} \mid \mathbf{x}; \phi)}[\log p(\mathbf{x} \mid \mathbf{z}; \theta)] \approx \frac{1}{M} \sum_{m=1}^M \log p(\mathbf{x} \mid \mathbf{z}^{(m)}; \theta)
$$
由于采样步骤对 $\phi$ 不可导，VAE 引入**重参数化**技巧，将随机变量 $\epsilon \sim p(\epsilon)$ 引入：
$$
\mathbf{z} = \boldsymbol{\mu} + \boldsymbol{\sigma} \odot \epsilon
$$
其中 $\boldsymbol{\mu}, \boldsymbol{\sigma}$ 是编码器的输出，$\epsilon \sim \mathcal{N}(0, I)$。

实际计算中，条件概率 $p(\mathbf{x} \mid \mathbf{z}; \theta)$ 通常是 Bernoulli 分布（二分类问题）或 Gaussian 分布（回归问题）。以图像重建为例，$p(\mathbf{x} \mid \mathbf{z}; \theta)$ 是 Gaussian 分布：
$$
p(\mathbf{x} \mid \mathbf{z}; \theta) = \mathcal{N}(\mathbf{x} \mid \mathrm{dec}(\mathbf{z}; \theta), \sigma^2 I)
$$
其中 $\mathrm{dec}(\mathbf{z}; \theta)$ 是生成网络的输出。

### KL Divergence Term

对于第二项，VAE 可以计算解析解。假设变分分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$ 和先验分布 $p(\mathbf{z}; \theta)$ 都是正态分布 $\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\sigma}^2 I)$ 和 $\mathcal{N}(0, I)$，它们的 KL 散度为：
$$
\mathrm{KL}\left(\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\sigma}^2 I) \parallel \mathcal{N}(0, I)\right) = \frac{1}{2} \left[-\log |\boldsymbol{\sigma}^2 I| - \mathrm{trace}(I) + \mathrm{trace}(\boldsymbol{\sigma}^2 I) + (\boldsymbol{\mu}^T \boldsymbol{\mu})\right]
$$

### Final Loss Function

最终的损失函数为：
$$
\begin{align*}
\max_{\theta, \phi} \mathrm{ELBO}(q, \mathbf{x}; \theta, \phi) &\approx \frac{1}{M} \sum_{m=1}^M \log p(\mathbf{x} \mid \mathbf{z}^{(m)}; \theta) \\
&\quad - \frac{1}{2} \left[-\log |\boldsymbol{\sigma}^2 I| - \mathrm{trace}(I) + \mathrm{trace}(\boldsymbol{\sigma}^2 I) + (\boldsymbol{\mu}^T \boldsymbol{\mu})\right]
\end{align*}
$$

## Start From Joint Distribution

我们从联合分布来讲述 VAE 的损失函数的由来。VAE 用 KL 散度拉近数据隐空间近似的联合分布 $q(\mathbf{x}, \mathbf{z}; \phi)$ 和数据隐空间的联合分布 $p(\mathbf{x}, \mathbf{z}; \theta)$：
$$
\begin{align*}
&\quad \mathrm{KL}[q(\mathbf{x}, \mathbf{z}; \phi) \parallel p(\mathbf{x}, \mathbf{z}; \theta)]\\
&= \mathbb{E}_{\mathbf{x}, \mathbf{z} \sim q(\mathbf{x}, \mathbf{z}; \phi)} \left[\log \frac{q(\mathbf{x}, \mathbf{z}; \phi)}{p(\mathbf{x}, \mathbf{z}; \theta)}\right] \\
&= \iint q(\mathbf{x}, \mathbf{z}; \phi) \log \frac{q(\mathbf{x}, \mathbf{z}; \phi)}{p(\mathbf{x}, \mathbf{z}; \theta)} \mathrm{d}\mathbf{x} \mathrm{d}\mathbf{z} \\
&= \iint q(\mathbf{z} \mid \mathbf{x}; \phi) p(\mathbf{x}) \log \frac{q(\mathbf{z} \mid \mathbf{x}; \phi) p(\mathbf{x})}{p(\mathbf{x} \mid \mathbf{z}; \theta) p(\mathbf{z})} \mathrm{d}\mathbf{x} \mathrm{d}\mathbf{z} \\
&= \iint q(\mathbf{z} \mid \mathbf{x}; \phi) p(\mathbf{x}) \log p(\mathbf{x}) \mathrm{d}\mathbf{x} \mathrm{d}\mathbf{z} + \iint q(\mathbf{z} \mid \mathbf{x}; \phi) p(\mathbf{x}) \log \frac{q(\mathbf{z} \mid \mathbf{x}; \phi)}{p(\mathbf{x} \mid \mathbf{z}; \theta) p(\mathbf{z})} \mathrm{d}\mathbf{x} \mathrm{d}\mathbf{z}
\end{align*}
$$
这里，$q(\mathbf{x}, \mathbf{z}; \phi) = q(\mathbf{z} \mid \mathbf{x}; \phi) p(\mathbf{x})$ 表示我们用网络拟合的后验分布。看第一项，有：
$$
\begin{align*}
\iint q(\mathbf{z} \mid \mathbf{x}; \phi) p(\mathbf{x}) \log p(\mathbf{x}) \mathrm{d}\mathbf{x} \mathrm{d}\mathbf{z} &= \int \log p(\mathbf{x}) \int q(\mathbf{x}, \mathbf{z}; \phi) \mathrm{d}\mathbf{z} \mathrm{d}\mathbf{x} \\
&= \int \log p(\mathbf{x}) q(\mathbf{x}; \phi) \mathrm{d}\mathbf{x} \\
&= \text{Constant}
\end{align*}
$$
因此考虑第二项：
$$
\begin{align*}
&\quad \iint q(\mathbf{z} \mid \mathbf{x}; \phi) p(\mathbf{x}) \log \frac{q(\mathbf{z} \mid \mathbf{x}; \phi)}{p(\mathbf{x} \mid \mathbf{z}; \theta) p(\mathbf{z})} \mathrm{d}\mathbf{x} \mathrm{d}\mathbf{z}\\
&= \mathbb{E}_{\mathbf{x} \sim p(\mathbf{x})} \left[\int q(\mathbf{z} \mid \mathbf{x}; \phi) \log \frac{q(\mathbf{z} \mid \mathbf{x}; \phi)}{p(\mathbf{x} \mid \mathbf{z}; \theta) p(\mathbf{z})} \mathrm{d}\mathbf{z}\right] \\
&= \mathbb{E}_{\mathbf{x} \sim p(\mathbf{x})} \left[-\int q(\mathbf{z} \mid \mathbf{x}; \phi) \log p(\mathbf{x} \mid \mathbf{z}; \theta) \mathrm{d}\mathbf{z} + \int q(\mathbf{z} \mid \mathbf{x}; \phi) \log \frac{q(\mathbf{z} \mid \mathbf{x}; \phi)}{p(\mathbf{z})} \mathrm{d}\mathbf{z}\right] \\
&= \mathbb{E}_{\mathbf{x} \sim p(\mathbf{x})} \left[-\mathbb{E}_{\mathbf{z} \sim q(\mathbf{z} \mid \mathbf{x}; \phi)} [\log p(\mathbf{x} \mid \mathbf{z}; \theta)] + \mathrm{KL}[q(\mathbf{z} \mid \mathbf{x}; \phi) \parallel p(\mathbf{z})]\right]
\end{align*}
$$

## Why VAE Has Generative Power

VAE 解决了自编码器（AE）非正则化潜空间的问题。AE 将数据映射到潜空间中的点，而 VAE 将数据映射到潜空间中的区域，确保潜空间的正则性。VAE 的编码器输出数据在潜空间的分布参数，使得在潜空间区域内的采样点都能重建输入数据，因此具有生成能力。

---

> VAE 的伪代码。

```python
class VariationalAutoEncoder(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.encoder = VAEEncoder()
        self.decoder = VAEDecoder()
        self.reconstruction_criteria = ReconstructionLoss()
        self.kl_weight = KL_weight

    @staticmethod
    def kl_divergence(mu: torch.Tensor, std: torch.Tensor) -> torch.Tensor:
        return -0.5 * (1 + std.log() - mu.square() - std.square()).sum(dim=-1).mean()

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        mu, logvar = self.encoder(x)
        std = torch.exp(0.5 * logvar)
        eps = torch.randn_like(std)
        z = mu + eps * std

        y = self.decoder(z)
        loss = self.reconstruction_criteria(x, y) + self.kl_weight * self.kl_divergence(mu, std)
        return y, loss
```
