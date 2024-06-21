# 2020 Score-based Generative Modeling Through Stochastic Differential Equations

## 0 Abstract

> Creating noise from data is easy; creating data from noise is generative modeling.

作者提出了一种基于分数（score）的生成建模方法，即通过不断注入噪声将一个复杂的分布逐渐转化为一个已知的先验分布的随机微分方程（SDE），以及一个通过逐渐去噪声将先验分布转化为目标分布的逆时 SDE。特别地，逆时 SDE 只依赖于被扰动的数据分布的分数。因此，可以用网络准确地估计分数，并且用 SDE 数值求解器来生成样本。作者提出的框架统一了 Score Matching with Langevin Dynamics (SMLD) 和 Denoising Diffusion Probabilistic Models (DDPM)。

## 1 Introduction

- **SMLD**：先在不同噪声水平下估计分数，然后在生成过程中用 Langevin 动力学从一系列递减的噪声水平中采样。
- **DDPM**：利用已知形式的反向过程，训练一系列概率模型来逐渐去噪声，对于连续的状态空间，DDPM 的训练目标隐式地计算了不同噪声水平下的分数。

- 对于扩散过程，作者考虑了连续的状态空间，用一个与数据点无关 SDE 来描述。
- 对于反向过程，作者发现该过程符合逆时 SDE。

因此，作者的贡献有三点：
1. 可以用 SDE 数值求解器来生成样本。具体来说提供了两个求解器：
    1. **Predictor-Corrector 采样器**：基于分数的 Monte Carlo Markov Chain (MCMC) 和 SDE 数值求解器。
    2. 基于概率流的 ODE 求解器。
2. **可控生成**：带条件的逆时 SDE 可以从无条件的分数中估计。
3. 一个**统一的框架**：统一了 SMLD 和 DDPM。

## 2 Related Work

### 2.1 Score Matching with Langevin Dynamics

SMLD 通过学习数据点的分数来生成样本，训练目标为：
$$
\theta^{*} = \arg \min_{\theta} \sum_{i=1}^{N} \sigma_{i}^{2} \mathbb{E}_{p_{\text{data}}(\mathbf{x})} \mathbb{E}_{p_{\sigma_{i}}(\tilde{\mathbf{x}} \mid \mathbf{x})} \left[\left\|\nabla_{\tilde{\mathbf{x}}} \log p_{\sigma_{i}}(\tilde{\mathbf{x}} \mid \mathbf{x}) - s_{\theta}(\tilde{\mathbf{x}}, \sigma_{i})\right\|^{2}_{2}\right],
$$
其中 $\sigma_{\min} = \sigma_{1} < \cdots < \sigma_{N} = \sigma_{\max}$ 是一系列噪声水平，$p_{\sigma_{i}}(\tilde{\mathbf{x}} \mid \mathbf{x}) = \mathcal{N}(\tilde{\mathbf{x}} \mid \mathbf{x}, \sigma_{i}^{2} \mathbf{I})$ 是给数据点 $\mathbf{x}$ 添加噪声，$p_{\sigma_{i}}(\tilde{\mathbf{x}}) = \int p_{\sigma_{i}}(\tilde{\mathbf{x}} \mid \mathbf{x}) p_{\text{data}}(\mathbf{x}) \mathrm{d} \mathbf{x}$ 是添加噪声后的数据分布，$s_{\theta}(\tilde{\mathbf{x}}, \sigma_{i})$ 是网络估计的分数。

### 2.2 Denoising Diffusion Probabilistic Models

DDPM 通过逐渐去噪声来生成样本，训练目标为：
$$
\theta^{*} = \arg \min_{\theta} (1 - \bar{\alpha}_{i}) \mathbb{E}_{p_{\text{data}}(\mathbf{x})} \mathbb{E}_{p_{\bar{\alpha}_{i}}(\tilde{\mathbf{x}} \mid \mathbf{x})} \left[\left\|\nabla_{\tilde{\mathbf{x}}} \log p_{\bar{\alpha}_{i}}(\tilde{\mathbf{x}} \mid \mathbf{x}) - s_{\theta}(\tilde{\mathbf{x}}, \bar{\alpha}_{i})\right\|^{2}_{2}\right],
$$
其中 $\bar{\alpha}_{i} = \prod_{j=1}^{i} (1 - \beta_{j})$ 是噪声水平，$p_{\bar{\alpha}_{i}}(\tilde{\mathbf{x}} \mid \mathbf{x}) = \mathcal{N}(\tilde{\mathbf{x}} \mid \sqrt{\bar{\alpha}_{i}} \mathbf{x}, (1 - \bar{\alpha}_{i}) \mathbf{I})$ 是给数据点 $\mathbf{x}$ 添加噪声，$p_{\bar{\alpha}_{i}}(\tilde{\mathbf{x}}) = \int p_{\bar{\alpha}_{i}}(\tilde{\mathbf{x}} \mid \mathbf{x}) p_{\text{data}}(\mathbf{x}) \mathrm{d} \mathbf{x}$ 是添加噪声后的数据分布，$s_{\theta}(\tilde{\mathbf{x}}, \bar{\alpha}_{i})$ 是网络估计的分数。

## 3 Score-Based Generative Modeling with SDEs

### 3.1 Perturbing Data with SDEs

![](images/sgm.png)

前向过程（扩散过程）可以用 Ito SDE 描述：
$$
\mathrm{d} \mathbf{x} = \mathbf{f}(\mathbf{x}, t) \mathrm{d} t + g(\mathbf{x}) \mathrm{d} \mathbf{w},
$$
其中 $\mathbf{f}:\mathbb{R}^{d} \mapsto \mathbb{R}^{d}$ 是 $\mathbf{x}$ 的漂移系数（drift coefficient），$g:\mathbb{R}^{d} \mapsto \mathbb{R}^{d}$ 是 $\mathbf{x}$ 的扩散系数（diffusion coefficient），$\mathbf{w}$ 是标准维纳过程。这里假设 $g$ 是常数，而不是一个 $d \times d$ 的矩阵，且与 $\mathbf{x}$ 无关。当系数在状态之间和时间上是 Lipschitz 连续的时候，Ito SDE 有唯一解。

定义 $p_{t}(\mathbf{x})$ 为在时间 $t$ 时刻的概率密度函数，$p_{st}(\mathbf{x} \mid \mathbf{s})$ 为从 $\mathbf{s}$ 到 $\mathbf{x}$ 的转移核。一般来说，$p_{T}$ 就不含有任何 $p_{0}$ 的信息了，即 $p_{T}(\mathbf{x})$ 是一个无结构的先验分布。

### 3.2 Generating Samples by Reversing SDEs

反向过程也是一个扩散过程，可以用逆时 SDE 描述：
$$
\mathrm{d} \mathbf{x} = [\mathbf{f}_{t}(\mathbf{x}) - g_{t}^2 \nabla_{\mathbf{x}} \log p_{t}(\mathbf{x})] \mathrm{d} t + g_{t} \mathrm{d} \bar{\mathbf{w}},
$$
其中 $\bar{\mathbf{w}}$ 也是标准维纳过程，但时间从 $T$ 到 $0$，$\mathrm{d} t$ 是负的时间微元。

### 3.3 Estimating Scores for SDEs

要估计 $\nabla_{\mathbf{x}} \log p_{t}(\mathbf{x})$，可以训练一个含时的神经网络 $\mathbf{s}_{\theta}(\mathbf{x}, t)$：
$$
\theta^{*} = \mathop{\arg\min}_{\theta} \mathbb{E}_{t}\left\{ \lambda_{t} \mathbb{E}_{\mathbf{x}_{0}} \mathbb{E}_{\mathbf{x}_{t} \mid \mathbf{x}_{0}} \left[ \| \mathbf{s}_{\theta}(\mathbf{x}_{t},t) - \nabla_{\mathbf{x}_{t}} \log p_{0t}(\mathbf{x}_{t} \mid \mathbf{x}_{0}) \|^{2}_{2} \right] \right\}.
$$
其中 $\lambda_{t}: [0, T] \mapsto \mathbb{R}^{+}$ 是一个权重函数，用来调整不同时间点的重要性，$t$ 从 $[0, T]$ 的均匀分布中采样，$\mathbf{x}_{0} \sim p_{0}$，$\mathbf{x}_{t} \sim p_{0t}(\mathbf{x}_{t} \mid \mathbf{x}_{0})$。只要数据量和模型参数足够多，$s_{\theta^{*}}(\mathbf{x}, t)$ 就可以几乎处处逼近 $\nabla_{\mathbf{x}} \log p_{t}(\mathbf{x})$。

## 4 Solving the Reverse SDE



## Appendix

之前的 Diffusion 都是离散的，而这里是连续的：
$$
\mathrm{d} \mathbf{x} = \mathbf{f}_{t}(\mathbf{x}) \mathrm{d} t + g_{t} \mathrm{d} \mathbf{w},
$$
其中：
$$
\mathrm{d} \mathbf{x} = \lim_{\Delta t \to 0} \mathbf{x}_{t + \Delta t} - \mathbf{x}_{t}
$$
所以之前离散的 Diffusion 前向过程为 $\mathbf{x}_{t} \to \mathbf{x}_{t + 1}$，$t \in \{0, 1, \ldots, T\}$，而这里连续的 Diffusion 前向过程为 $\mathbf{x}_{t} \to \mathbf{x}_{t + \Delta t}$，$t \in [0, 1]$。

带入得到：
$$
\mathbf{x}_{t + \Delta t} = \mathbf{x}_{t} + \mathbf{f}_{t}(\mathbf{x}_{t}) \Delta t + g_{t} \sqrt{\Delta t} \boldsymbol{\epsilon}.
$$
用概率形式表达：
$$
p(\mathbf{x}_{t + \Delta t} \mid \mathbf{x}_{t}) = \mathcal{N}(\mathbf{x}_{t} + \mathbf{f}_{t}(\mathbf{x}_{t}) \Delta t, g_{t}^{2} \Delta t \mathbf{I}).
$$
为了得到反向的 SDE，就要得到 $p(\mathbf{x}_{t} \mid \mathbf{x}_{t + \Delta t})$：
$$
\begin{align*}
p(\mathbf{x}_{t} \mid \mathbf{x}_{t + \Delta t}) &= \frac{p(\mathbf{x}_{t + \Delta t} \mid \mathbf{x}_{t}) p(\mathbf{x}_{t})}{p(\mathbf{x}_{t + \Delta t})} \\
&= p(\mathbf{x}_{t + \Delta t} \mid \mathbf{x}_{t}) \frac{p(\mathbf{x}_{t})}{p(\mathbf{x}_{t + \Delta t})} \\
&= p(\mathbf{x}_{t + \Delta t} \mid \mathbf{x}_{t}) \exp\left(\log p(\mathbf{x}_{t}) - \log p(\mathbf{x}_{t + \Delta t})\right) \\
&\propto \exp \left( -\frac{\|\mathbf{x}_{t + \Delta t} - \mathbf{x}_{t} - \mathbf{f}_{t}(\mathbf{x}_{t}) \Delta t\|^{2}_{2}}{2 g_{t}^{2} \Delta t} + \log p(\mathbf{x}_{t}) - \log p(\mathbf{x}_{t + \Delta t}) \right).
\end{align*}
$$
用 Taylor 展开处理 $\log p(\mathbf{x}_{t + \Delta t})$：
$$
\log p(\mathbf{x}_{t + \Delta t}) = \log p(\mathbf{x}_{t}) + (\mathbf{x}_{t + \Delta t} - \mathbf{x}_{t})^{\top} \nabla_{\mathbf{x_{t}}} \log p(\mathbf{x}_{t}) + \Delta t \frac{\partial}{\partial t} \log p(\mathbf{x}_{t}) + \cdots.
$$
带回得到：
$$
p(\mathbf{x}_{t} \mid \mathbf{x}_{t + \Delta t}) \propto \exp \left( -\frac{\|\mathbf{x}_{t + \Delta t} - \mathbf{x}_{t} - [\mathbf{f}_{t}(\mathbf{x}_{t}) - g_{t}^{2}\nabla_{\mathbf{x_{t}}} \log p(\mathbf{x}_{t})] \Delta t \|^{2}_{2} }{2 g_{t}^{2} \Delta t} + O(\Delta t) \right).
$$
注意到上面的式子很多都是和 $\mathbf{x}_{t}$ 有关的，但实际上已知的是 $\mathbf{x}_{t + \Delta t}$，所以要重写为 $\mathbf{x}_{t + \Delta t}$ 的形式。注意到当 $\Delta t \to 0$ 时，$O(\Delta t) \to 0$，$t \approx t + \Delta t$，所以：
$$
\begin{align*}
p(\mathbf{x}_{t} \mid \mathbf{x}_{t + \Delta t}) &\propto \exp \left( -\frac{\|\mathbf{x}_{t + \Delta t} - \mathbf{x}_{t} - [\mathbf{f}_{t}(\mathbf{x}_{t}) - g_{t}^{2}\nabla_{\mathbf{x_{t}}} \log p(\mathbf{x}_{t})] \Delta t \|^{2}_{2} }{2 g_{t}^{2} \Delta t} \right) \\
&\approx \exp \left( -\frac{\|\mathbf{x}_{t} - \mathbf{x}_{t + \Delta t} + [\mathbf{f}_{t + \Delta t}(\mathbf{x}_{t + \Delta t}) - g_{t}^{2}\nabla_{\mathbf{x_{t + \Delta t}}} \log p(\mathbf{x}_{t + \Delta t})] \Delta t \|^{2}_{2} }{2 g_{t}^{2} \Delta t} \right).
\end{align*}
$$
因此：
$$
p(\mathbf{x}_{t} \mid \mathbf{x}_{t + \Delta t}) = \mathcal{N}(\mathbf{x}_{t + \Delta t} + [\mathbf{f}_{t + \Delta t}(\mathbf{x}_{t + \Delta t}) - g_{t}^{2}\nabla_{\mathbf{x_{t + \Delta t}}} \log p(\mathbf{x}_{t + \Delta t})] \Delta t, g_{t}^{2} \Delta t \mathbf{I}).
$$
取 $\Delta t \to 0$，对应的 SDE 为：
$$
\mathrm{d} \mathbf{x} = [\mathbf{f}_{t}(\mathbf{x}) - g_{t}^{2} \nabla_{\mathbf{x}} \log p_{t}(\mathbf{x})] \mathrm{d} t + g_{t} \mathrm{d} \bar{\mathbf{w}}.
$$

---

对于 SLMD，扩散公式为：
$$
\mathbf{x}_{t} = \mathbf{x}_{0} + \sigma_{t} \boldsymbol{\epsilon},
$$
因此有：
$$
\begin{align*}
\mathbf{x}_{t + \Delta t} &= \mathbf{x}_{t} + \sqrt{\sigma_{t + \Delta t}^{2} - \sigma_{t}^{2}} \boldsymbol{\epsilon} \\
&= \mathbf{x}_{t} + \sqrt{\frac{\sigma_{t + \Delta t}^{2} - \sigma_{t}^{2}}{\Delta t}} \sqrt{\Delta t}\boldsymbol{\epsilon} \\
&= \mathbf{x}_{t} + \sqrt{\frac{\Delta \sigma_{t}^{2}}{\Delta t}} \boldsymbol{\epsilon}.
\end{align*}
$$
对应 $\mathbf{f}_{t}(\mathbf{x}) = 0$，$g_{t} = \sqrt{\frac{\mathrm{d} \sigma_{t}^{2}}{\mathrm{d} t}}$。

对于 DDPM，扩散公式为：
$$
\mathbf{x}_{t + 1} = \sqrt{1 - \beta_{t + 1}} \mathbf{x}_{t} + \sqrt{\beta_{t + 1}} \boldsymbol{\epsilon},
$$
需要定义一个新的序列 $\{\bar{\beta}_t = T \beta_t\}_{t=1}^{T}$：
$$
\mathbf{x}_{t + 1} = \sqrt{1 - \frac{\bar{\beta}_{t + 1}}{T}} \mathbf{x}_{t} + \sqrt{\frac{\bar{\beta}_{t + 1}}{T}} \boldsymbol{\epsilon}.
$$
当 $T \to \infty$ 时，$\bar{\beta}_{t}$ 变成了函数 $\beta(t)$，其中 $t \in [0, 1]$。因此得到 $\beta(t / T) = \bar{\beta}_{t}$，$\Delta t = 1 / T$。因此有：
$$
\begin{align*}
\mathbf{x}_{t + \Delta t} &= \sqrt{1 - \beta(t + \Delta t) \Delta t} \mathbf{x}_{t} + \sqrt{\beta(t + \Delta t) \Delta t} \boldsymbol{\epsilon} \\
&\approx \left(1 - \frac{1}{2} \beta(t + \Delta t) \Delta t\right) \mathbf{x}_{t} + \sqrt{\beta(t + \Delta t) \Delta t} \boldsymbol{\epsilon} \\
&\approx \mathbf{x}_{t} - \frac{1}{2} \beta(t) \Delta t \mathbf{x}_{t} + \sqrt{\beta(t)} \sqrt{\Delta t} \boldsymbol{\epsilon}.
\end{align*}
$$
对应 $\mathbf{f}_{t}(\mathbf{x}) = -\frac{1}{2} \beta(t) \mathbf{x}$，$g_{t} = \sqrt{\beta(t)}$。
