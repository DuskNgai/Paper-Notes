# Variational Autoencoder

|                    Symbol                     |                    Description                    |
| :-------------------------------------------: | :-----------------------------------------------: |
|                 $\theta,\phi$                 |                 model parameters                  |
|                      $p$                      |        Generative model/true distribution         |
|                      $q$                      |     Inference model/variational distribution      |
| $\mathbf{x}^{(n)}$/$\mathbf{x}$/$\mathcal{X}$ |    data sample/data random variable/data space    |
| $\mathbf{z}^{(i)}$/$\mathbf{z}$/$\mathcal{Z}$ | latent vector/latent random variable/latent space |

---

## Recap of EM Algorithm

回顾一下 EM 算法中的核心公式：
$$
\log p(\mathbf{x};\theta) = \underbrace{\mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \log \frac{p(\mathbf{x}, \mathbf{z}; \theta)}{q(\mathbf{z})} \right]}_{\mathop{\mathop{\mathrm{ELBO}}}(q,\mathbf{x};\theta)} + \underbrace{\mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \log \frac{q(\mathbf{z})}{p(\mathbf{z} \mid \mathbf{x}; \theta)} \right]}_{\mathop{\mathrm{KL}}[q(\mathbf{z}) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]}
$$
在 EM 算法中，我们已知当 $q(\mathbf{z}) = p(\mathbf{z} \mid \mathbf{x}; \theta)$ 时，ELBO 达到最大值。但由于 $p(\mathbf{z} \mid \mathbf{x}; \theta)$ 可能过于复杂，无法直接计算。VAE 通过引入生成网络和推断网络，将近似 $p(\mathbf{z} \mid \mathbf{x}; \theta)$ 的过程转化为一个优化问题。

1. **推断网络**：编码器 $\mathrm{enc}(\mathbf{x}; \phi)$ 估计变分分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$。理论上 $q(\mathbf{z})$ 是任意的、独立于 $\mathbf{x}$ 的，但由于最优的变分分布是 $p(\mathbf{z}\mid\mathbf{x})$，因此需要建模为条件概率。
2. **生成网络**：解码器 $\mathrm{dec}(\mathbf{z}; \theta)$ 估计条件概率 $p(\mathbf{x} \mid \mathbf{z}; \theta)$。

因此，VAE 对数据的建模实际上是：
$$
\log p(\mathbf{x}; \theta) = \mathop{\mathrm{ELBO}}(q,\mathbf{x};\theta,\phi) + \mathop{\mathop{\mathrm{KL}}}[q(\mathbf{z} \mid \mathbf{x}; \phi) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]
$$

## Inference Network

推断网络的输入应当是数据 $\mathbf{x}$，输出是潜变量 $\mathbf{z}$ 的条件概率分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$，而不是一个确定的值。因此一个数据点就可以对应到潜空间上由 $q(\mathbf{z} \mid \mathbf{x}; \phi)$ 定义的一个区域，我们希望从潜空间这个区域中采样而来的 $\mathbf{z}$ 都有能力还原出原数据 $\mathbf{x}$。推断网络的目标是找到网络参数 $\phi^*$，最小化变分分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$ 与 $p(\mathbf{z} \mid \mathbf{x}; \theta)$ 之间的 KL 散度，即：
$$
\phi^* = \mathop{\arg\min}_{\phi} \mathop{\mathrm{KL}}[q(\mathbf{z} \mid \mathbf{x}; \phi) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]
$$
实际上，这等价于最大化 ELBO：
$$
\phi^* = \mathop{\arg\max}_{\phi} \mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta, \phi)
$$
这一过程类似于 EM 算法中的 E 步。

## Generative Network

生成网络的输入应当是潜变量 $\mathbf{z}$，输出是数据 $\mathbf{x}$ 的条件概率分布 $p(\mathbf{x} \mid \mathbf{z}; \theta)$。但是因为推断网络的输出是一个分布，需要对于这个分布进行采样来得到潜变量 $\mathbf{z}$。生成网络的目标是找到网络参数 $\theta^*$，让原数据 $\mathbf{x}$ 与生成数据 $\text{dec}(\mathbf{z}; \theta)$ 之间的差异最小：
$$
\theta^* = \mathop{\arg\min}_{\theta} \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z} \mid \mathbf{x}; \phi)} \left[\mathcal{L}(\mathbf{x}, \mathrm{dec}(\mathbf{z}; \theta))\right]
$$
直观地理解，就是让网络生成原数据的概率最大。实际上，这也等价于最大化 ELBO：
$$
\theta^* = \mathop{\arg\max}_{\theta} \mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta, \phi)
$$
这一过程类似于 EM 算法中的 M 步。

## Cost Function

将两个目标函数结合起来，模型的目标是找到参数 $\theta^*, \phi^*$ 使得 ELBO 最大：
$$
\begin{aligned}
\theta^*, \phi^* &= \mathop{\arg\max}_{\theta, \phi} \mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta, \phi) \\
&= \mathop{\arg\max}_{\theta, \phi} \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z} \mid \mathbf{x}; \phi)} \left[\log \frac{p(\mathbf{x} \mid \mathbf{z}; \theta) p(\mathbf{z}; \theta)}{q(\mathbf{z} \mid \mathbf{x}; \phi)} \right] \\
&= \mathop{\arg\max}_{\theta, \phi} \underbrace{\mathbb{E}_{\mathbf{z} \sim q(\mathbf{z} \mid \mathbf{x}; \phi)} [\log p(\mathbf{x} \mid \mathbf{z}; \theta)]}_{\text{Reconstruction Term}} - \underbrace{\mathop{\mathrm{KL}}[q(\mathbf{z} \mid \mathbf{x}; \phi) \parallel p(\mathbf{z}; \theta)]}_{\text{Prior Matching Term}}
\end{aligned}
$$

### Reconstruction Term

对于第一项，VAE 通过 Monte Carlo 采样近似。对于每个样本 $\mathbf{x}$，根据变分分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$ 采样 $M$ 个样本 $\mathbf{z}^{(m)}$，用 Monte Carlo 采样估计：
$$
\mathbb{E}_{\mathbf{z} \sim q(\mathbf{z} \mid \mathbf{x}; \phi)}[\log p(\mathbf{x} \mid \mathbf{z}; \theta)] \approx \frac{1}{M} \sum_{m=1}^M \log p(\mathbf{x} \mid \mathbf{z}^{(m)}; \theta)
$$
根据极大似然估计，我们的目标就等价于最大化上式。实际问题中，条件概率 $p(\mathbf{x} \mid \mathbf{z}; \theta)$ 通常是分类分布（分类问题）或 Gaussian 分布（回归问题）。以图像重建为例，假设 $p(\mathbf{x} \mid \mathbf{z}; \theta) = \mathcal{N}(\mathbf{x} \mid \mathrm{dec}(\mathbf{z}; \theta), \lambda \mathbf{I})$，其中 $\mathrm{dec}(\mathbf{z}; \theta)$ 是生成网络的输出，$\lambda$ 是方差。那么：
$$
\log p(\mathbf{x} \mid \mathbf{z}; \theta) = -\frac{1}{2|\lambda\mathbf{I}|} \|\mathbf{x} - \mathrm{dec}(\mathbf{z}; \theta)\|^2 - \frac{1}{2} \log |\lambda \mathbf{I}| + \text{Constant}
$$
很多时候，我们会固定方差 $\lambda$ 为某个常数，这时候我们只需要最小化 $\|\mathbf{x} - \mathrm{dec}(\mathbf{z}; \theta)\|^2$ 即可，这等价于常见的 MSE 损失函数。

### Prior Matching Term

对于第二项，为了简便计算和采样，VAE 假设变分分布 $q(\mathbf{z} \mid \mathbf{x}; \phi)$ 和先验分布 $p(\mathbf{z}; \theta)$ 都是 Gaussian 分布：$\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\sigma}^2 \mathbf{I})$ 和 $\mathcal{N}(\mathbf{0}, \mathbf{I})$，它们的 KL 散度为：
$$
\mathop{\mathrm{KL}}\left[\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\sigma}^2 \mathbf{I}) \parallel \mathcal{N}(\mathbf{0}, \mathbf{I})\right] = \frac{1}{2} \left[-\log |\boldsymbol{\sigma}^2 \mathbf{I}| - \mathrm{trace}(\mathbf{I}) + \mathrm{trace}(\boldsymbol{\sigma}^2 \mathbf{I}) + (\boldsymbol{\mu}^T \boldsymbol{\mu})\right]
$$

### Reparameterization Trick

在计算 Reconstruction Term 时，由于采样步骤对 $\phi$ 不可导，导致无法使用梯度下降算法进行优化 $\phi$。因此 VAE 结合了变分分布是 Gaussian 分布 $\mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\sigma}^2 \mathbf{I})$ 的特点，引入了**重参数化**技巧，将采样过程分解为两步，首先从标准正态分布 $\mathcal{N}(\mathbf{0}, \mathbf{I})$ 中采样 $\boldsymbol{\epsilon}$，然后通过变分分布的参数 $\boldsymbol{\mu}, \boldsymbol{\sigma}$ 重构采样的 $\mathbf{z}$：
$$
\mathbf{z} = \boldsymbol{\mu} + \boldsymbol{\sigma} \odot \boldsymbol{\epsilon}
$$
这样，我们就可以对 $\boldsymbol{\mu}, \boldsymbol{\sigma}$ 求导，从而可以使用梯度下降算法进行优化。

### Final Loss Function

综上，VAE 的损失函数为：
$$
\begin{aligned}
\max_{\theta, \phi} \mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta, \phi) &\approx \frac{1}{M} \sum_{m=1}^M \log p(\mathbf{x} \mid \mathbf{z}^{(m)}; \theta) \\
&- \frac{1}{2} \left[-\log |\boldsymbol{\sigma}^2 I| - \mathrm{trace}(I) + \mathrm{trace}(\boldsymbol{\sigma}^2 I) + (\boldsymbol{\mu}^T \boldsymbol{\mu})\right]
\end{aligned}
$$

## Discussion

## Autoencoder

讲到 VAE 之前，我们先要讲一下 Autoencoder（AE）。AE 是一种无监督学习的模型，它通过编码器（$\mathrm{enc}(\mathbf{x}; \phi)$）将输入数据映射到潜空间，再通过解码器（$\mathrm{dec}(\mathbf{z}; \theta)$）将潜空间的表示映射回原始数据。AE 的目标是最小化重建误差，即最小化输入数据 $\mathbf{x}$ 和重建数据 $\hat{\mathbf{x}}$ 之间的差异：
$$
\min_{\theta, \phi} \mathbb{E}_{\mathbf{x} \sim p(\mathbf{x})} \left[\mathcal{L}(\mathbf{x}, \mathrm{dec}(\mathrm{enc}(\mathbf{x}; \phi); \theta))\right]
$$

一般来说，潜空间的维度要比输入数据的维度小，这样 AE 可以学习到数据的压缩表示。AE 的生成能力很弱，因为 AE 的潜空间是无约束的，即潜空间中只有编码器映射到的点有意义，而潜空间中的其他点没有意义。因此，无法通过随意地采样潜空间来生成新的数据。

VAE 是对 AE 的改进，它对于潜空间做出了两点改进：
1. 相比于 AE 把数据点映射到潜空间上的一个点，VAE 把数据点映射到潜空间上的一个区域，使得区域内的任意点都有重建能力。
2. VAE 对整个潜空间做了正则化，使得数据点在潜空间中的分布与某个先验分布相似，便于对潜空间进行采样。

为什么要对 AE 的潜空间做正则化？一种理解是，正则化防止了生存模型的过拟合。想象一下过拟合的情景，模型拟合了训练集中的每个数据点，那就完全失去了泛化能力，也就是生成能力。在 VAE、Diffusion 中，正则化的方式是让潜空间的分布与某个先验分布相似。但正则化又不止于此，随机批量输入、有限的模型容量、噪声注入等都是正则化的方式。

---

> VAE 的伪代码。

```python
class VariationalAutoEncoder(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.encoder = VAEEncoder()
        self.decoder = VAEDecoder()
        self.kl_weight = KL_weight

    @staticmethod
    def reparameterize(mu: torch.Tensor, logvar: torch.Tensor) -> torch.Tensor:
        std = torch.exp(0.5 * logvar)
        eps = torch.randn_like(std)
        z = mu + eps * std
        return z

    @staticmethod
    def recon_loss_fn(x: torch.Tensor, y: torch.Tensor) -> torch.Tensor:
        return torch.nn.functional.mse_loss(x, y, reduction="sum") / x.shape[0]

    @staticmethod
    def kl_divergence(mu: torch.Tensor, logvar: torch.Tensor) -> torch.Tensor:
        return -0.5 * (1 + logvar - mu.square() - logvar.exp()).sum(dim=-1).mean()

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        mu, logvar = self.encoder(x)        # [B, D]
        z = self.reparameterize(mu, logvar) # [B, D]
        y = self.decoder(z)

        loss = self.recon_loss_fn(x, y) + self.kl_weight * self.kl_divergence(mu, logvar)
        return y, loss
```