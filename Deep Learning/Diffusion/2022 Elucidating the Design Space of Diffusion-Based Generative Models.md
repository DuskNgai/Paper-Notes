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

EDM 的 SDE Solver 的算法：Euler-Maruyama 方法是先执行 ODE Solver，然后再加上噪声项。而 EDM 的算法是在取样点上加上噪声项，然后再执行 ODE Solver。

![](images/edm-sde.png)

![](images/edm.png)

## Derivation

数据分布 $\mathbb{P}_{\text{data}}(\mathbf{x})$，其标准差为 $\sigma_{\text{data}}$。用一系列标准差不同的高斯分布给数据分布加噪，然后再对加噪后的数据缩放，得到的分布为：
$$
\mathbb{P}(\mathbf{x}; s, \sigma) = \int_{\mathbb{R}^{d}} \mathcal{N}\left(\mathbf{x}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}\right) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}
$$
其中 $s \in \{s_{i}\}_{i=1}^{n}$, $\sigma \in \{\sigma_{i}\}_{i=1}^{n}$。当 $\max_{i}\{s_{i}\sigma_{i}\} \gg \sigma_{\text{data}}$ 时，$\mathbb{P}\left(\mathbf{x}; s_{\arg\max_{i}\{s_{i}\sigma_{i}\}}, \sigma_{\arg\max_{i}\{s_{i}\sigma_{i}\}}\right)$ 就很接近于纯高斯分布。

Diffusion 的采样过程就是从 $\mathcal{N}\left(\mathbf{0}, \max_{i}\{s_{i}\sigma_{i}\}^{2} \mathbf{I}\right)$ 中获得一个采样 $\mathbf{x}_{T}$，然后将其逐步去噪，得到最后的采样 $\mathbf{x}_{0}$。这个过程可以用 ODE 或 SDE 来描述。

### Formulation

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
\mathrm{d}\mathbf{x}_{t} = \left[\frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} - 2 s(t)^{2}\dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log p_{t}(\mathbf{x}_{t})\right] \mathrm{d}t + s(t)\sqrt{2\dot{\sigma}(t)\sigma(t)} \mathrm{d}\mathbf{w}_{t}
$$
其中 $p_{t}(\mathbf{x}_{t})$ 是 $t$ 时刻 $\mathbf{x}_{t}$ 的边际概率分布。根据定义，$p_{t}(\mathbf{x}_{t}) = \mathbb{P}(\mathbf{x}_{t}; s(t), \sigma(t))$。

反向 SDE 对应的概率流 ODE 为：
$$
\mathrm{d}\mathbf{x}_{t} = \left[\frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} - s(t)^{2}\dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log p_{t}(\mathbf{x}_{t})\right] \mathrm{d}t
$$
SDE 族为：
$$
\mathrm{d}\mathbf{x}_{t} = \left[\frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} - [s(t)^{2}\dot{\sigma}(t)\sigma(t) + s(t)^{2}\xi(t)\sigma(t)] \nabla_{\mathbf{x}_{t}} \log p_{t}(\mathbf{x}_{t})\right] \mathrm{d}t + s(t)\sqrt{2\xi(t)\sigma(t)} \mathrm{d}\mathbf{w}_{t}
$$
其中 $\xi(t)^{2} < \dot{\sigma}(t)^{2}$ 是控制噪声程度的函数。

### Denoising Score Matching

设 $x_{\theta}:\mathbb{R}^{d} \times \mathbb{R} \times \mathbb{R} \mapsto \mathbb{R}^{d}$ 是去噪神经网络，训练的目标是恢复原始数据，即它最小化了任意加噪后的数据与原始数据的 L2 loss：
$$
\mathcal{L} = \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\boldsymbol{\epsilon} \sim \mathcal{N}(\mathbf{0}, \mathbf{I})}\|x_{\theta}(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}; s, \sigma) - \mathbf{x}_{0}\|_{2}^{2}
$$
设 $\mathbf{y} = s\mathbf{x}_{0} + s\sigma \boldsymbol{\epsilon}$，$\boldsymbol{\epsilon} \sim \mathcal{N}(\mathbf{0}, \mathbf{I})$。要让 $\mathcal{L}$ 最小，则求 $\mathcal{L}$ 关于 $x_{\theta}$ 变分的零点：
$$
\begin{aligned}
0 &= \delta_{x_{\theta}}\mathcal{L} \\
0 &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\boldsymbol{\epsilon} \sim \mathcal{N}(\mathbf{0}, \mathbf{I})} \delta_{x_{\theta}} \|x_{\theta}(\mathbf{y}; s, \sigma) - \mathbf{x}_{0}\|_{2}^{2}\\
0 &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\mathbf{y} \sim \mathcal{N}(s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I})} \delta_{x_{\theta}} \|x_{\theta}(\mathbf{y}; s, \sigma) - \mathbf{x}_{0}\|_{2}^{2} \\
0 &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\mathbf{y} \sim \mathcal{N}(s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I})} \left[x_{\theta}(\mathbf{y}; s, \sigma) - \mathbf{x}_{0}\right] \\
0 &= \mathbb{E}_{\mathbf{y} \sim \mathcal{N}(s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I})} \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[x_{\theta}(\mathbf{y}; s, \sigma) - \mathbf{x}_{0}\right] \\
0 &= \int_{\mathbb{R}^{d}} \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[\mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \left(x_{\theta}(\mathbf{y}; s, \sigma) - \mathbf{x}_{0}\right)\right] \mathrm{d}\mathbf{y} \\
0 &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[\mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \left(x_{\theta}(\mathbf{y}; s, \sigma) - \mathbf{x}_{0}\right)\right]
\end{aligned}
$$
可以解得：
$$
\begin{aligned}
x_{\theta}(\mathbf{y}; s, \sigma) &= \frac{\mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[\mathbf{x}_{0} \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I})\right]}{\mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \left[\mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I})\right]} \\
&= \frac{\int_{\mathbb{R}^{d}} \mathbf{x}_{0} \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}}{\int_{\mathbb{R}^{d}} \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}} \\
&= s^{-1}\mathbf{y} + \frac{\int_{\mathbb{R}^{d}} (s\mathbf{x}_{0} - \mathbf{y}) \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}}{s \int_{\mathbb{R}^{d}} \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}} \\
&= s^{-1}\mathbf{y} + \frac{\int_{\mathbb{R}^{d}} s^{2}\sigma^{2}[\nabla_{\mathbf{y}} \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I})] \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}}{s \int_{\mathbb{R}^{d}} \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}} \\
&= s^{-1}\mathbf{y} + \frac{s\sigma^{2} \nabla_{\mathbf{y}} \int_{\mathbb{R}^{d}} \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}}{\int_{\mathbb{R}^{d}} \mathcal{N}(\mathbf{y}; s\mathbf{x}_{0}, s^{2}\sigma^{2} \mathbf{I}) \mathbb{P}_{\text{data}}(\mathbf{x}_{0}) \mathrm{d}\mathbf{x}_{0}} \\
&= s^{-1}\mathbf{y} + s\sigma^{2} \frac{\nabla_{\mathbf{y}} p_{t}(\mathbf{y})}{p_{t}(\mathbf{y})} \\
&= s^{-1}\mathbf{y} + s\sigma^{2} \nabla_{\mathbf{y}} \log p_{t}(\mathbf{y})
\end{aligned}
$$
因此，$\nabla_{\mathbf{y}} \log p_{t}(\mathbf{y})$ 和 $x_{\theta}(\mathbf{y}; s, \sigma)$ 的关系为：
$$
\nabla_{\mathbf{y}} \log p_{t}(\mathbf{y}) = \frac{x_{\theta}(\mathbf{y}; s, \sigma) - s^{-1}\mathbf{y}}{s\sigma^{2}}
$$
定义 $\epsilon_{\theta}(\mathbf{y}; s, \sigma) = \left[s^{-1}\mathbf{y} - x_{\theta}(\mathbf{y}; s, \sigma)\right] / \sigma$，则此时的优化目标是：
$$
\|x_{\theta}(\mathbf{y}; s, \sigma) - \mathbf{x}_{0}\|_{2}^{2} = \|x_{\theta}(\mathbf{y}; s, \sigma) - s^{-1}\mathbf{y} + \sigma \boldsymbol{\epsilon}\|_{2}^{2} = \sigma^{2} \|\epsilon_{\theta}(\mathbf{y}; s, \sigma)\|_{2}^{2}
$$
可见 $\epsilon_{\theta}(\mathbf{y}; s, \sigma)$ 同样最小化了 $\mathcal{L}$，且 $\nabla_{\mathbf{y}} \log p_{t}(\mathbf{y}; \sigma)$ 和 $\epsilon_{\theta}(\mathbf{y}; s, \sigma)$ 的关系为：
$$
\nabla_{\mathbf{y}} \log p_{t}(\mathbf{y}; \sigma) = -\frac{\epsilon_{\theta}(\mathbf{y}; s, \sigma)}{s\sigma}
$$

### Time-dependent Signal Scaling

将用神经网络表达的分数带入到 ODE 中，得到：
$$
\begin{aligned}
\mathrm{d}\mathbf{x}_{t} &= \left[\frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} - s(t)^{2}\dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log p_{t}(\mathbf{x}_{t})\right] \mathrm{d}t \\
&= \left[ \left(\frac{\dot{s}(t)}{s(t)} + \frac{\dot{\sigma}(t)}{\sigma(t)}\right)\mathbf{x}_{t} - \frac{s(t)\dot{\sigma}(t)}{\sigma(t)}x_{\theta}(\mathbf{x}_{t}; s(t), \sigma(t))\right] \mathrm{d}t \\
&= \left[ \frac{\dot{s}(t)}{s(t)}\mathbf{x}_{t} - s(t)\dot{\sigma}(t)\epsilon_{\theta}(\mathbf{x}_{t}; s(t), \sigma(t))\right] \mathrm{d}t
\end{aligned}
$$
可以通过数值求解，如 Euler 法或 Runge-Kutta 等方法，EDM 采用的是 2 阶 Heun 方法，即梯形方法。见最前面的伪代码。

将用神经网络表达的分数带入到 SDE 中，得到：
$$
\begin{aligned}
\mathrm{d}\mathbf{x}_{t} &= \left[\frac{\dot{s}(t)}{s(t)} \mathbf{x}_{t} - 2 s(t)^{2}\dot{\sigma}(t)\sigma(t) \nabla_{\mathbf{x}_{t}} \log p_{t}(\mathbf{x}_{t})\right] \mathrm{d}t + s(t)\sqrt{2\dot{\sigma}(t)\sigma(t)} \mathrm{d}\mathbf{w}_{t} \\
&= \left[ \left(\frac{\dot{s}(t)}{s(t)} + 2 \frac{\dot{\sigma}(t)}{\sigma(t)}\right)\mathbf{x}_{t} - 2 \frac{s(t)\dot{\sigma}(t)}{\sigma(t)}x_{\theta}(\mathbf{x}_{t}; s(t), \sigma(t))\right] \mathrm{d}t + s(t)\sqrt{2\dot{\sigma}(t)\sigma(t)} \mathrm{d}\mathbf{w}_{t} \\
&= \left[ \frac{\dot{s}(t)}{s(t)}\mathbf{x}_{t} + 2 s(t)\dot{\sigma}(t)\epsilon_{\theta}(\mathbf{x}_{t}; s(t), \sigma(t))\right] \mathrm{d}t + s(t)\sqrt{2\dot{\sigma}(t)\sigma(t)} \mathrm{d}\mathbf{w}_{t}
\end{aligned}
$$

### Improvements to Deterministic Sampling

对 ODE 做离散求积，其截断误差和 $\mathrm{d}\mathbf{x}_{t}/\mathrm{d}t$ 的曲率成正比。EDM 认为，在噪声水平较低和较高的情况下，$\mathrm{d}\mathbf{x}_{t}/\mathrm{d}t$ 的曲率比较小，可以用较大的步长。而在中间的情况下，$\mathrm{d}\mathbf{x}_{t}/\mathrm{d}t$ 的曲率比较大，需要用较小的步长。此外，ODE 的累计误差与步数成正比，而每一步产生的误差与步长成超线性关系，因此步数越多，误差越小。

在 RTX3090 上，用 EDM 提供的模型，每秒钟生成 16 张图像。

### Preconditioning and Training

如果直接用网络表达分数，那数值就会特别大，不利于网络训练。因此 EDM 将网络改写为：
$$
x_{\theta}(\mathbf{x}; s, \sigma) = c_{\text{skip}}(\sigma)\mathbf{x} + c_{\text{out}}(\sigma) F_{\theta}(c_{\text{in}}(\sigma)\mathbf{x}; c_{\text{noise}}(\sigma))
$$
其中 $F_{\theta}$ 是网络，$c_{\text{skip}}$ 是跳跃连接，$c_{\text{in}}, c_{\text{out}}$ 是输入和输出的缩放，$c_{\text{noise}}$ 是噪声注入。带入到训练目标中，有：
$$
\begin{aligned}
\mathcal{L} &= \mathbb{E}_{\mathbf{x}_{0} \sim \mathbb{P}_{\text{data}}} \mathbb{E}_{\sigma \sim \mathbb{P}_{\text{train}}} \mathbb{E}_{\boldsymbol{\epsilon} \sim \mathcal{N}(\mathbf{0}, \mathbf{I})} \left[\lambda(\sigma) \|x_{\theta}(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}; s, \sigma) - \mathbf{x}_{0}\|_{2}^{2}\right] \\
&= \mathbb{E}_{\mathbf{x}_{0}, \sigma, \boldsymbol{\epsilon}} \left[\lambda(\sigma) \|c_{\text{skip}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}) + c_{\text{out}}(\sigma) F_{\theta}(c_{\text{in}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}); c_{\text{noise}}(\sigma)) - \mathbf{x}_{0}\|_{2}^{2}\right] \\
&= \mathbb{E}_{\mathbf{x}_{0}, \sigma, \boldsymbol{\epsilon}} \left[\lambda(\sigma) c_{\text{out}}(\sigma)^{2} \left\|F_{\theta}(c_{\text{in}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}); c_{\text{noise}}(\sigma)) - \frac{1}{c_{\text{out}}(\sigma)}(\mathbf{x}_{0} - c_{\text{skip}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}))\right\|_{2}^{2}\right]
\end{aligned}
$$
其中 $\lambda(\sigma)$ 是一项权重项，用于调整不同噪声水平的重要性。在训练时，$\lambda(\sigma) = 1 / c_{\text{out}}(\sigma)^{2}$。$p_{\text{train}}(\sigma)$ 也需要经验性地选择，在中等噪声水平下才能显著降低损失；在极低的噪声水平下，很难辨别出极小的噪声成分；而在高噪声水平下，训练目标总是与接近数据集平均值的正确答案相差甚远。因此，$p_{\text{train}}(\sigma)$ 就经验性地设置为 $\log\sigma \sim \mathcal{U}(P_{\text{mean}}, P_{\text{std}})$。

一般来说，网络的输入满足方差为 1 会比较好，因此：
$$
\begin{aligned}
1 & =\mathbb{Var}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}}[c_{\text{in}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon})] \\
&= c_{\text{in}}(\sigma)^{2} \mathbb{Var}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}}[s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}] \\
&= c_{\text{in}}(\sigma)^{2} (s^{2}\sigma_{\text{data}}^{2} + s^{2}\sigma^{2}) \\
c_{\text{in}}(\sigma) &= \frac{1}{\sqrt{s^{2}\sigma_{\text{data}}^{2} + s^{2}\sigma^{2}}}
\end{aligned}
$$
同时，目标也满足方差为 1 会比较好，因此：
$$
\begin{aligned}
1 & = \mathbb{Var}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}} \left[\frac{1}{c_{\text{out}}(\sigma)}(\mathbf{x}_{0} - c_{\text{skip}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}))\right] \\
&= \frac{1}{c_{\text{out}}(\sigma)^{2}} \mathbb{Var}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}} \left[\mathbf{x}_{0} - c_{\text{skip}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon})\right] \\
c_{\text{out}}(\sigma)^{2} &= \mathbb{Var}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}} \left[(1 - sc_{\text{skip}}(\sigma))\mathbf{x}_{0} - c_{\text{skip}}(\sigma)s\sigma\boldsymbol{\epsilon}\right] \\
&= (1 - sc_{\text{skip}}(\sigma))^{2} \sigma_{\text{data}}^{2} + c_{\text{skip}}(\sigma)^{2} s^{2}\sigma^{2}
\end{aligned}
$$
需要选择能让 $c_{\text{out}}(\sigma)$ 最小的那个 $c_{\text{skip}}(\sigma)$：
$$
\begin{aligned}
0 &= \frac{\partial c_{\text{out}}(\sigma)^{2}}{\partial c_{\text{skip}}(\sigma)} \\
0 &= 2s(1 - sc_{\text{skip}}(\sigma))\sigma_{\text{data}}^{2} - 2c_{\text{skip}}(\sigma)s^{2}\sigma^{2} \\
c_{\text{skip}}(\sigma) &= \frac{\sigma_{\text{data}}^{2}}{s(\sigma_{\text{data}}^{2} + \sigma^{2})}
\end{aligned}
$$
对应的 $c_{\text{out}}(\sigma)$ 为：
$$
\begin{aligned}
c_{\text{out}}(\sigma)^{2} &= (1 - sc_{\text{skip}}(\sigma))^{2} \sigma_{\text{data}}^{2} + c_{\text{skip}}(\sigma)^{2} s^{2}\sigma^{2} \\
&= \left(1 - \frac{\sigma_{\text{data}}^{2}}{\sigma_{\text{data}}^{2} + \sigma^{2}}\right)^{2} \sigma_{\text{data}}^{2} + \left(\frac{\sigma_{\text{data}}^{2}}{\sigma_{\text{data}}^{2} + \sigma^{2}}\right)^{2} \sigma^{2} \\
&= \frac{(\sigma^{4} \sigma_{\text{data}}^{2} + \sigma^{2} \sigma_{\text{data}}^{4})}{(\sigma^{2} + \sigma_{\text{data}}^{2})^{2}} \\
c_{\text{out}}(\sigma) &= \frac{\sigma \sigma_{\text{data}}}{\sqrt{\sigma^{2} + \sigma_{\text{data}}^{2}}}
\end{aligned}
$$
此时：
$$
\lambda(\sigma) = \frac{1}{c_{\text{out}}(\sigma)^{2}} = \frac{\sigma^{2} + \sigma_{\text{data}}^{2}}{(\sigma \sigma_{\text{data}})^{2}}
$$
假如我们零初始化网络 $F_{\theta}$，那么在训练初始阶段，对于某个固定的 $\sigma$，有：
$$
\begin{aligned}
&\quad\ \ \mathbb{E}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}} \left[\lambda(\sigma) c_{\text{out}}(\sigma)^{2} \left\|F_{\theta}(c_{\text{in}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}); c_{\text{noise}}(\sigma)) - \frac{1}{c_{\text{out}}(\sigma)}(\mathbf{x}_{0} - c_{\text{skip}}(\sigma)(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon}))\right\|_{2}^{2}\right] \\
&= \mathbb{E}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}} \left[\frac{\sigma^{2} + \sigma_{\text{data}}^{2}}{(\sigma \sigma_{\text{data}})^{2}} \left\|\mathbf{x}_{0} - \frac{\sigma_{\text{data}}^{2}}{s(\sigma_{\text{data}}^{2} + \sigma^{2})}(s\mathbf{x}_{0} + s\sigma\boldsymbol{\epsilon})\right\|_{2}^{2}\right] \\
&= \mathbb{E}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}} \left[\frac{\sigma^{2} + \sigma_{\text{data}}^{2}}{(\sigma \sigma_{\text{data}})^{2}} \left\|\frac{\sigma^{2}}{\sigma_{\text{data}}^{2} + \sigma^{2}}\mathbf{x}_{0} - \frac{\sigma_{\text{data}}^{2}}{\sigma_{\text{data}}^{2} + \sigma^{2}}\sigma\boldsymbol{\epsilon}\right\|_{2}^{2}\right] \\
&= \mathbb{E}_{\mathbf{x}_{0}, \boldsymbol{\epsilon}} \left[\frac{1}{\sigma^{2} + \sigma_{\text{data}}^{2}} \left\|\frac{\sigma}{\sigma_{\text{data}}}\mathbf{x}_{0} - \sigma_{\text{data}}\boldsymbol{\epsilon}\right\|_{2}^{2}\right] \\
&= \frac{1}{\sigma^{2} + \sigma_{\text{data}}^{2}} \left[\left(\frac{\sigma}{\sigma_{\text{data}}}\right)^{2} \sigma_{\text{data}}^{2} + \sigma_{\text{data}}^{2}\right] \\
&= 1
\end{aligned}
$$

总的来说，这些操作不是为了改善指标，而是使训练更加稳健。

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
此外 VP SDE 满足：
$$
s(t)\sqrt{\sigma(t)^2 + 1} = 1
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
