# Variational Auto-Encoder

|                 Symbol                  |                    Description                    |
| :-------------------------------------: | :-----------------------------------------------: |
|                 $t$/$T$                 |                 step/total steps                  |
|              $\theta,\phi$              |                 model parameters                  |
| $\mathbf{x}$/$\mathbf{X}$/$\mathcal{X}$ |    data sample/data random variable/data space    |
| $\mathbf{z}$/$\mathbf{Z}$/$\mathcal{Z}$ | latent vector/latent random variable/latent space |

---

VAE 还是继承了 EM 算法的隐变量思想，将数据分布建模为
$$
p(\mathbf{x})=\int p(\mathbf{x}\mid\mathbf{z})p(\mathbf{z})\mathrm{d}\mathbf{z}
$$
回顾一下 EM 算法中的核心公式
$$
\log p(\mathbf{x})=\mathrm{ELBO}(q,\mathbf{x})+\mathrm{KL}\left(q(\mathbf{z})\parallel p(\mathbf{z}\mid\mathbf{x})\right)
$$
在 EM 算法里面，为了获取最优的变分分布 $q(\mathbf{z})$，我们需要积分得到后验分布 $p(\mathbf{z}\mid\mathbf{x})$。如果 $\mathbf{z}$ 的分布形式比较复杂，那么求精确解就会非常困难，因此需要近似估计。VAE 就是一种利用神经网络来估计分布的模型，其思想是利用神经网络来分别建模两个复杂的条件概率密度函数：

1. 用**推断网络（编码器）**$\mathrm{enc}$ 估计变分分布 $q(\mathbf{z};\phi)$。理论上 $q(\mathbf{z})$ 可以独立于 $\mathbf{x}$，但是由于最优的变分分布是后验分布 $p(\mathbf{z}\mid\mathbf{x})$，因此网络被建模成 $\mathrm{enc}(\mathbf{x};\phi)=q(\mathbf{z}\mid\mathbf{x};\phi)$ 得到隐变量 $\mathbf{z}$。
2. 用**生成网络（解码器）**$\mathrm{dec}$ 估计条件概率 $p(\mathbf{x}\mid\mathbf{z};\theta)$ 得到生成的数据 $\mathbf{x}$。效果上做的是分布之间的变换。

因此，VAE 可以看作是一种可微的 EM 算法。那么，VAE 为什么会具有生成能力呢？这得从自编码器（AE）开始说起。

我们通常认为，AE 中的编码器倾向于学习数据中存在的差异性信息，而解码器倾向于学习数据中存在的共性信息。如果 AE 能够将重建损失降得很低，那么说明 AE 的潜空间充分编码了数据。因此，AE 起到了数据降维的作用。但在 AE 中，数据被压缩到潜空间中的部分区域，在这部分区域外的采样无法生成有意义的数据。也因此，AE 的生成能力很弱。而 VAE 解决了 AE 中非正则化潜空间的问题。VAE 的编码器输出的是数据在潜空间的分布参数，并且强迫这个分布与某个先验分布接近，这样就确保了潜空间的正则性。

形象地表述就是，AE 将数据映射到了潜空间中的一个点，而 VAE 将数据映射到了潜空间中的一个区域，这个区域的范围由 VAE 的编码器近似的后验概率 $p(\mathbf{z}\mid\mathbf{x})$ 所表示。任何在这个区域内的采样点都应该有能力还原出输入的数据点。但是采样点之间总是会存在一点差异，因此每个重建结果也会有所不同。因此 VAE 就具有了一定的生成能力。

## Inference Network

推断网络 $\mathrm{enc}$ 的目标是找到一组网络参数 $\phi^*$，使得变分分布 $q(\mathbf{z}\mid\mathbf{x};\phi)$ 接近后验概率 $p(\mathbf{z}\mid\mathbf{x};\theta)$，即
$$
\phi^*=\mathop{\arg\min}_{\phi}\,\mathrm{KL}\left(q(\mathbf{z}\mid\mathbf{x};\phi)\parallel p(\mathbf{z}\mid\mathbf{x};\theta)\right)
$$
我们已经知道，对于绝大部分问题来说，精确推断后验概率 $p(\mathbf{z}\mid\mathbf{x};\theta)$ 非常困难。而变分推断告诉我们，问题可以转化为：
$$
\begin{align*}
\phi^*&=\mathop{\arg\max}_{\phi}\,\mathrm{ELBO}(q,\mathbf{x};\theta,\phi)
\end{align*}
$$
即找到 $\phi^*$ 使得 ELBO 最大，类似于 EM 算法的 E 步。

## Generate Network

生成网络 $\mathrm{dec}$ 的目标是找到一组网络参数 $\theta^*$，使得 ELBO 最大，即
$$
\theta^*=\mathop{\arg\max}_{\theta}\,\mathrm{ELBO}(q,\mathbf{x};\theta,\phi)
$$
类似于 EM 算法的 M 步。

## Cost Function

至此，模型的目标就是最大化 ELBO，最终的目标函数就是
$$
\begin{align*}
\max_{\theta,\phi}\,\mathrm{ELBO}(q,\mathbf{x};\theta,\phi)&=\max_{\theta,\phi}\,\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\log\left[\frac{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z};\theta)}{q(\mathbf{z}\mid\mathbf{x};\phi)}\right]\\
&=\max_{\theta,\phi}\,\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\left[\log p(\mathbf{x}\mid\mathbf{z};\theta)\right]-\mathrm{KL}\left(q(\mathbf{z}\mid\mathbf{x};\phi)\parallel p(\mathbf{z};\theta)\right)
\end{align*}
$$
和 GAN 不同的地方在于，VAE 是联合优化编码器和解码器的。如果 KL 散度降到 0 了，那么说明近似的后验分布与 $\mathbf{x}$ 无关了，这样重建损失必然不可能小；而重建损失如果降到 0 了，那么说明近似的后验分布与 $\mathbf{x}$ 非常相关，除非先验分布非常好，否则 KL 散度不可能小。因此损失函数中的两项不能割裂开来看。

### Reconstrution Term

对于第一项，VAE 通过采样的方式近似。对于每个样本 $\mathbf{x}$，根据变分分布 $q(\mathbf{z}\mid\mathbf{x};\phi)$ 采样 $M$ 个样本 $\mathbf{z}^{(m)}$，用 Monte Carlo 估计
$$
\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\left[\log p(\mathbf{x}\mid\mathbf{z};\theta)\right]\approx\frac{1}{M}\sum_{m=1}^{M}\log p(\mathbf{x}\mid\mathbf{z}^{(m)};\theta)
$$
上面的近似中，采样步骤对 $\phi$ 是不可导的，因此上述步骤无法优化 $\phi$。VAE 采用了**重参数化**的方法来规避这个问题。VAE 引入了一个分布为 $p(\epsilon)$ 的随机变量 $\epsilon$，上面的式子就可以转化为
$$
\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\left[\log p(\mathbf{x}\mid\mathbf{z};\theta)\right]=\mathbb{E}_{\epsilon\sim p(\epsilon)}\left[\log p(\mathbf{x}\mid g(\phi, \epsilon);\theta)\right]
$$
其中 $\mathbf{z}=g(\phi,\epsilon)$ 是一个固定的映射。VAE 假设变分分布 $q(\mathbf{z}\mid\mathbf{x};\phi)$ 是正态分布 $\mathcal{N}(\boldsymbol{\mu},\boldsymbol{\sigma}^2I)$，其中 $\boldsymbol{\mu},\boldsymbol{\sigma}$ 是 $\mathrm{enc}$ 的输出。根据正态分布的性质，取 $\epsilon\sim\mathcal{N}(0,I)$，就可以转化到 $\mathbf{z}$
$$
\mathbf{z}=\boldsymbol{\mu}+\boldsymbol{\sigma}\odot\epsilon
$$

实践中，条件概率 $p(\mathbf{x}\mid\mathbf{z};\theta)$ 一般用其他函数来近似，例如 Bernoulli 分布、Laplace 分布、Gaussian 分布等。图像重建任务中，一般用正态分布来近似，这样就可以用 L2 损失函数来衡量二者的差距：
$$
\begin{align*}
\log p(\mathbf{x}\mid\mathbf{z};\theta)&=-\frac{1}{2}\left\|\frac{\mathbf{x}-\mu(\mathbf{z})}{\sigma^2(\mathbf{z})}\right\|-\frac{n}{2}\log2\pi-\frac{1}{2}\log\sigma^2(\mathbf{z})
\end{align*}
$$
一般选取方差为固定的常数，就退化成了普通的 L2 损失函数。

### KL Divergence Term

对于第二项，VAE 可以计算解析解。VAE 假设变分分布 $q(\mathbf{z}\mid\mathbf{x};\phi)$ 是正态分布 $\mathcal{N}(\boldsymbol{\mu},\boldsymbol{\sigma}^2I)$，先验分布 $p(\mathbf{z};\theta)$ 也是正态分布 $\mathcal{N}(0,I)$，则它们的 KL 散度为
$$
\begin{align*}
&\mathrm{KL}\left(\mathcal{N}(\boldsymbol{\mu},\boldsymbol{\sigma}^2I)\parallel\mathcal{N}(0,I)\right)\\
&=\frac{1}{2}\left[-\log|\boldsymbol{\sigma}^2I|-\mathrm{trace}(I)+\mathrm{trace}\left(\boldsymbol{\sigma}^2I\right)+(\boldsymbol{\mu}^T\boldsymbol{\mu})\right]
\end{align*}
$$

最后的损失函数就是：
$$
\begin{align*}
\max_{\theta,\phi}\,\mathrm{ELBO}(q,\mathbf{x};\theta,\phi)&=\max_{\theta,\phi}\,\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z};\phi)}\left[\log p(\mathbf{x}\mid\mathbf{z};\theta)\right]-\mathrm{KL}\left(q(\mathbf{z}\mid\mathbf{x};\phi)\parallel p(\mathbf{z};\theta)\right)\\
&\approx\frac{1}{M}\sum_{m=1}^{M}\log p(\mathbf{x}\mid\mathbf{z}^{(m)};\theta)-\frac{1}{2}\left[-\log|\boldsymbol{\sigma}^2I|-\mathrm{trace}(I)+\mathrm{trace}\left(\boldsymbol{\sigma}^2I\right)+(\boldsymbol{\mu}^T\boldsymbol{\mu})\right]
\end{align*}
$$

## Start From Joint Distribution

我们从联合分布来讲述 VAE 的损失函数的由来。VAE 用 KL 散度拉近数据隐空间近似的联合分布 $q(\mathbf{x},\mathbf{z};\phi)$ 和数据隐空间的联合分布 $p(\mathbf{x},\mathbf{z};\theta)$ 
$$
\begin{align*}
&\quad\ \ \mathrm{KL}\left(q(\mathbf{x},\mathbf{z};\phi)\parallel p(\mathbf{x},\mathbf{z};\theta)\right)\\
&=\mathbb{E}_{\mathbf{x},\mathbf{z}\sim q(\mathbf{x},\mathbf{z};\phi)}\left[\log\frac{q(\mathbf{x},\mathbf{z};\phi)}{p(\mathbf{x},\mathbf{z};\theta)}\right]\\
&=\iint q(\mathbf{x},\mathbf{z};\phi)\log\frac{q(\mathbf{x},\mathbf{z};\phi)}{p(\mathbf{x},\mathbf{z};\theta)}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}\\
&=\iint q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z})}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}\\
&=\iint q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log p(\mathbf{x})\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}+\iint q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z})}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}
\end{align*}
$$
这里，$q(\mathbf{x},\mathbf{z};\phi)=q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})$ 表示我们用网络拟合的后验分布。看第一项，有
$$
\begin{align*}
\iint q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log p(\mathbf{x})\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}&=\int\log p(\mathbf{x})\int q(\mathbf{x},\mathbf{z};\phi)\mathrm{d}\mathbf{z}\mathrm{d}\mathbf{x}\\
&=\int\log p(\mathbf{x})q(\mathbf{x};\phi)\mathrm{d}\mathbf{x}\\
&=\mathrm{Constant}
\end{align*}
$$
因此考虑第二项
$$
\begin{align*}
&\quad \ \ \iint q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z})}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z})}\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[-\int q(\mathbf{z}\mid\mathbf{x};\phi)\log p(\mathbf{x}\mid\mathbf{z};\theta)\mathrm{d}\mathbf{z}+\int q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{z})}\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[-\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}[\log p(\mathbf{x}\mid\mathbf{z};\theta)]+\mathrm{KL}\left(q(\mathbf{z}\mid\mathbf{x};\phi)\parallel p(\mathbf{z})\right)\right]
\end{align*}
$$

### Auto Clustering

把 $\mathbf{z}$ 拆开成连续特征表征 $\mathbf{z}$ 和离散聚类标签 $y$，那么我们可以把联合分布的拉近表示为
$$
\begin{align*}
&\quad\ \ \mathrm{KL}\left(q(\mathbf{x},\mathbf{z},y;\phi)\parallel p(\mathbf{x},\mathbf{z},y;\theta)\right)\\
&=\mathbb{E}_{\mathbf{x},\mathbf{z},y\sim q(\mathbf{x},\mathbf{z},y;\phi)}\left[\log\frac{q(\mathbf{x},\mathbf{z},y;\phi)}{p(\mathbf{x},\mathbf{z},y;\theta)}\right]\\
&=\iint\sum_{y}q(\mathbf{x},\mathbf{z},y;\phi)\log\frac{q(\mathbf{x},\mathbf{z},y;\phi)}{p(\mathbf{x},\mathbf{z},y;\theta)}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}\\
&=\iint\sum_{y}q(\mathbf{z},y\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(\mathbf{z},y\mid\mathbf{x};\phi)p(\mathbf{x})}{p(\mathbf{x}\mid\mathbf{z},y;\theta)p(\mathbf{z},y)}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}
\end{align*}
$$
为了拆分到具体的损失函数计算形式，我们需要进一步去假设一些变量关系。这里我们按照 Hierarchical VAE 的选取方案，让 $\mathbf{z}$ 只和 $\mathbf{x}$ 有关，$y$ 只和 $\mathbf{z}$ 有关，即
$$
\begin{align*}
q(\mathbf{z},y\mid\mathbf{x};\phi)&=q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\\
p(\mathbf{x}\mid\mathbf{z},y;\theta)&=p(\mathbf{x}\mid\mathbf{z};\theta)\\
p(\mathbf{z},y;\theta)&=p(\mathbf{z}\mid y;\theta)p(y)
\end{align*}
$$
带入得到
$$
\begin{align*}
&\quad\ \ \mathrm{KL}\left(q(\mathbf{x},\mathbf{z},y;\phi)\parallel p(\mathbf{x},\mathbf{z},y;\theta)\right)\\
&=\iint\sum_{y}q(\mathbf{z},y\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(\mathbf{z},y\mid\mathbf{x};\phi)p(\mathbf{x})}{p(\mathbf{x}\mid\mathbf{z},y;\theta)p(\mathbf{z},y;\theta)}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}\\
&=\iint\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z}\mid y;\theta)p(y)}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}\\
&=\iint\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log p(\mathbf{x})\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}+\iint\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z}\mid y;\theta)p(y)}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}
\end{align*}
$$
还是分离出第二项。对于第二项，我们有两种损失函数的计算方式。第一种是从 Diffusion 来的
$$
\begin{align*}
&\quad\ \ \iint\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z}\mid y;\theta)p(y)}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z}\mid y;\theta)p(y)}\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(y\mid\mathbf{z},\mathbf{x};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z}\mid y;\theta)p(y)}\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(y)}\frac{q(y\mid\mathbf{z},\mathbf{x};\phi)}{p(\mathbf{z}\mid y;\theta)}\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(y)}\frac{q(\mathbf{z}\mid y,\mathbf{x};\phi)q(y\mid\mathbf{x};\phi)}{p(\mathbf{z}\mid y;\theta)q(\mathbf{z}\mid\mathbf{x};\phi)}\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(\mathbf{z}\mid y,\mathbf{x};\phi)}{p(\mathbf{z}\mid y;\theta)}\mathrm{d}\mathbf{z}+\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(y\mid\mathbf{x};\phi)}{p(y)}\mathrm{d}\mathbf{z}-\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log p(\mathbf{x}\mid\mathbf{z};\theta)\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(\mathbf{z}\mid y,\mathbf{x};\phi)q(y\mid\mathbf{x};\phi)\log\frac{q(\mathbf{z}\mid y,\mathbf{x};\phi)}{p(\mathbf{z}\mid y;\theta)}\mathrm{d}\mathbf{z}+\sum_{y}q(y\mid\mathbf{x};\phi)\log\frac{q(y\mid\mathbf{x};\phi)}{p(y)}-\int q(\mathbf{z}\mid\mathbf{x};\phi)\log p(\mathbf{x}\mid\mathbf{z};\theta)\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\mathbb{E}_{y\sim q(y\mid\mathbf{x};\phi)}\left[\mathrm{KL}\left(q(\mathbf{z}\mid y,\mathbf{x};\phi)\parallel p(\mathbf{z}\mid y;\theta)\right)\right]+\mathrm{KL}\left(q(y\mid\mathbf{x};\phi)\parallel p(y)\right)-\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\left[\log p(\mathbf{x}\mid\mathbf{z};\theta)\right]\right]\\
\end{align*}
$$
虽然 Diffusion 很 work，但是 Diffusion 的先验是知道的，在我们的情况下，做不到，所以第一种形式无法像 Diffusion 做进一步的解析计算。第二种是这样的
$$
\begin{align*}
&\quad\ \ \iint\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)p(\mathbf{x})\log\frac{q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z}\mid y;\theta)p(y)}\mathrm{d}\mathbf{x}\mathrm{d}\mathbf{z}\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{x}\mid\mathbf{z};\theta)p(\mathbf{z}\mid y;\theta)p(y)}\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{z}\mid y;\theta)}\mathrm{d}\mathbf{z}+\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(y\mid\mathbf{z};\phi)}{p(y)}\mathrm{d}\mathbf{z}-\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log p(\mathbf{x}\mid\mathbf{z};\theta)\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\int\sum_{y}q(y\mid\mathbf{z};\phi)q(\mathbf{z}\mid\mathbf{x};\phi)\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{z}\mid y;\theta)}\mathrm{d}\mathbf{z}+\int q(\mathbf{z}\mid\mathbf{x};\phi)\mathrm{KL}\left(q(y\mid\mathbf{z};\phi)\parallel p(y)\right)\mathrm{d}\mathbf{z}-\int q(\mathbf{z}\mid\mathbf{x};\phi)\log p(\mathbf{x}\mid\mathbf{z};\theta)\mathrm{d}\mathbf{z}\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\left[\mathbb{E}_{y\sim q(y\mid\mathbf{z};\phi)}\left[\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{z}\mid y;\theta)}\right]+\mathrm{KL}\left(q(y\mid\mathbf{z};\phi)\parallel p(y)\right)-\log p(\mathbf{x}\mid\mathbf{z};\theta)\right]\right]\\
&=\mathbb{E}_{\mathbf{x}\sim p(\mathbf{x})}\left[\mathbb{E}_{y\sim q(y\mid\mathbf{z};\phi)}\left[\mathrm{KL}\left(q(\mathbf{z}\mid\mathbf{x};\phi)\parallel p(\mathbf{z}\mid y;\theta)\right)\right]+\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\left[\mathrm{KL}\left(q(y\mid\mathbf{z};\phi)\parallel p(y)\right)-\log p(\mathbf{x}\mid\mathbf{z};\theta)\right]\right]
\end{align*}
$$
为了方便计算，我们需要规定分布的形式。我们规定，$q(\mathbf{z}\mid\mathbf{x};\phi)$ 是 $D$ 维多元标准 Gaussian 分布，$q(y\mid\mathbf{z};\phi)$ 是定义在 $N$ 个取值上的任意离散分布，$p(y)$ 是定义在 $N$ 个取值上的均匀分布，$p(\mathbf{z}\mid y;\theta)$ 是 $D$ 维多元标准 Gaussian 分布，$p(\mathbf{x}\mid\mathbf{z};\theta)$ 是和输入相同维度的多元标准 Gaussian 分布。因此解析计算相关项，注意第一个和第二个等式是等价的
$$
\begin{align*}
\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\left[\mathbb{E}_{y\sim q(y\mid\mathbf{z};\phi)}\left[\log\frac{q(\mathbf{z}\mid\mathbf{x};\phi)}{p(\mathbf{z}\mid y;\theta)}\right]\right]&=\frac{1}{2}\mathbb{E}_{\mathbf{z}\sim q(\mathbf{z}\mid\mathbf{x};\phi)}\left[\mathbb{E}_{y\sim q(y\mid\mathbf{z};\phi)}\left[\sum_{i}^{D}\log\boldsymbol{\sigma}^{2}_{i}(y)-\sum_{i}^{D}\log\boldsymbol{\sigma}^{2}_{i}(\mathbf{x})+\left\|\frac{\mathbf{z}-\boldsymbol{\mu}(y)}{\boldsymbol{\sigma}(y)}\right\|^{2}-\left\|\frac{\mathbf{z}-\boldsymbol{\mu}(\mathbf{x})}{\boldsymbol{\sigma}(\mathbf{x})}\right\|^{2}\right]\right]\\
\mathbb{E}_{y\sim q(y\mid\mathbf{z};\phi)}\left[\mathrm{KL}\left(q(\mathbf{z}\mid\mathbf{x};\phi)\parallel p(\mathbf{z}\mid y;\theta)\right)\right]&=\frac{1}{2}\mathbb{E}_{y\sim q(y\mid\mathbf{z};\phi)}\left[\sum_{i}^{D}\left(\log\boldsymbol{\sigma}^{2}_{i}(y)-\log\boldsymbol{\sigma}^{2}_{i}(\mathbf{x})+\frac{\boldsymbol{\sigma}^{2}_{i}(\mathbf{x})}{\boldsymbol{\sigma}^{2}_{i}(y)}+\frac{\left(\boldsymbol{\mu}_{i}(\mathbf{x})-\boldsymbol{\mu}_{i}(y)\right)^2}{\boldsymbol{\sigma}_{i}^2(y)}-1\right)\right]\\
\mathrm{KL}\left(q(y\mid\mathbf{z};\phi)\parallel p(y)\right)&=\sum_{k}^{N}a_{k}(\mathbf{z})\log a_{k}(\mathbf{z})+N\log N\\
\log p(\mathbf{x}\mid\mathbf{z};\theta)&=-\frac{1}{2}\left\|\frac{\mathbf{x}-\mu(\mathbf{z})}{\sigma^2(\mathbf{z})}\right\|-\frac{n}{2}\log2\pi-\frac{1}{2}\log\sigma^2(\mathbf{z})
\end{align*}
$$
网络规定为：$q(\mathbf{z}\mid\mathbf{x};\phi)$ 生成 $D$ 维多元标准 Gaussian 分布的均值和方差，重参数化得到采样 $\mathbf{z}=\boldsymbol{\epsilon}\otimes\boldsymbol{\sigma}(\mathbf{x})+\boldsymbol{\mu}(\mathbf{x})$。$q(y\mid\mathbf{z};\phi)$ 基于采样得到的 $\mathbf{z}$ 得到分类结果 $y$，和先验分布 $p(y)$ 做 KL 散度。采样一个 $y$，计算 $p(\mathbf{z}\mid y;\theta)$ 的均值和方差，计算对应的 KL 散度。最后用新采样的 $\mathbf{z}$ 计算 $p(\mathbf{x}\mid\mathbf{z};\theta)$ 重构。

## Hierarchical VAE

Hierarchical VAE，特别是 Markovian Hierarchical VAE，在潜空间上将多个 VAE 串联起来，每个 VAE 的潜空间都是之前一个的潜空间（除了第一个 VAE 是从数据空间）编码而来。

```mermaid
flowchart LR;
0((x))-- q(z_1|x) -->1((z_1))-- q(z_2|z_1) -->2((...))-- q(z_T|z_T-1) -->3((z_T));
3-- p(z_T-1|z_T) -->2-- p(z_1|z_2) -->1-- p(x|z_1) -->0;
```

因此，联合分布可以写成
$$
\begin{align*}
p(\mathbf{x}\mid\mathbf{z}_{1:T};\theta)&=p(\mathbf{z}_T;\theta)p(\mathbf{x}\mid\mathbf{z}_1;\theta)\prod_{t=2}^{T}p(\mathbf{z}_{t-1}\mid\mathbf{z}_{t};\theta)\\
q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)&=q(\mathbf{z}_1\mid\mathbf{x};\phi)\prod_{t=2}^{T}q(\mathbf{z}_t\mid\mathbf{z}_{t-1};\phi)
\end{align*}
$$
对应的 ELBO 为
$$
\begin{align*}
\log p(\mathbf{x};\theta)&=\log\int p(\mathbf{x},\mathbf{z}_{1:T};\theta)\mathrm{d}\mathbf{z}_{1:T}\\
&=\log\int\frac{p(\mathbf{x},\mathbf{z}_{1:T};\theta)q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}{q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}\mathrm{d}\mathbf{z}_{1:T}\\
&=\log\mathbb{E}_{q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}\left[\frac{p(\mathbf{x},\mathbf{z}_{1:T};\theta)}{q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}\right]\\
&\geq\mathbb{E}_{q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}\log\left[\frac{p(\mathbf{x},\mathbf{z}_{1:T};\theta)}{q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}\right]
\end{align*}
$$
其中
$$
\mathrm{ELBO}(q,\mathbf{x};\theta,\phi)=\mathbb{E}_{q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}\log\left[\frac{p(\mathbf{x},\mathbf{z}_{1:T};\theta)}{q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}\right]=\mathbb{E}_{q(\mathbf{z}_{1:T}\mid\mathbf{x};\phi)}\log\left[\frac{p(\mathbf{z}_T;\theta)p(\mathbf{x}\mid\mathbf{z}_1;\theta)\prod_{t=2}^{T}p(\mathbf{z}_{t-1}\mid\mathbf{z}_{t};\theta)}{q(\mathbf{z}_1\mid\mathbf{x};\phi)\prod_{t=2}^{T}q(\mathbf{z}_t\mid\mathbf{z}_{t-1};\phi)}\right]
$$

---

> VAE 的伪代码。

```python
class VariationalAutoEncoder(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.encoder = VAEEncoder()
        self.decoder = VAEDecoder()

    def forward(self, x: torch.Tensor) -> torch.Tensor:
        mu, logvar = self.encoder(x)
        std = torch.exp(0.5 * logvar)
        eps = torch.randn_like(std)
        z = mu + eps * std

        y = self.decoder(z)
        loss = (y - x).square().mean() + KL_wight * KL_divergence(mu, std)
        return y, loss
```
