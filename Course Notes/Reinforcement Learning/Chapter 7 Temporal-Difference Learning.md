# Chapter 7 Temporal-Difference Learning

在强化学习中，我们要计算状态价值 $v(s)$ 或动作价值 $q(s, a)$。动态规划（DP）利用已知的环境模型进行精确的期望备份（Expected Backup）；但在 Model-free 环境中，我们不知道状态转移概率，只能通过与环境交互来进行采样备份（Sample Backup）。

Temporal-Difference (TD) 学习的核心思想就是：基于单步采样的 Bellman 方程展开，结合自举进行价值更新。

## TD Learning for State Value (TD(0))

State Value 的 Bellman Expectation Equation 是：
$$
v_{\pi}(s) = \mathbb{E}_{\pi}[R_{t + 1} + \gamma v_{\pi}(S_{t + 1}) \mid S_{t} = s]
$$

为了求解这个方程，不同的算法采用了不同的备份策略：

- **Dynamic Programming**：进行全宽度备份 (Full Backup)。需要环境模型已知，计算所有可能下一状态的期望。
- **Monte Carlo**：进行深度采样备份。因为不知道期望，所以让智能体走到回合结束，用真实的累积回报 $g_{t}$ 作为目标：
    $$
    v_{t + 1}(s_{t}) \gets v_{t}(s_{t}) + \alpha_{t} [g_{t} - v_{t}(s_{t})]
    $$
- **Temporal Difference**：结合了 DP 的自举和 MC 的采样。我们只往前走一步（采样），用采样的单步奖励 $r_{t + 1}$ 加上对下一个状态价值的当前估计 $v_{t}(s_{t + 1})$ 来代替真实的期望：
    $$
    v_{t + 1}(s_{t}) \gets v_{t}(s_{t}) + \alpha_{t} \left[ (r_{t + 1} + \gamma v_{t}(s_{t + 1})) - v_{t}(s_{t}) \right]
    $$

**TD Target**：
$$
\bar{v}_{t}(s_{t}) \triangleq r_{t + 1} + \gamma v_{t}(s_{t + 1})
$$
**TD Error**：
$$
\delta_{t} \triangleq \bar{v}_{t}(s_{t}) - v_{t}(s_{t})
$$
TD 学习的名字就来源于这个 TD 误差：$v_{t}(s_{t})$ 是当前的状态值估计，$\bar{v}_{t}(s_{t})$ 是下一个时刻得到的信息。如果 $v_{t} = v_{\pi}$，则：
$$
\mathbb{E}[\delta_{t} \mid s_{t}] = \mathbb{E}[r_{t + 1} + \gamma v_{\pi}(s_{t + 1}) \mid s_{t}] - v_{\pi}(s_{t}) = 0
$$
若 $\delta_{t} \ne 0$，则说明 $v_{t}$ 还没有收敛到 $v_{\pi}$。

## TD Learning for Action Value

在没有环境模型时，优化策略必须估计 $q_{\pi}(s, a)$。Action Value 的 Bellman Expectation Equations 是：
$$
q_{\pi}(s, a) = \mathbb{E}_{\pi}[R_{t + 1} + \gamma q_{\pi}(S_{t + 1}, A_{t + 1}) \mid S_{t} = s, A_{t} = a]
$$

### Sarsa (State-Action-Reward-State-Action)

Sarsa 直接使用当前策略 $\pi$ 产生的单步轨迹 $(s_{t}, a_{t}, r_{t + 1}, s_{t + 1}, a_{t + 1})$ 进行更新。它对状态转移和动作选择都进行了采样备份：
$$
q_{t + 1}(s_{t}, a_{t}) \gets q_{t}(s_{t}, a_{t}) + \alpha_{t} \left[ (r_{t + 1} + \gamma q_{t}(s_{t + 1}, a_{t + 1})) - q_{t}(s_{t}, a_{t}) \right]
$$
它对应的备份目标是：
$$
\mathbb{E}[R_{t + 1} + \gamma q_{\pi}(S_{t + 1}, A_{t + 1}) \mid S_{t} = s_{t}, A_{t} = a_{t}]
$$

### Expected Sarsa

Sarsa 的方差来源于环境转移和策略动作的随机性。Expected Sarsa 通过对动作空间求期望，消除了动作选择的随机性：
$$
q_{t + 1}(s_{t}, a_{t}) \gets q_{t}(s_{t}, a_{t}) + \alpha_{t} \left[ \left(r_{t + 1} + \gamma \sum_{a'} \pi(a' \mid s_{t + 1}) q_{t}(s_{t + 1}, a')\right) - q_{t}(s_{t}, a_{t}) \right]
$$
它对应的备份目标是：
$$
\mathbb{E}[R_{t + 1} + \gamma v_{\pi}(S_{t + 1}) \mid S_{t} = s_{t}, A_{t} = a_{t}]
$$

## Q-Learning

Q-Learning 不再试图近似 Bellman Expectation Equations，而是直接近似 Bellman Optimality Equation。在备份时，它假设在下一状态总是采取最优动作，而不管实际执行的是什么动作：
$$
q_{t + 1}(s_{t}, a_{t}) \gets q_{t}(s_{t}, a_{t}) + \alpha_{t} \left[ (r_{t + 1} + \gamma \max_{a' \in \mathcal{A}(s_{t + 1})} q_{t}(s_{t + 1}, a')) - q_{t}(s_{t}, a_{t}) \right]
$$
它对应的备份目标是：
$$
\mathbb{E}\left[R_{t + 1} + \gamma \max_{a' \in \mathcal{A}(S_{t + 1})} q_{*}(S_{t + 1}, a') \mid S_{t} = s_{t}, A_{t} = a_{t}\right]
$$

- **On-policy**：采样和更新基于同一策略。如 MC、Sarsa。
- **Off-policy**：采样基于行为策略 $\mu$，更新基于目标策略 $\pi$。如 Q-Learning。

**Off-policy Q-Learning**：
1. **Initialization**：初始化 $q_{0}(s, a) \in \mathbb{R}, \forall s, a$。
2. **Infinite Loop**：重复以下步骤：
    1. **Episode Generation**：基于行为策略 $\mu$（例如关于 $q$ 的 $\epsilon$-greedy 策略）生成一个 Episode $s_{0}, a_{0}, r_{1}, \dots, s_{T - 1}, a_{T - 1}, r_{T}$。
    2. **Step-by-step Update**：对于 Episode 中的每一个时间步 $t = 0, 1, \dots, T-1$：
        1. **Value Update**：
           $$
           q_{t + 1}(s_{t}, a_{t}) \leftarrow q_{t}(s_{t}, a_{t}) + \alpha_{t} \left[ r_{t + 1} + \gamma \max_{a \in \mathcal{A}(s_{t + 1})} q_{t}(s_{t + 1}, a) - q_{t}(s_{t}, a_{t}) \right]
           $$
        2. **Policy Improvement**：目标策略 $\pi$ 和行为策略 $\mu$ 可以不一样：
           $$
           \pi(s) = \arg\max_{a \in \mathcal{A}(s)} q(s, a)
           $$

## $n$-step Bootstrapping

- **Sampling**：利用环境真实的反馈来更新。
- **Bootstrapping**：利用对后继状态价值的“已有估计”来更新“当前估计”。

通过展开 Bellman 方程 $n$ 步，我们可以在 TD 和 MC 之间滑动：
- **1-step TD (TD(0))**：$G_{t}^{(0)} = R_{t + 1} + \gamma v_{t}(S_{t + 1})$
- **$n$-step TD**：$G_{t}^{(n - 1)} = R_{t + 1} + \gamma R_{t + 2} + \dots + \gamma^{n - 1} v_{t + n - 1}(S_{t + n})$
- **$\infty$-step TD (MC)**：$G_{t}^{\infty} = R_{t + 1} + \gamma R_{t + 2} + \dots + \gamma^{T - 1} R_{T}$

需要注意的是，TD 利用了 Markov 依赖；MC 不利用 Markov 依赖，适用范围更广。

**Bias-Variance Trade-off**：
- 展开的步数 $n$ 越小（越偏向 TD）：使用的真实采样越少，猜测（自举）的成分越多。**方差小，偏差大**。
- 展开的步数 $n$ 越大（越偏向 MC）：使用的真实采样越多，甚至不需要猜测。**方差大，偏差小**。

### TD($\lambda$) Learning

TD($\lambda$) Learning 相当于给 $n$-step 奖励做了指数平滑：
$$
G_{t}^{\lambda} = (1 - \lambda) \sum_{n = 0}^{\infty} \lambda^{n - 1} G_{t}^{(n)}
$$

## Importance Sampling

重要性采样（Importance Sampling, IS）是一种统计技术，用于从一个分布（**行为分布** $\mu$）中采样，来估计另一个分布（**目标分布** $\pi$）下的期望。在强化学习中，它主要用于 **Off-Policy** 学习，即行为策略 $\mu$（用于与环境交互生成数据）与目标策略 $\pi$（待评估或优化的策略）不同。

假设我们想估计期望 $\mathbb{E}_{X \sim \pi}[f(X)]$，但只能从分布 $\mu$ 中采样 $X_{i}$。引入重要性权重：
$$
\rho_{i} = \frac{\pi(X_{i})}{\mu(X_{i})},
$$
则：
$$
\mathbb{E}_{X \sim \pi}[f(X)] = \mathbb{E}_{X \sim \mu}\left[ \frac{\pi(X)}{\mu(X)} f(X) \right] \approx \frac{1}{n} \sum_{i = 1}^{n} \rho_{i} f(X_{i}).
$$

在强化学习中，轨迹的概率取决于策略和转移概率：
$$
p_{\pi}(\tau) = \prod_{t = 0}^{T - 1} \pi(a_{t} \mid s_{t}) \cdot p(s_{t+1} \mid s_{t}, a_{t}),
$$
其中 $p(s_{t+1} \mid s_{t}, a_{t})$ 是环境动态（未知，但在两个策略下相同）。因此，从 $\mu$ 采样得到的轨迹用于估计 $\pi$ 下的期望时，重要性权重为：
$$
\rho_{0:T - 1} = \frac{p_{\pi}(\tau)}{p_{\mu}(\tau)} = \prod_{t = 0}^{T - 1} \frac{\pi(a_{t} \mid s_{t})}{\mu(a_{t} \mid s_{t})}.
$$

### In MC

**Off-Policy Monte Carlo**：使用行为策略 $\mu$ 生成完整轨迹，计算累积回报 $G_{t}$，然后通过重要性权重修正后更新目标策略 $\pi$ 的价值函数：
$$
v_{\pi}(s_{t}) \leftarrow v_{\pi}(s_{t}) + \alpha \left( \rho_{t:T - 1} G_{t} - v_{\pi}(s_{t}) \right).
$$
**问题**：方差极大，容易产生不稳定更新。

### In TD

在 TD 方法中，我们可以利用**单步重要性权重**来实现 Off-Policy 更新。

- **Off-Policy TD(0)**（不使用重要性采样）是有偏的，因为 TD 目标依赖于后继状态的价值估计，而该估计来自目标策略 $\pi$，但实际转移来自行为策略 $\mu$。修正方法是在更新中乘以单步重要性权重：
  $$
  v_{\pi}(s_{t}) \leftarrow v_{\pi}(s_{t}) + \alpha \rho_{t} \left( r_{t+1} + \gamma v_{\pi}(s_{t+1}) - v_{\pi}(s_{t}) \right),
  $$
  其中 $\rho_{t} = \dfrac{\pi(a_{t} \mid s_{t})}{\mu(a_{t} \mid s_{t})}$。

- **Sarsa 的 Off-Policy 版本**（称为 **Expected Sarsa** 本身已是 Off-Policy 的变体）也可以使用重要性采样，但通常不必要，因为 Expected Sarsa 已经对动作取期望，消除动作采样偏差。

- **Q-Learning** 本质上是 Off-Policy 的，它直接使用 $\max_{a'} q(s_{t+1}, a')$ 作为目标，不依赖于行为策略 $\mu$ 的动作分布，因此**不需要重要性采样**。

### In TD($\lambda$)

对于 $n$-步 TD，重要性权重需要乘上对应步长的累积乘积：
$$
G_{t}^{\lambda,\mu \to \pi} = (1-\lambda) \sum_{n = 1}^{\infty} \lambda^{n - 1} \left( \prod_{k = 0}^{n - 1} \rho_{t + k} \right) G_{t}^{(n)},
$$
其中 $\rho_{t + k} = \dfrac{\pi(a_{t+k} \mid s_{t + k})}{\mu(a_{t+k} \mid s_{t+k})}$。
