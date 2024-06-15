# 2020 Score-based Generative Modeling Through Stochastic Differential Equations

## 0 Abstract

> Creating noise from data is easy; creating data from noise is generative modeling.

作者提出了一种基于分数（score）的生成建模方法，一个通过不断注入噪声来将一个复杂的分布逐渐转化为一个已知的先验分布的随机微分方程（SDE），以及一个对应的、通过逐渐去噪声来将先验分布转化为目标分布的逆时 SDE。特别的，逆时 SDE 只依赖于被扰动的数据分布的分数。因此，可以用网络准确地估计分数，并且用 SDE 数值求解器来生成样本。作者提出的框架统一了 Score Matching with Langevin Dynamics (SMLD) 和 Denoising Diffusion Probabilistic Models (DDPM)。

## 1 Introduction

- SMLD：先在不同噪声水平下估计分数，然后在生成过程中用 Langevin 动力学从一系列递减的噪声水平中采样。
- DDPM：利用了已知形式的反向过程，训练了一系列概率模型来逐渐去噪声，对于连续的状态空间，DDPM 的训练目标隐式地计算了不同噪声水平下的分数。

- 对于扩散过程，作者考虑了连续的状态空间，用一个与数据点无关 SDE 来描述。
- 对于反向过程，作者发现该过程符合逆时 SDE。

因此作者的贡献有 3 点：
1. 可以用 SDE 数值求解器来生成样本。具体来说提供了两个求解器：
    1. Predictor-Corrector 采样器：基于分数的 Monte Carlo Markov Chain (MCMC) 和 SDE 数值求解器。
    2. 基于概率流的 ODE 求解器。
2. 可控生成：带条件的逆时 SDE 可以从无条件的分数中估计。
3. 一个统一的框架：统一了 SMLD 和 DDPM。

## 3 Score-Based Generative Modeling with SDEs

### 3.1 Perturbing Data with SDEs

![](images/sgm.png)

前向过程（扩散过程）可以用 Ito SDE 描述：
$$
\mathrm{d}\mathbf{x} = \mathbf{f}(\mathbf{x}, t)\mathrm{d}t + g(\mathbf{x})\mathrm{d}\mathbf{w},
$$
其中 $\mathbf{f}:\mathbb{R}^{d}\mapsto\mathbb{R}^{d}$ 是 $\mathbf{x}$ 的漂移系数（drift coefficient），$g:\mathbb{R}^{d}\mapsto\mathbb{R}^{d}$ 是 $\mathbf{x}$ 的扩散系数（diffusion coefficient），$\mathbf{w}$ 是标准维纳过程。这里假设了 $g$ 是常数，而不是一个 $d\times d$ 的矩阵，且与 $\mathbf{x}$ 无关。当系数在状态之间和时间上是 Lipschitz 连续的时候，Ito SDE 有唯一解。

定义 $p_{t}(\mathbf{x})$ 为在时间 $t$ 时刻的概率密度函数，$p_{st}(\mathbf{x}\mid\mathbf{s})$ 为从 $\mathbf{s}$ 到 $\mathbf{x}$ 的转移核。一般来说，$p_{T}$ 就不含有任何 $p_{0}$ 的信息了，即 $p_{T}(\mathbf{x})$ 是一个无结构的先验分布。

### 3.2 Generating Samples by Reversing SDEs

反向过程也是一个扩散过程，可以用逆时 SDE 描述：
$$
\mathrm{d}\mathbf{x} = [\mathbf{f}(\mathbf{x}, t) - g(t)^2\nabla_{\mathbf{x}}\log p_{t}(\mathbf{x})]\mathrm{d}t + g(t)\mathrm{d}\bar{\mathbf{w}},
$$
其中 $\bar{\mathbf{w}}$ 也是标准维纳过程，但时间从 $T$ 到 $0$，$\mathrm{d}t$ 是负的时间微元。

### 3.3 Estimating Scores for SDEs

要估计 $\nabla_{\mathbf{x}}\log p_{t}(\mathbf{x})$，可以训练一个含时的神经网络 $s_{\theta}(\mathbf{x}, t)$：
$$
\theta^{*} = \mathop{\arg\min}_{\theta}\,\mathbb{E}_{t}\left\{\lambda(t)\mathbb{E}_{\mathbf{x}(0)}\mathbb{E}_{\mathbf{x}(t)\mid\mathbf{x}(0)}\left[\|s_{\theta}(\mathbf{x}(t),t)-\nabla_{\mathbf{x}(t)}\log_{p_{0t}}(\mathbf{x}(t)\mid\mathbf{x}(0))
\|^{2}_{2}\right]\right\}.
$$
其中 $\lambda(t):[0, T]\mapsto\mathbb{R}^{+}$ 是一个权重函数，用来调整不同时间点的重要性，$t$ 从 $[0, T]$ 的均匀分布中采样，$\mathbf{x}(0)\sim p_{0}$，$\mathbf{x}(t)\sim p_{0t}(\mathbf{x}(t)\mid\mathbf{x}(0))$。只要数据量和模型参数足够多，$s_{\theta^{*}}(\mathbf{x}, t)$ 就可以几乎处处逼近 $\nabla_{\mathbf{x}}\log p_{t}(\mathbf{x})$。
