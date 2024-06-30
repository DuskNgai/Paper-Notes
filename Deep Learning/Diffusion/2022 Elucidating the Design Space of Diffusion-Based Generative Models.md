# Elucidating the Design Space of Diffusion-Based Generative Models

## Claim

对于采样、训练过程以及分数网络的前处理（pre-conditioning）进行了模块化。EDM 认为，采样步骤应当是和训练细节是分开的。

- 更加关注于 Diffusion 模型中可以拆分出来的算法和对象。
- 改进了采样过程，分析并改进了离散化、Runge-Kutta 求解器、随机性的作用等。
- 改进了训练过程，分析了改进网络输入、输出和损失函数的前提条件。

## Motivation

现有的理论和实践中，对于扩散模型的设计空间没有进行详细的探讨。具体来说，改变一种超参数可能会影响多个方面，而这些方面之间的关系并不清晰。

## Method

### Sampling

Diffusion 模型采样时的步长 $\{t_{i}\}$ 应该随着 $\sigma$ 的减小而减小，而且和模型输入无关。具体来说，EDM 让 $t_{i} = \sigma^{-1}(\sigma_{i})$，且 $\sigma_{0} = \max_{i}\{\sigma_{i}\}$, $\sigma_{N - 1} = \min_{i}\{\sigma_{i}\}$, $\sigma_{N} = 0$。为了维持反函数的存在，$\sigma_{i}$ 满足：
$$
\sigma_{i} = \left((\min_{i}\{\sigma_{i}\})^{1/\rho} + \frac{i}{N - 1}\left((\max_{i}\{\sigma_{i}\})^{1/\rho} - (\min_{i}\{\sigma_{i}\})^{1/\rho}\right)\right)^{\rho}
$$
EMD 取 $\rho = 7$。

EDM 的 ODE Solver 的算法：

![](images/edm-ode.png)

EDM 的 SDE Solver 的算法：

![](images/edm-sde.png)

![](images/edm.png)

## Derivation

数据分布 $\mathbb{P}_{\text{data}}(\mathbf{x})$，其标准差为 $\sigma_{\text{data}}$。用一系列标准差不同的高斯分布给数据分布加噪，得到加噪后的分布为：
$$
\mathbb{P}(\mathbf{x}; \sigma) = \mathcal{N}\left(\mathbf{0}, \sigma^{2} \mathbf{I}\right) * \mathbb{P}_{\text{data}} = \int_{\mathbb{R}^{d}} \mathcal{N}\left(\mathbf{x}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I}\right) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}
$$
其中 $\sigma \in \{\sigma_{i}\}_{i=1}^{n}$。当 $\max_{i}\{\sigma_{i}\} \gg \sigma_{\text{data}}$ 时，$\mathbb{P}\left(\mathbf{x}; \max_{i}\{\sigma_{i}\}\right)$ 就很接近于纯高斯分布。

Diffusion 的采样过程就是从 $\mathcal{N}\left(\mathbf{0}, \max_{i}\{\sigma_{i}\}^{2} \mathbf{I}\right)$ 中获得一个采样 $\mathbf{x}_{0}$，然后将其逐步去噪，获得一系列具有 $\max_{i}\{\sigma_{i}\} = \sigma_{0} > \sigma_{1} > \cdots > \sigma_{T} = 0$ 噪声水平的样本 $\mathbf{x}_{i}$。

### ODE Formulation

从加噪时的操作开始：
$$
\mathbf{x}_{t} \sim \mathcal{N}\left(\mathbf{x}_{t}; s(t)\mathbf{x}_{0}, s(t)^2 \sigma(t)^2 \mathbf{I}\right)
$$
前向 SDE 为：
$$
\mathrm{d}\mathbf{x}_{t} = \frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} \mathrm{d}t + s(t) \sqrt{2\dot{\sigma}(t)\sigma(t)} \mathrm{d}\mathbf{w}_{t}
$$
反向 SDE 为：
$$
\mathrm{d}\mathbf{x}_{t} = \left[\frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} - 2 s(t)^{2}\dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log p(\mathbf{x}_{t})\right] \mathrm{d}t + s(t)\sqrt{2\dot{\sigma}(t)\sigma(t)} \mathrm{d}\mathbf{w}_{t}
$$
其中 $p(\mathbf{x}_{t})$ 是 $\mathbf{x}_{t}$ 的边际概率分布。对应的 ODE 为：
$$
\mathrm{d}\mathbf{x}_{t} = \left[\frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} - s(t)^{2}\dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log p(\mathbf{x}_{t})\right] \mathrm{d}t
$$

$p(\mathbf{x}_{t})$ 与 $\mathbb{P}(\mathbf{x}; \sigma)$ 的关系为：
$$
\begin{aligned}
p(\mathbf{x}_{t}) &= \int_{\mathbb{R}^{d}} \mathcal{N}\left(\mathbf{x}_{t}; s(t)\mathbf{x}_{0}, s(t)^{2} \sigma(t)^{2} \mathbf{I}\right) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0} \\
&= \int_{\mathbb{R}^{d}} s(t)^{-d} \mathcal{N}\left(s(t)^{-1}\mathbf{x}_{t}; \mathbf{x}_{0}, \sigma(t)^{2} \mathbf{I}\right) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0} \\
&= s(t)^{-d} \left[\mathcal{N}\left(\mathbf{0}, \sigma(t)^{2} \mathbf{I}\right) * \mathbb{P}_{\text{data}}\right](s(t)^{-1}\mathbf{x}_{t}) \\
&= s(t)^{-d} \mathbb{P}(s(t)^{-1}\mathbf{x}_{t}; \sigma(t))
\end{aligned}
$$
带入到概率流 ODE 中：
$$
\begin{aligned}
\mathrm{d} \mathbf{x}_{t} &= \left[\frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} - s(t)^{2}\dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log \left\{ \frac{1}{s(t)^{d}} \mathbb{P}(s(t)^{-1}\mathbf{x}_{t}; \sigma(t)) \right\}\right] \mathrm{d}t \\
&=\left[ \frac{\dot{s}(t)}{s(t)}\mathbf{x}_{t} - s(t)^{2}\dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log \mathbb{P}\left(\frac{\mathbf{x}_{t}}{s(t)}; \sigma(t)\right) \right] \mathrm{d}t \\
&=\left[ \frac{\dot{s}(t)}{s(t)}\mathbf{x}_{t} - s(t)\dot{\sigma}(t)\sigma(t) \nabla_{s(t)^{-1}\mathbf{x}_{t}} \log \mathbb{P}(s(t)^{-1}\mathbf{x}_{t}; \sigma(t)) \right] \mathrm{d}t
\end{aligned}
$$
其中：
$$
\begin{aligned}
\nabla_{\mathbf{x}_{t}} \log \mathbb{P}\left(\frac{\mathbf{x}_{t}}{s(t)}; \sigma(t)\right) &= \frac{\mathrm{d} }{\mathrm{d} \mathbf{x}_{t}} \log \mathbb{P}\left(\frac{\mathbf{x}_{t}}{s(t)}; \sigma(t)\right) \\
&= \frac{\mathrm{d} }{\mathrm{d} \mathbf{u}_{t}} \log \mathbb{P}\left(\mathbf{u}_{t}; \sigma(t)\right) \frac{\mathrm{d} \mathbf{u}_{t}}{\mathrm{d} \mathbf{x}_{t}} \\
&= s(t)^{-1} \nabla_{s(t)^{-1}\mathbf{x}_{t}} \log \mathbb{P}\left(s(t)^{-1}\mathbf{x}_{t}; \sigma(t)\right)
\end{aligned}
$$

取 $s(t) = 1$，则有：
$$
\mathrm{d} \mathbf{x}_{t} = - \dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log \mathbb{P}(\mathbf{x}_{t}; \sigma(t)) \mathrm{d}t
$$

### Denoising Score Matching

设 $D_{\theta}(\mathbf{x}; \sigma)$ 训练的目标是原始数据，即它最小化了任意加噪后的数据与原始数据的 L2 loss：
$$
\begin{aligned}
\mathcal{L} &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\boldsymbol{\epsilon} \sim \mathcal{N}(\mathbf{0}, \sigma^{2} \mathbf{I})}\|D_{\theta}(\mathbf{y}; \sigma) - \mathbf{x}_{0}\|_{2}^{2} \\
&= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\mathbf{y} \sim \mathcal{N}(\mathbf{x}_{0}, \sigma^{2} \mathbf{I})}\|D_{\theta}(\mathbf{y}; \sigma) - \mathbf{x}_{0}\|_{2}^{2}
\end{aligned}
$$
其中 $\mathbf{y} = \mathbf{x}_{0} + \boldsymbol{\epsilon}$。要让 $\mathcal{L}$ 最小，则求 $\mathcal{L}$ 关于 $D_{\theta}$ 变分的零点：
$$
\begin{aligned}
0 &= \delta_{D_{\theta}}\mathcal{L} \\
0 &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\mathbf{y} \sim \mathcal{N}(\mathbf{x}_{0}, \sigma^{2} \mathbf{I})} \delta_{D_{\theta}}\|D_{\theta}(\mathbf{y}; \sigma) - \mathbf{x}_{0}\|_{2}^{2} \\
0 &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\mathbf{y} \sim \mathcal{N}(\mathbf{x}_{0}, \sigma^{2} \mathbf{I})} \left[D_{\theta}(\mathbf{y}; \sigma) - \mathbf{x}_{0}\right] \\
0 &= \mathbb{E}_{\mathbf{y} \sim \mathcal{N}(\mathbf{x}_{0}, \sigma^{2} \mathbf{I})} \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[D_{\theta}(\mathbf{y}; \sigma) - \mathbf{x}_{0}\right] \\
0 &= \int_{\mathbb{R}^{d}} \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[\mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I}) \left(D_{\theta}(\mathbf{y}; \sigma) - \mathbf{x}_{0}\right)\right] \mathrm{d}\mathbf{y} \\
0 &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[\mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I}) \left(D_{\theta}(\mathbf{y}; \sigma) - \mathbf{x}_{0}\right)\right] \\
D_{\theta}(\mathbf{y}; \sigma) &= \frac{\mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[\mathbf{x}_{0} \mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I})\right]}{\mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[\mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I})\right]} = \frac{\int_{\mathbb{R}^{d}} \mathbf{x}_{0} \mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}}{\int_{\mathbb{R}^{d}} \mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}} \\
&= \frac{\int_{\mathbb{R}^{d}} \mathbf{y} \mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0} - \int_{\mathbb{R}^{d}} (\mathbf{y} - \mathbf{x}_{0}) \mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}}{\mathbb{P}(\mathbf{y}; \sigma)} \\
&= \mathbf{y} + \frac{\int_{\mathbb{R}^{d}} \sigma^{2}[\nabla_{\mathbf{y}} \mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I})] \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}}{\mathbb{P}(\mathbf{y}; \sigma)} \\
&= \mathbf{y} + \frac{\sigma^{2} \nabla_{\mathbf{y}} \int_{\mathbb{R}^{d}} \mathcal{N}(\mathbf{y}; \mathbf{x}_{0}, \sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}}{\mathbb{P}(\mathbf{y}; \sigma)} \\
&= \mathbf{y} + \sigma^{2} \nabla_{\mathbf{y}} \log \mathbb{P}(\mathbf{y}; \sigma)
\end{aligned}
$$
因此，$\nabla_{\mathbf{y}} \log \mathbb{P}(\mathbf{y}; \sigma)$ 和 $D_{\theta}(\mathbf{y}; \sigma)$ 的关系为：
$$
\nabla_{\mathbf{y}} \log \mathbb{P}(\mathbf{y}; \sigma) = \frac{D_{\theta}(\mathbf{y}; \sigma) - \mathbf{y}}{\sigma^{2}}
$$

### Solution by Discretization

将用神经网络表达的分数带入到 ODE 中，得到：
$$
\begin{aligned}
\mathrm{d} \mathbf{x}_{t} &= \left[\frac{\dot{s}(t)}{s(t)}\mathbf{x}_{t} - s(t)\dot{\sigma}(t)\sigma(t) \nabla_{s(t)^{-1}\mathbf{x}_{t}} \log \mathbb{P}\left(s(t)^{-1}\mathbf{x}_{t}; \sigma(t)\right)\right] \mathrm{d}t \\
&= \left[ \frac{\dot{s}(t)}{s(t)}\mathbf{x}_{t} - \frac{s(t)\dot{\sigma}(t)}{\sigma(t)} \left(D_{\theta}\left(\frac{\mathbf{x}_{t}}{s(t)}; \sigma(t)\right) - \frac{\mathbf{x}_{t}}{s(t)}\right)\right] \mathrm{d}t \\
&= \left[ \left(\frac{\dot{s}(t)}{s(t)} + \frac{\dot{\sigma}(t)}{\sigma(t)}\right)\mathbf{x}_{t} - \frac{s(t)\dot{\sigma}(t)}{\sigma(t)}D_{\theta}\left(\frac{\mathbf{x}_{t}}{s(t)}; \sigma(t)\right)\right] \mathrm{d}t
\end{aligned}
$$
可以通过数值求解，如 Euler 法或 Runge-Kutta 等方法，EDM 采用的是 2 阶 Heun 方法，即梯形方法。见最前面的伪代码。

### Improvements to Deterministic Sampling

对 ODE 做离散求积，其截断误差和 $\mathrm{d}\mathbf{x}_{t}/\mathrm{d}t$ 的曲率成正比。EDM 认为，在噪声水平较低和较高的情况下，$\mathrm{d}\mathbf{x}_{t}/\mathrm{d}t$ 的曲率比较小，可以用较大的步长。而在中间的情况下，$\mathrm{d}\mathbf{x}_{t}/\mathrm{d}t$ 的曲率比较大，需要用较小的步长。

### Stochastic Sampling

考虑一个描述从 $\mathbb{P}_{\text{data}}$ 演化到 $\mathbb{P}(\mathbf{x}_{t}; \sigma(t))$ 的 Heat Equation：
$$
\frac{\partial q(\mathbf{x}_{t})}{\partial t} = \kappa(t)\Delta_{\mathbf{x}_t} q(\mathbf{x}_{t})
$$
其中 $\kappa(t)$ 是扩散系数。我们的目标是让 $q(\mathbf{x}_{0}) = \mathbb{P}_{\text{data}}(\mathbf{x}_{0})$ 以及 $q(\mathbf{x}_{t}) = \mathbb{P}(\mathbf{x}_{t}; \sigma(t))$。为了获取 $\kappa(t)$，我们可以用 Fourier 变换：
$$
\frac{\partial \hat{q}(\boldsymbol{\omega}, t)}{\partial t} = -\kappa(t)\|\boldsymbol{\omega}\|^{2}\hat{q}(\boldsymbol{\omega}, t)
$$
其中：
$$
\begin{aligned}
\hat{q}(\boldsymbol{\omega}, t) &= \mathcal{F}\{q(\mathbf{x}_{t})\} \\
&= \mathcal{F}\{\mathbb{P}(\mathbf{x}_{t}; \sigma(t))\} \\
&= \mathcal{F}\left\{\mathcal{N}\left(\mathbf{x}_{t}; \mathbf{0}, \sigma(t)^{2}\mathbf{I}\right) * \mathbb{P}_{\text{data}}\right\} \\
&= \exp\left(-\frac{1}{2}\sigma(t)^{2}\|\boldsymbol{\omega}\|^{2}\right)\hat{\mathbb{P}}_{\text{data}}(\boldsymbol{\omega})
\end{aligned}
$$
对 $\hat{q}(\boldsymbol{\omega}, t)$ 求关于 $t$ 的偏导数：
$$
\begin{aligned}
\frac{\partial \hat{q}(\boldsymbol{\omega}, t)}{\partial t} &= -\dot{\sigma}(t)\sigma(t)\|\boldsymbol{\omega}\|^{2}\exp\left(-\frac{1}{2}s(t)^{2}\sigma(t)^{2}\|\boldsymbol{\omega}\|^{2}\right)\hat{\mathbb{P}}_{\text{data}}(\boldsymbol{\omega}) \\
&= -\dot{\sigma}(t)\sigma(t) \|\boldsymbol{\omega}\|^{2}\ \hat{q}(\boldsymbol{\omega}, t)
\end{aligned}
$$
比较两式，得到：
$$
\kappa(t) = \dot{\sigma}(t)\sigma(t)
$$
因此我们所需要的 Heat Equation 为：
$$
\frac{\partial q(\mathbf{x}_{t})}{\partial t} = \dot{\sigma}(t)\sigma(t) \Delta_{\mathbf{x}} q(\mathbf{x}_{t})
$$

与前向 SDE 的 Fokker-Planck 方程对比：
$$
\frac{\partial q(\mathbf{x}_{t})}{\partial t} = -\nabla_{\mathbf{x}_{t}} \cdot \left[f(t)\mathbf{x}_{t} q(\mathbf{x}_{t})\right] + \frac{1}{2} \Delta_{\mathbf{x}_{t}} \left[g(t)^{2} q(\mathbf{x}_{t})\right]
$$
可以得到：
$$
f(t)\mathbf{x}_{t} = \left(\frac{1}{2} g(t)^{2} - \dot{\sigma}(t)\sigma(t) \right) \nabla_{\mathbf{x}_{t}} \log q(\mathbf{x}_{t})
$$
带回到前向 SDE 中，有：
$$
\mathrm{d}\mathbf{x} = \left(\frac{1}{2} g(t)^{2} - \dot{\sigma}(t)\sigma(t) \right) \nabla_{\mathbf{x}} \log \mathbb{P}(\mathbf{x}) \mathrm{d}t + g(t) \mathrm{d}\mathbf{w}_{t}
$$
令 $g(t) = \sqrt{2\beta(t)}\sigma(t)$，则有：
$$
\mathrm{d}\mathbf{x}_{\pm} = \left(\pm\beta(t) - \dot{\sigma}(t)\sigma(t) \right) \nabla_{\mathbf{x}} \log \mathbb{P}(\mathbf{x}) \mathrm{d}t + \sqrt{2\beta(t)}\sigma(t) \mathrm{d}\mathbf{w}_{t}
$$
于是 $\beta(t)$ 就是控制采样时候噪声注入的超参数。因此，SDE 求解器就是在 ODE 求解器的基础上加上了噪声项，见最前面的伪代码。

### Preconditioning and Training

如果直接用网络表达分数，那数值就会特别大，不利于网络训练。因此 EDM 将网络改写为：
$$
D_{\theta}(\mathbf{x}; \sigma) = c_{\text{skip}}(\sigma)\mathbf{x} + c_{\text{out}}(\sigma) F_{\theta}(c_{\text{in}}(\sigma)\mathbf{x}); c_{\text{noise}}(\sigma)
$$
其中 $F_{\theta}$ 是网络，$c_{\text{skip}}$ 是跳跃连接，$c_{\text{in}}, c_{\text{out}}$ 是输入和输出的缩放，$c_{\text{noise}}$ 是噪声注入。这样就可以避免数值过大的问题。


### Variance Perserving Formulation

VP (DDPM) SDE 是：
$$
\mathrm{d}\mathbf{x} = -\frac{1}{2}\left(\beta_{\min} + t(\beta_{\max} - \beta_{\min})\right)\mathbf{x} \mathrm{d}t + \sqrt{\beta_{\min} + t(\beta_{\max} - \beta_{\min})} \mathrm{d}\mathbf{w}_{t}
$$
一般选择 $\beta_{\min} = 0.1$，$\beta_{\max} = 20$。定义 $\beta(t) = \beta_{\min} + t(\beta_{\max} - \beta_{\min})$，则有：
$$
\begin{aligned}
s(t) &= \exp\left(-\frac{1}{2}\int_{0}^{t} \beta(u) \mathrm{d}u\right) \\
\sigma(t) &= \sqrt{\int_{0}^{t} \frac{\beta(u)}{s(u)^2} \mathrm{d}u} \\
&= \sqrt{\int_{0}^{t} \beta(u)\exp\left(\int_{0}^{u} \beta(v) \mathrm{d}v\right)} \mathrm{d}u \\
&= \sqrt{\int_{0}^{t} \frac{\mathrm{d}}{\mathrm{d}u}\exp\left(\int_{0}^{u} \beta(v) \mathrm{d}v\right) \mathrm{d}u} \\
&= \sqrt{\exp\left(\int_{0}^{t} \beta(u) \mathrm{d}u\right) - 1}
\end{aligned}
$$
其中：
$$
\begin{aligned}
\int_{0}^{t} \beta(u) \mathrm{d}u &= \int_{0}^{t} \left(\beta_{\min} + u(\beta_{\max} - \beta_{\min})\right) \mathrm{d}u \\
&= \beta_{\min}t + \frac{1}{2}t^{2}(\beta_{\max} - \beta_{\min})
\end{aligned}
$$
此外还有：
$$
s(t) = \frac{1}{\sqrt{\sigma(t)^2 + 1}}
$$

### Variance Exploding Formulation

VE (NCSN) SDE 是：
$$
\mathrm{d}\mathbf{x} = \sigma_{\min}\left(\frac{\sigma_{\max}}{\sigma_{\min}}\right)^{t} \sqrt{2\log\frac{\sigma_{\max}}{\sigma_{\min}}} \mathrm{d}\mathbf{w}_{t}
$$
则有：
$$
\begin{aligned}
s(t) &= \exp(0) = 1 \\
\sigma(t) &= \sqrt{\sigma_{\min}^{2} 2\log\frac{\sigma_{\max}}{\sigma_{\min}} \int_{0}^{t} \left(\frac{\sigma_{\max}}{\sigma_{\min}}\right)^{2u} \mathrm{d}u} \\
&= \sigma_{\min} \sqrt{2\log\frac{\sigma_{\max}}{\sigma_{\min}} \frac{1}{2\log\frac{\sigma_{\max}}{\sigma_{\min}}} \left(\left(\frac{\sigma_{\max}}{\sigma_{\min}}\right)^{2t} - 1\right)} \\
&= \sqrt{\left(\frac{\sigma_{\max}}{\sigma_{\min}}\right)^{2t} - 1}
\end{aligned}
$$
