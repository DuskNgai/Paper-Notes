# Chapter 8 Value Function Approximation

## Value Function Approximation

**Value Function Approximation**：在状态空间较大或连续时，无法使用表格存储值函数，需使用参数化函数 $v(s; \theta)$ 或 $q(s, a; \theta)$ 来近似 $v_{\pi}(s)$ 或 $q_{\pi}(s, a)$。

### Objective Function

我们希望通过调整参数 $\theta$ 最小化近似值与真实值之间的均方误差：
$$
J(\theta) = \frac{1}{2} \mathbb{E}_{\pi} \left[ (v(S; \theta) - v_{\pi}(S))^{2} \right]
$$
由于智能体与环境交互时状态并非均匀出现，我们引入**平稳分布 (Stationary Distribution)** $d_{\pi}(s)$ 进行加权：
$$
J(\theta) = \frac{1}{2} \mathbb{E}_{S \sim d_{\pi}} \left[ (v(S; \theta) - v_{\pi}(S))^{2} \right]
$$
其中 $d_{\pi}(s)$ 满足 $d_{\pi}^{\top} = d_{\pi}^{\top} P_{\pi}$，即状态在策略 $\pi$ 下的访问频率分布。

## Stochastic Gradient Descent (SGD)

梯度下降法的更新规则为：
$$
\theta_{t + 1} \leftarrow \theta_{t} - \alpha \mathbb{E}_{\pi} \left[ (v(S; \theta) - v_{\pi}(S)) \nabla_{\theta} v(S; \theta) \right]
$$
使用一个样本状态 $s$ 来计算梯度：
$$
\theta_{t + 1} \leftarrow \theta_{t} + \alpha (v_{\pi}(s) - v(s; \theta_t)) \nabla_{\theta} v(s; \theta_t)
$$
更新中使用的目标 $v_{\pi}(s)$ 是未知的，我们需要使用**样本回报**来替代：

**Monte Carlo**：使用 Episode 回报 $G_{t}$ 作为目标：
$$
\theta_{t + 1} \leftarrow \theta_{t} - \alpha (v(s_t; \theta_t) - g_{t}) \nabla_{\theta} v(s_t; \theta_t)
$$

**Temporal Difference**：使用 TD 目标 $R_{t + 1} + \gamma v(S_{t + 1}; \theta)$：
$$
\theta_{t + 1} \leftarrow \theta_{t} - \alpha [v(s_t; \theta_t) - (r_{t + 1} + \gamma v(s_{t + 1}; \theta_t))] \nabla_{\theta} v(s_t; \theta_t)
$$
这里的更新被称为**半梯度 (Semi‑Gradient)**，因为目标中的 $v(s_{t + 1}; \theta_t)$ 也依赖于 $\theta$，但我们在计算梯度时将其视为常数。

## Linear Function Approximation

线性逼近是理论分析的基石。设特征向量为 $\mathbf{x}(s)$，状态值函数表示为：
$$
v(s; \theta) = \theta^{\top} \mathbf{x}(s)
$$
其梯度形式简洁：
$$
\nabla_{\theta} v(s; \theta) = \mathbf{x}(s)
$$

## Control with Function Approximation

将函数逼近扩展到动作价值函数 $q(s, a; \theta)$。

### Sarsa with Function Approximation (On‑policy)

$$
\theta_{t + 1} \leftarrow \theta_{t} - \alpha [q(s_{t}, a_{t}; \theta_{t}) - (r_{t + 1} + \gamma q(s_{t + 1}, a_{t + 1}; \theta_{t}))] \nabla_{\theta_{t}} q(s_{t}, a_{t}; \theta_{t})
$$

### Q‑learning with Function Approximation (Off‑policy)

$$
\theta_{t + 1} \leftarrow \theta_{t} - \alpha [q(s_{t}, a_{t}; \theta_{t}) - (r_{t + 1} + \gamma \max_{a'} q(s_{t + 1}, a'; \theta_{t}))] \nabla_{\theta_{t}} q(s_{t}, a_{t}; \theta_{t})
$$

## Deep Q‑Learning (DQN)

深度神经网络作为非线性逼近器时，训练极不稳定。DQN 引入了两个核心机制来打破这种不稳定：

- **Experience Replay**：将转移 $(s, a, r, s')$ 存入回放缓冲池 $\mathcal{D}$，通过随机采样打破数据间的时间相关性。
- **Target Network**：使用参数为 $\theta^{-}$ 的独立网络计算 TD 目标，固定一段时间后再同步（$\theta^{-} \leftarrow \theta$），解决“追逐移动目标”的问题。

**Loss Function**:
$$
J(\theta) = \mathbb{E}_{(S, A, R, S') \sim \mathcal{D}} \left[ (R + \gamma \max_{a'} q(S', a'; \theta^{-}) - q(S, A; \theta))^2 \right]
$$

**Off‑policy DQN**：
1. **Replay Buffer**：基于行为策略 $\mu$ 收集数据，放入 Buffer $\mathcal{B} = \{(s, a, r, s')\}$。
2. **Sampling**：从 $\mathcal{B}$ 中随机采样小批量数据 $(s, a, r, s')$ 进行训练。
3. **Target Network**：使用 $\theta^{-}$ 计算 TD 目标：
$$
y = r + \gamma \max_{a' \in \mathcal{A}(s')} q(s', a'; \theta^{-})
$$
4. **Objective**：最小化均方误差：
$$
J(\theta) = \mathbb{E}_{(s, a, r, s') \sim \mathcal{B}} \left[ (y - q(s, a; \theta))^2 \right]
$$
5. **Synchronization**：固定一段时间后，将 $\theta^{-} \leftarrow \theta$。

## The Deadly Triad

在强化学习中，若同时出现以下三个要素，算法极易出现**发散 (Divergence)** 或不稳定性：

1.  **Function Approximation**：使用参数化函数（尤其是神经网络）处理大规模状态空间。
2.  **Bootstrapping**：使用后续状态的估计值更新当前值（如 TD 学习、$n$-step TD）。
3.  **Off‑policy**：训练数据分布（行为策略）与待优化分布（目标策略）不同（如 Q‑learning）。

### Algorithms with the Deadly Triad

- **MC (On‑policy)** 仅有要素 1，通常是稳定的。
- **Sarsa (Linear)** 包含要素 1 和 2，在线性情况下稳定收敛。
- **Q‑learning** 同时包含三要素。由于半梯度更新不追踪目标的变换，加上 Off‑policy 导致的状态分布偏移，参数可能进入正反馈循环，导致值估计无限增长。

**结论**：DQN 中的 **Target Network** 本质上是为了暂时“切断” **Bootstrapping** 带来的直接耦合，从而在存在 **Off‑policy** 和 **Function Approximation** 的情况下维持训练的稳定性。
