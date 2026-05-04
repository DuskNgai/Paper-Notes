# Chapter 9 Policy Gradient Methods

在状态空间较大或连续时，无法使用表格存储策略，需使用参数化函数 $\pi(a \mid s; \theta)$ 来近似策略 $\pi(a \mid s)$。对于离散动作空间，神经网络的最后一层通常是 softmax 层；对于连续动作空间，通常输出高斯分布的均值和方差。

$$
\nabla_{\theta} \pi(a \mid s; \theta) = \pi(a \mid s; \theta) \nabla_{\theta} \log \pi(a \mid s; \theta)
$$
由于 $\pi$ 是一个概率分布，$\nabla_{\theta} \log \pi(a \mid s; \theta)$ 被称为 **score function**。

以 softmax 输出 $\pi(a \mid s; \theta) \propto \exp(h(s, a; \theta))$ 为例（其中 $h$ 为网络输出的动作偏好 logits）：
$$
\begin{aligned} \nabla_{\theta} \log \pi(a \mid s; \theta) &= \nabla_{\theta} h(s, a; \theta) - \sum_{a'} \pi(a' \mid s; \theta) \nabla_{\theta} h(s, a'; \theta) \\ &= \nabla_{\theta} h(s, a; \theta) - \mathbb{E}_{A \sim \pi(\cdot \mid s; \theta)} [\nabla_{\theta} h(s, A; \theta)] \end{aligned}
$$

以高斯输出 $\pi(a \mid s; \theta) = \mathcal{N}(a \mid \mu(s; \theta), \sigma^{2}(s; \theta))$ 为例：
$$
\begin{aligned}
\nabla_{\theta} \log \pi(a \mid s; \theta) &= \nabla_{\theta} \left( -\frac{1}{2} \log (2\pi) - \frac{1}{2} \log \sigma^{2}(s; \theta) - \frac{(a - \mu(s; \theta))^{2}}{2\sigma^{2}(s; \theta)} \right) \\
&= \frac{a - \mu(s; \theta)}{\sigma^{2}(s; \theta)} \nabla_{\theta} \mu(s; \theta) + \frac{(a - \mu(s; \theta))^{2} - \sigma^{2}(s; \theta)}{2\sigma^4(s; \theta)} \nabla_{\theta} \sigma^{2}(s; \theta)
\end{aligned}
$$
在一些简化设定中，如果方差 $\sigma^{2}$ 是常数，则第二项为 0

## Trajectory Perspective (Discrete State and Action Spaces)

给定策略 $\pi_{\theta}$，在环境中生成一条轨迹 $\tau = (s_{0}, a_{0}, r_{1}, \dots, s_{T - 1}, a_{T - 1}, r_T)$。轨迹的概率取决于策略和环境动态：
$$
p_{\pi}(\tau) = p(s_{0}) \prod_{t = 0}^{T - 1} \pi(a_{t} \mid s_{t}; \theta) \cdot p(s_{t+1} \mid s_{t}, a_{t})
$$
其中 $p(s_{0})$ 是初始状态分布，$p(s_{t + 1} \mid s_{t}, a_{t})$ 是环境动态。

我们的目标是最大化轨迹累计折扣奖励的期望：
$$
J(\theta) = \mathbb{E}_{\tau \sim p_{\pi}(\tau)}[R(\tau)], \quad R(\tau) = \sum_{t = 0}^{T - 1} \gamma^t R_{t+1}
$$
其梯度为：
$$
\begin{aligned}
\nabla_{\theta} J(\theta) &= \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ R(\tau) \nabla_{\theta} \log p_{\pi}(\tau) \right] \\
&= \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ \left( \sum_{t = 0}^{T - 1} \gamma^t R_{t+1} \right) \left( \sum_{t = 0}^{T - 1} \nabla_{\theta} \log \pi(A_{t} \mid S_{t}; \theta) \right) \right]
\end{aligned}
$$
环境动态 $p(s_{t+1} \mid s_{t}, a_{t})$ 与 $\theta$ 无关，求导时已消去。因此，策略梯度是 **model-free** 的。通过采样 $N$ 条轨迹来估计此梯度是无偏的，但具有很高的方差。直观上理解，策略梯度尝试提高累积奖励为正的轨迹出现的概率。

## Variance Reduction

为了降低方差，引入因果关系：在 $t$ 时刻采取的动作 $A_{t}$ 不会影响过去的奖励 $R_{1}, \dots, R_{t}$。因此，未来奖励的累加（Reward-to-go）$G_{t} = \sum_{k = t}^{T - 1} \gamma^{k - t} R_{k + 1}$ 足以评估该动作的好坏。我们可以将求和拆解，去掉与 $A_t$ 无关的过去奖励，仅保留未来奖励的累加：
$$
\begin{aligned}
\nabla_{\theta} J(\theta) &= \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ \sum_{t = 0}^{T - 1} \left( \sum_{k = 0}^{T - 1} \gamma^{k} R_{k + 1} \right) \nabla_{\theta} \log \pi(A_{t} \mid S_{t}; \theta) \right] \\
&= \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ \sum_{t = 0}^{T - 1} \left( \sum_{k = t}^{T - 1} \gamma^{k} R_{k + 1} \right) \nabla_{\theta} \log \pi(A_{t} \mid S_{t}; \theta) \right] \\
&= \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ \sum_{t = 0}^{T - 1} \gamma^{t} \left( \sum_{k = t}^{T - 1} \gamma^{k - t} R_{k + 1} \right) \nabla_{\theta} \log \pi(A_{t} \mid S_{t}; \theta) \right] \\
&= \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ \sum_{t = 0}^{T - 1} \gamma^{t} G_{t} \nabla_{\theta} \log \pi(A_{t} \mid S_{t}; \theta) \right]
\end{aligned}
$$

### REINFORCE Algorithm

**REINFORCE** 是 Monte Carlo Policy Gradient 的经典实现：

- **Infinite Loop**:
    1. **Generate Trajectory**: 使用当前策略 $\pi_{\theta}$ 采样生成一条完整轨迹 $\tau$。
    2. **Update Parameters**: 针对轨迹中的每个时间步 $t = 0, 1, \dots, T - 1$，计算折扣回报 $G_{t}$ 并更新 $\theta$：
        - $G_{t} = \sum_{k = t}^{T - 1} \gamma^{k - t} R_{k + 1}$
        - $\theta \leftarrow \theta + \alpha \gamma^t G_{t} \nabla_{\theta} \log \pi(a_{t} \mid s_{t}; \theta)$

## Variance Reduction with Baseline

通过引入一个不依赖于动作 $A_{t}$ 的**基线 (Baseline)** $b(s)$，可以进一步降低方差：
$$
\nabla_{\theta} J(\theta) = \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ \sum_{t = 0}^{T - 1} \gamma^{t} (G_{t} - b(S_{t})) \nabla_{\theta} \log \pi(A_{t} \mid S_{t}; \theta) \right]
$$

## Appendix

### Policy Gradient Theorem

$$
\begin{aligned}
\nabla_{\theta} J(\theta) &= \nabla_{\theta} \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ R(\tau) \right] \\
&= \nabla_{\theta} \int p_{\pi}(\tau) R(\tau) \mathrm{d}\tau \\
&= \int \nabla_{\theta} p_{\pi}(\tau) R(\tau) \mathrm{d}\tau \\
&= \int p_{\pi}(\tau) \nabla_{\theta} \log p_{\pi}(\tau) R(\tau) \mathrm{d}\tau \\
&= \mathbb{E}_{\tau \sim p_{\pi}(\tau)} \left[ R(\tau) \nabla_{\theta} \log p_{\pi}(\tau) \right]
\end{aligned}
$$

### Policy Gradient Theorem with Baseline

证明引入不依赖于动作的基线 $b(S_{t})$ 不会改变梯度期望（即不引入偏差）。令 $H_{t} = (S_0, A_0, \dots, S_{t})$ 表示历史，$F_{t} = (A_{t}, S_{t+1}, \dots, S_T)$ 表示未来。根据全期望公式：
$$
\begin{aligned}
\mathbb{E}_{\tau}\left[ b(S_{t}) \nabla_\theta \log \pi(A_{t} \mid S_{t}) \right] &= \mathbb{E}_{H_{t}}\left[ \mathbb{E}_{F_{t} \mid H_{t}}\left[ b(S_{t}) \nabla_\theta \log \pi(A_{t} \mid S_{t}) \mid H_{t} \right] \right] \\
&= \mathbb{E}_{H_{t}}\left[ b(S_{t}) \cdot \mathbb{E}_{A_{t} \mid S_{t}}\left[ \nabla_\theta \log \pi(A_{t} \mid S_{t}) \mid S_{t} \right] \right] \\
&= \mathbb{E}_{H_{t}}\left[ b(S_{t}) \cdot \sum_{a} \pi(a \mid S_{t}) \nabla_\theta \log \pi(a \mid S_{t}) \right] \\
&= \mathbb{E}_{H_{t}}\left[ b(S_{t}) \cdot \nabla_\theta \sum_{a} \pi(a \mid S_{t}) \right] \\
&= \mathbb{E}_{H_{t}}\left[ b(S_{t}) \cdot \nabla_\theta 1 \right] \\
&= \mathbb{E}_{H_{t}}[0] = 0.
\end{aligned}
$$
