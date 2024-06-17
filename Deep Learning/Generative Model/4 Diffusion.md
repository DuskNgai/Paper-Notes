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

Diffusion 模型的前向过程是数据逐步加噪的过程。根据 Diffusion 模型的第一个特点和 MHVAE 的性质，前向过程的联合分布为：
$$
q(\mathbf{x}_{1:T} \mid \mathbf{x_{0}}) = \prod_{t=1}^{T} q(\mathbf{x}_{t} \mid \mathbf{x}_{t-1})
$$
其中 $\mathbf{x}_{0} \sim q(\mathbf{x}_{0})$ 是给定的无噪声数据。

根据 Diffusion 模型的第二个特点，$\mathbf{x}_{0}$ 通过总共 $T$ 步的加 Gaussian 噪声过程，得到一系列的加噪数据 $\left\{\mathbf{x}_t\right\}_{t=1}^T$。每一步的加噪过程的方差为 $\left\{\beta_t\right\}_{t=1}^T$，因此每一步的加噪过程的分布为：
$$
\begin{align*}
q(\mathbf{x}_{t} \mid \mathbf{x}_{t-1}) &= \mathcal{N}(\mathbf{x}_{t}; \sqrt{1-\beta_{t}} \mathbf{x}_{t-1}, \beta_{t} \mathbf{I}) \\
&= \mathcal{N}(\mathbf{x}_{t}; \sqrt{\alpha_{t}} \mathbf{x}_{t-1}, (1-\alpha_{t}) \mathbf{I})
\end{align*}
$$
$\beta_t$ 是一个超参数，$\alpha_{t} := 1 - \beta_{t}$。

前向过程的一个重要性质是，$\mathbf{x}_t$ 的分布可以由 $\mathbf{x}_{0}$ 直接得到：
$$
q(\mathbf{x}_{t} \mid \mathbf{x}_{0}) = \mathcal{N}\left(\mathbf{x}_{t}; \sqrt{\bar{\alpha}_{t}} \mathbf{x}_{0}, (1-\bar{\alpha}_{t}) \mathbf{I}\right)
$$
其中 $\bar{\alpha}_{t} = \prod_{s=1}^{t} \alpha_{s}$。推导见 [Add Noise in Single Step](appendix/Prove%20Diffusion.md#Add%20Noise%20in%20Single%20Step)。

## Reversed Process

Diffusion 模型的反向过程是数据逐步去噪的过程。根据 Diffusion 模型的第三个特点，我们可以认为解码从 $\mathbf{x}_{T} \sim \mathcal{N}(\mathbf{0}, \mathbf{I})$ 开始，反向过程的联合分布为：
$$
p(\mathbf{x}_{0:T}; \theta) = p(\mathbf{x}_{T}) \prod_{t=1}^{T} p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)
$$
其中 $p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)$ 是一个用网络建模的分布。

这里，我们可以写出 MHVAE 中的 ELBO 在这套记号下的形式：
$$
\mathrm{ELBO}(q, \mathbf{x}; \theta) = \mathbb{E}_{q(\mathbf{x}_{1:T} \mid \mathbf{x}_{0})} \left[\log \frac{p(\mathbf{x}_{T}) \prod_{t=1}^{T} p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)}{\prod_{t=1}^{T} q(\mathbf{x}_{t} \mid \mathbf{x}_{t-1})}\right]
$$
类似于 VAE，我们可以将 ELBO 分解：
$$
\begin{align*}
\mathrm{ELBO}(q, \mathbf{x}; \theta) &= \underbrace{\mathbb{E}_{q(\mathbf{x}_{1} \mid \mathbf{x}_{0})} \log p(\mathbf{x}_{0} \mid \mathbf{x}_{1}; \theta)}_{\text{Reconstruction Term}} \\
&\quad - \underbrace{\mathrm{KL}[q(\mathbf{x}_{T} \mid \mathbf{x}_{0}) \parallel p(\mathbf{x}_{T})]}_{\text{Prior Matching Term}} \\
&\quad - \sum_{t=2}^{T} \underbrace{\mathbb{E}_{q(\mathbf{x}_{t} \mid \mathbf{x}_{0})}[\mathrm{KL}[q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) \parallel p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)]]}_{\text{Denoising Matching Term}}
\end{align*}
$$
推导见 [Decomposition of ELBO](appendix/Prove%20Diffusion.md#Decomposition%20of%20ELBO)。这里每一项的含义分别是：

1. **Reconstruction Term**：VAE 中也有这一项，用来衡量模型的重建能力。
2. **Prior Matching Term**：VAE 中也有这一项，用来衡量 $q(\mathbf{x}_{T} \mid \mathbf{x}_{0})$ 和 $p(\mathbf{x}_{T})$ 之间的差异。这一项没有可学习的参数，不能优化。
3. **Denoising Matching Term**：用来衡量 $q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0})$ 和 $p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)$ 之间的差异，其中 $p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)$ 去主动接近 GT $q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0})$。

### Denoising Matching Term

以下是 Denoising Matching Term 具体形式的推导过程。

首先，根据 Bayes 公式和 Markov 链的性质，有：
$$
q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) = \frac{q(\mathbf{x}_{t} \mid \mathbf{x}_{t-1}, \mathbf{x}_{0}) q(\mathbf{x}_{t-1} \mid \mathbf{x}_{0})}{q(\mathbf{x}_{t} \mid \mathbf{x}_{0})} = \frac{q(\mathbf{x}_{t} \mid \mathbf{x}_{t-1}) q(\mathbf{x}_{t-1} \mid \mathbf{x}_{0})}{q(\mathbf{x}_{t} \mid \mathbf{x}_{0})}
$$
从前向过程中可知 $q(\mathbf{x}_{t} \mid \mathbf{x}_{0}) = \mathcal{N}(\mathbf{x}_{t}; \sqrt{\bar{\alpha}_{t}} \mathbf{x}_{0}, (1-\bar{\alpha}_{t}) \mathbf{I})$，$q(\mathbf{x}_{t} \mid \mathbf{x}_{t-1}) = \mathcal{N}(\mathbf{x}_{t}; \sqrt{\alpha_{t}} \mathbf{x}_{t-1}, (1-\alpha_{t}) \mathbf{I})$，带入化简得：
$$
q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) = \mathcal{N}\left(\mathbf{x}_{t-1}; \frac{(1-\bar{\alpha}_{t-1}) \sqrt{\alpha_{t}} \mathbf{x}_{t} + (1-\alpha_{t}) \sqrt{\bar{\alpha}_{t-1}} \mathbf{x}_{0}}{1-\bar{\alpha}_{t}}, \frac{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_{t}} \mathbf{I}\right)
$$
推导见 [Denoising Matching Term](appendix/Prove%20Diffusion.md#Denoising%20Matching%20Term)。

到这一步，$q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0})$ 已经确定为是一个 Gaussian 分布，均值和方差分别为：
$$
\boldsymbol{\mu}_{q}(t) = \frac{(1-\bar{\alpha}_{t-1}) \sqrt{\alpha_{t}} \mathbf{x}_{t} + (1-\alpha_{t}) \sqrt{\bar{\alpha}_{t-1}} \mathbf{x}_{0}}{1-\bar{\alpha}_t} \quad \sigma_{q}^{2}(t) = \frac{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t}
$$
可以发现，所有的 $\alpha$ 和 $\mathbf{x}_{0}$ 是已知的，$\mathbf{x}_{t}$ 是这一步的输入。因此，我们可以将 $p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)$ 建模为方差已知的 Gaussian 分布，让优化的目标尽可能的简单：
$$
p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta) = \mathcal{N}(\mathbf{x}_{t-1}; \boldsymbol{\mu}(\mathbf{x}_t, t; \theta), \sigma_{q}^2(t) \mathbf{I})
$$
其中 $\boldsymbol{\mu}(\mathbf{x}_t, t; \theta)$ 是待优化的均值。结合 Gaussian 分布的 KL 散度是解析的特点，可以得到一个简单的优化目标：
$$
\mathrm{KL}[q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) \parallel p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)] = \frac{ \|\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta) \|^2}{2 \sigma_{q}^2(t)}
$$

推导见 [Matching Mean](appendix/Prove%20Diffusion.md#Matching%20Mean)。针对于这个优化目标，显然还可以进一步简化。这里有三种等价的简化方式。

#### Learning Predicting the Original Data

第一种简化方式的最终结果是重建数据。可以将 $\boldsymbol{\mu}(\mathbf{x}_t, t; \theta)$ 的形式建模得和 $\boldsymbol{\mu}(t)$ 更加接近，即：
$$
\boldsymbol{\mu}(\mathbf{x}_t, t; \theta) = \frac{(1-\bar{\alpha}_{t-1}) \sqrt{\alpha_{t}} \mathbf{x}_{t} + (1-\alpha_{t}) \sqrt{\bar{\alpha}_{t-1}} \mathbf{x}(\mathbf{x}_{t}, t; \theta)}{1-\bar{\alpha}_t}
$$
带入二者和 $\sigma_{q}^2(t)$ 的表达式得到：
$$
\begin{align*}
\mathrm{KL}[q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) \parallel p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)] &= \frac{1}{2} \left( \frac{\bar{\alpha}_{t-1}}{1-\bar{\alpha}_{t-1}} - \frac{\bar{\alpha}_{t}}{1-\bar{\alpha}_t} \right) \left\| \mathbf{x}_{0} - \mathbf{x}(\mathbf{x}_t, t; \theta) \right\|^2 \\
&= \frac{1}{2} \left(\text{SNR}(t-1) - \text{SNR}(t)\right) \left\| \mathbf{x}_{0} - \mathbf{x}(\mathbf{x}_t, t; \theta) \right\|^2
\end{align*}
$$
其中 SNR 定义为：
$$
\text{SNR}(t) = \frac{\bar{\alpha}_{t}}{1-\bar{\alpha}_{t}}
$$
所以，只要将网络设计为输入 $t$ 时刻和对应的加噪数据，输出一个去噪数据，就可以让 Denoising Matching Term 最小化。综合多步的 Denoising Matching Term 为：
$$
\sum_{t=2}^{T} \frac{1}{2} \left(\text{SNR}(t-1) - \text{SNR}(t)\right) \mathbb{E}_{q(\mathbf{x}_{t} \mid \mathbf{x}_{0})} [\left\| \mathbf{x}_{0} - \mathbf{x}(\mathbf{x}_t, t; \theta) \right\|^2]
$$


#### Learning Predicting the Noise

第二种简化方式的最终结果是学习噪声，DDPM 选择的是这一种。已知：
$$
\begin{align*}
\mathbf{x}_{t} &= \sqrt{\bar{\alpha}_{t}} \mathbf{x}_{0} + \sqrt{1-\bar{\alpha}_{t}} \boldsymbol{\epsilon}_{0} \\
\mathbf{x}_{0} &= \frac{1}{\sqrt{\bar{\alpha}_{t}}} \mathbf{x}_{t} - \frac{\sqrt{1-\bar{\alpha}_{t}}}{\sqrt{\bar{\alpha}_{t}}} \boldsymbol{\epsilon}_{0}
\end{align*}
$$
带入 $\boldsymbol{\mu}_{q}(t)$ 的表达式得到：
$$
\boldsymbol{\mu}_{q}(t) = \frac{1}{\sqrt{\alpha}_{t}} \mathbf{x}_{t} - \frac{(1-\alpha_{t})}{\sqrt{1-\bar{\alpha}_{t}} \sqrt{\alpha}_{t}} \boldsymbol{\epsilon}_{0}
$$
所以，$\boldsymbol{\mu}(\mathbf{x}_t, t; \theta)$ 的形式可以建模为：
$$
\boldsymbol{\mu}(\mathbf{x}_t, t; \theta) = \frac{1}{\sqrt{\alpha}_{t}} \mathbf{x}_{t} - \frac{(1-\alpha_{t})}{\sqrt{1-\bar{\alpha}_{t}} \sqrt{\alpha}_{t}} \boldsymbol{\epsilon}(\mathbf{x}_t, t; \theta)
$$
此时 KL 散度为：
$$
\begin{align*}
\mathrm{KL}[q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) \parallel p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)] &= \frac{1}{2} \left(\frac{(1-\bar{\alpha}_{t})\bar{\alpha}_{t-1}}{(1-\bar{\alpha}_{t-1})\bar{\alpha}_{t}} - 1\right) \left\| \boldsymbol{\epsilon}_{0} - \boldsymbol{\epsilon}(\mathbf{x}_t, t; \theta) \right\|^2 \\
&= \frac{1}{2} \left(\frac{\text{SNR}(t-1)}{\text{SNR}(t)} - 1\right) \left\| \boldsymbol{\epsilon}_{0} - \boldsymbol{\epsilon}(\mathbf{x}_t, t; \theta) \right\|^2
\end{align*}
$$
因此模型**只要输入 $t$ 时刻和对应的加噪图像，输出一个噪声去接近 $t=1$ 时刻加入的噪声**即可。综合多步的 Denoising Matching Term 为：
$$
\sum_{t=2}^{T} \frac{1}{2} \left(\frac{\text{SNR}(t-1)}{\text{SNR}(t)} - 1\right) \mathbb{E}_{q(\mathbf{x}_{t} \mid \mathbf{x}_{0})} [\left\| \boldsymbol{\epsilon}_{0} - \boldsymbol{\epsilon}(\mathbf{x}_t, t; \theta) \right\|^2]
$$

#### Learning Predicting the Score

第三种简化方式的最终结果是学习分数。首先需要介绍 Tweedie's formula。对于符合 Gaussian 分布的随机变量 $\mathbf{z}\sim\mathcal{N}(\boldsymbol{\mu}_{z}, \boldsymbol{\Sigma}{z})$，Tweedie's formula 为：
$$
\mathbb{E}[\boldsymbol{\mu}_{z} \mid \mathbf{z}] = \boldsymbol{\mu}_{z} + \boldsymbol{\Sigma}_{z} \nabla_{\mathbf{z}} \log \mathbb{Pr}(\mathbf{z})
$$
因此，对于 $q(\mathbf{x}_{t} \mid \mathbf{x}_{0}) = \mathcal{N}(\mathbf{x}_{t}; \sqrt{\bar{\alpha}_{t}} \mathbf{x}_{0}, (1-\bar{\alpha}_{t}) \mathbf{I})$，有 Tweedie's formula：
$$
\mathbb{E}[\sqrt{\bar{\alpha}_{t}} \mathbf{x}_{0} \mid \mathbf{x}_{t}] = \mathbf{x}_{t} + (1-\bar{\alpha}_{t}) \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})
$$
其中 $\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})$ 是 $\mathbf{x}_{t}$ 的分数。因此，均值 $\sqrt{\bar{\alpha}_{t}} \mathbf{x}_{0}$ 最好的估计是：
$$
\begin{align*}
\sqrt{\bar{\alpha}_{t}} \mathbf{x}_{0} &= \mathbf{x}_{t} + (1-\bar{\alpha}_{t}) \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) \\
\mathbf{x}_{0} &= \frac{1}{\sqrt{\bar{\alpha}_{t}}} \mathbf{x}_{t} + \frac{1-\bar{\alpha}_{t}}{\sqrt{\bar{\alpha}_{t}}} \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})
\end{align*}
$$
带入 $\boldsymbol{\mu}_{q}(t)$ 的表达式得到：
$$
\boldsymbol{\mu}_{q}(t) = \frac{1}{\sqrt{\alpha}_{t}} \mathbf{x}_{t} + \frac{(1-\alpha_{t})}{\sqrt{\alpha}_{t}} \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})
$$
所以，$\boldsymbol{\mu}(\mathbf{x}_t, t; \theta)$ 的形式可以建模为：
$$
\boldsymbol{\mu}(\mathbf{x}_t, t; \theta) = \frac{1}{\sqrt{\alpha}_{t}} \mathbf{x}_{t} + \frac{(1-\alpha_{t})}{\sqrt{\alpha}_{t}} \mathbf{s}(\mathbf{x}_t, t; \theta)
$$
其中 $\mathbf{s}(\mathbf{x}_t, t; \theta)$ 是拟合 $\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})$ 的网络。此时 KL 散度为：
$$
\begin{align*}
\mathrm{KL}[q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) \parallel p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)] &= \frac{1}{2} \left(\frac{\bar{\alpha}_{t-1}}{(1-\bar{\alpha}_{t-1})\bar{\alpha}_{t}} - 1\right) \left\| \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) - \mathbf{s}(\mathbf{x}_t, t; \theta) \right\|^2 \\
&= \frac{1}{2} \frac{1}{1 - \bar{\alpha}_{t}} \left(\frac{\text{SNR}(t-1)}{\text{SNR}(t)} - 1\right) \left\| \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) - \mathbf{s}(\mathbf{x}_t, t; \theta) \right\|^2
\end{align*}
$$
因此模型**只要输入 $t$ 时刻和对应的加噪图像，输出一个分数去接近 $\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})$** 即可。综合多步的 Denoising Matching Term 为：
$$
\sum_{t=2}^{T} \frac{1}{2} \frac{1}{1 - \bar{\alpha}_{t}} \left(\frac{\text{SNR}(t-1)}{\text{SNR}(t)} - 1\right) \mathbb{E}_{q(\mathbf{x}_{t} \mid \mathbf{x}_{0})} [\left\| \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) - \mathbf{s}(\mathbf{x}_t, t; \theta) \right\|^2]
$$

此外，分数 $\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})$ 和噪声 $\boldsymbol{\epsilon}_{0}$ 之间的关系是：
$$
\begin{align*}
\mathbf{x}_{0} = \frac{1}{\sqrt{\bar{\alpha}_{t}}} \mathbf{x}_{t} + \frac{1-\bar{\alpha}_{t}}{\sqrt{\bar{\alpha}_{t}}} \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) &= \frac{1}{\sqrt{\bar{\alpha}_{t}}} \mathbf{x}_{t} - \frac{\sqrt{1-\bar{\alpha}_{t}}}{\sqrt{\bar{\alpha}_{t}}} \boldsymbol{\epsilon}_{0} \\
\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) &= -\frac{1}{\sqrt{1-\bar{\alpha}_{t}}} \boldsymbol{\epsilon}_{0}
\end{align*}
$$
分数衡量的是如何在数据空间中移动以最大化对数概率；直观上，它们是噪声的负方向，往分数的方向移动可以让 $\mathbf{x}_{t}$ 去噪。

### Reconstruction Term

DDPM 在图像数据上估计 Reconstruction Term。它们将图像数据从 $\{0, \dots, 255\}$ 转化到 $[-1, 1]$，然后把联合分布分解到各个边缘分布上，计算离散的边缘分布：
$$
\begin{align*}
p(\mathbf{x}_{0} \mid \mathbf{x}_{1}; \theta) &= \mathcal{N}(\mathbf{x}_{0}; \boldsymbol{\mu}(\mathbf{x}_{1}, t; \theta), \sigma_{q}^2(1) I) \\
&= \prod_{i=1}^{n} \int_{\delta_{-}(x_{0}^{i})}^{\delta_{+}(x_{0}^{i})} \mathcal{N}(x; \mu^{i}(\mathbf{x}_{1}, t; \theta), \sigma_{q}^2(1)) \mathrm{d}x
\end{align*}
$$
其中 $n$ 是 $\mathbf{x}_{0}$ 的维度，
$$
\delta_{-}(x) = \begin{cases}
-\infty & \text{if } x = -1 \\
x - \frac{1}{255} & \text{if } x > -1
\end{cases} \qquad
\delta_{+}(x) = \begin{cases}
\infty & \text{if } x = 1 \\
x + \frac{1}{255} & \text{if } x < 1
\end{cases}
$$
这一项也可以不加入最后的 loss 计算，影响不大。

## Score-Based Generative Models

SGM 从另外一个角度解释了 Diffusion 模型。SGM 的出发点是能量函数 $E(\mathbf{x})$，它定义了数据的概率密度函数：
$$
\mathbb{Pr}(\mathbf{x}) = \frac{\exp(-E(\mathbf{x}))}{\int_{\mathbb{R}^{n}} \exp(-E(\mathbf{x})) \mathrm{d}\mathbf{x}}
$$
它的分数是：
$$
\nabla_{\mathbf{x}} \log \mathbb{Pr}(\mathbf{x}) = -\nabla_{\mathbf{x}} E(\mathbf{x})
$$
SGM 的优化目标是用网络 $\mathbf{s}(\mathbf{x}; \theta)$ 拟合分数：
$$
\mathbb{E}_{\mathbf{x} \sim \mathbb{Pr}(\mathbf{x})} \left[\left\| \nabla_{\mathbf{x}} \log \mathbb{Pr}(\mathbf{x}) - \mathbf{s}(\mathbf{x}; \theta) \right\|^2\right]
$$

那么分数究竟衡量了什么？由于分数是对数概率的梯度，因此朝着分数的方向移动可以最大化对数概率，Langevin 动力学就是利用了这一性质。简短来说，我们可以在数据空间中随便采样一个点，Langevin 动力学会用分数的方向去移动这个点：
$$
\mathbf{x}_{t+1} = \mathbf{x}_{t} + c \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) + \sqrt{2c}\boldsymbol{\epsilon}_{t}
$$
其中 $c$ 是学习率，$\boldsymbol{\epsilon}_{t}$ 是 Gaussian 噪声，目的是不让 Langevin 动力学的结果塌缩到一个局部最优解，增加生成结果的多样性。因此，SGM 的生成过程是一个 Langevin 动力学的过程。

SGM 也有自己的问题：
1. 数据流形的维度很低，而数据空间的维度很高，这会导致数据空间大部份区域的概率密度为 0，无法计算分数。
2. 优化过程也是需要采样的，在概率密度低的区域计算损失函数方差很大，优化困难。
3. 当概率密度为 $\mathbb{Pr}(\mathbf{x}) = c_1 \mathbb{Pr}_1(\mathbf{x}) + c_2 \mathbb{Pr}_2(\mathbf{x})$ 时，SGM 无法区分两个分布的权重。

当 SGM 和 DDPM 结合后，通过在数据空间中添加一系列 Gaussian 噪声的扰动，可以很好地解决 SGM 的问题。给定一组方差 $\left\{\sigma_{t}^2\right\}_{t=1}^T$，一次扰动后 $\mathbf{x}_{t}$ 的分布为：
$$
\mathbb{Pr}_{\sigma_{t}}(\mathbf{x}_{t}) = \int_{\mathbb{R}^{n}} \mathbb{Pr}(\mathbf{x}) \mathcal{N}(\mathbf{x}_{t}; \mathbf{x}, \sigma_{t} \mathbf{I}) \mathrm{d}\mathbf{x}
$$
这样，SGM 的问题被很好的解决了：
1. 通过添加噪声，数据空间的概率密度不会为 0。
2. 通过添加噪声，优化过程中概率密度低的区域更容易被采样。
3. 通过添加噪声，权重大的区域会被更多的噪声覆盖，从而可以区分两个分布的权重。

## Guidance

以图像分类任务为例，我们需要学习条件概率 $\mathbb{Pr}(\mathbf{x} \mid y)$，那么该如何设计损失函数呢？

### Classifier Guidance

分解此时的分数：
$$
\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t} \mid y) = \underbrace{\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})}_{\text{Unconditional Score}} + \underbrace{\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(y \mid \mathbf{x}_{t})}_{\text{Adversarial Gradient}}
$$
因此需要额外训练一个分类器，用来计算 $\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(y \mid \mathbf{x}_{t})$。但不需要重新训练 Diffusion 模型。实际训练时，损失函数为：
$$
\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t} \mid y) = \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) + \gamma \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(y \mid \mathbf{x}_{t})
$$


### Classifier Free Guidance

$$
\begin{align*}
\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t} \mid y) &= \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) + \gamma \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(y \mid \mathbf{x}_{t}) \\
&= \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t}) + \gamma (\nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t} \mid y) - \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})) \\
&= \underbrace{\gamma \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t} \mid y)}_{\text{Conditional Score}} + \underbrace{(1 - \gamma) \nabla_{\mathbf{x}_{t}} \log \mathbb{Pr}(\mathbf{x}_{t})}_{\text{Unconditional Score}}
\end{align*}
$$
同时训练带条件的 Diffusion 模型和无条件的 Diffusion 模型，这两个模型实际上是同一个模型，只是输入的不同。在推理阶段，无条件的 Diffusion 模型可以用来生成样本，而带条件的 Diffusion 模型可以用来生成带条件的样本。然后将二者的结果插值一下，就可以最终得到带条件的样本。


---

> DDPM 的伪代码

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
