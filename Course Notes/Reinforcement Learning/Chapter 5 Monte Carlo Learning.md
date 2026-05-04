# Chapter 5 Monte Carlo Learning

**Model-free**：不需要环境的动态模型（即状态转移概率和奖励函数）即可评估值函数或改进策略。
**Monte Carlo Methods**：通过从环境中采样得到的 Episodes 来估计值函数。它不依赖于动态模型，而是直接从实际交互的累积回报中学习。

## First-visit vs. Every-visit

在评估 $v_\pi(s)$ 或 $q_\pi(s, a)$ 时，一个 Episode 可能会多次经过同一个状态 $s$。根据如何处理这些重复访问，分为两种方法：

- **First-visit MC**：在一个 Episode 中，仅利用**第一次**访问状态 $s$ 后产生的回报 $G_{t}$ 来估计值函数。估计值是独立同分布的，能够产生无偏估计，但方差较大。
- **Every-visit MC**：在一个 Episode 中，**每一次**访问状态 $s$ 后产生的所有回报 $G_{t}$ 都会被纳入均值计算。虽然单个 Episode 内的数据不再独立，但随着样本量增加，它会收敛到真实值，且初始阶段方差通常较小。

## First-visit MC Policy Evaluation

**First-visit MC Policy Evaluation**：把 First-visit MC 方法应用于策略评估：
$$
v_{\pi}(s) = \mathbb{E}_{\pi}[G_{t} \mid S_{t} = s] \approx \frac{1}{n(s)} \sum_{i = 1}^{n(s)} g_{t}^{(i)}, \quad \forall s \in \mathcal{S}
$$

1. **Initialization**：初始化 $v_{0}(s) \in \mathbb{R}$，回报列表 $g(s) = \emptyset$。
2. **Infinite Loop**：重复以下步骤，直到收敛：
    1. **Episode Generation**：基于当前策略 $\pi$ 生成一个 Episode $s_{0}, a_{0}, r_{1}, \dots, s_{T - 1}, a_{T - 1}, r_{t}$。
    2. **Backward Update**：
        - 初始化回报 $g = 0$。
        - 从 Episode 的最后一个时间步 $T - 1$ 开始，向前遍历每个时间步 $t$：
            1. $g \leftarrow \gamma g + r_{t + 1}$。
            2. 如果 $s_{t}$ 是这个 Episode 的第一次访问：
                - $g(s_{t}) \leftarrow g(s_{t}) \cup \{g\}$。
                - $v_{k + 1}(s_{t}) \leftarrow \mathrm{average}(g(s_{t}))$。

从最后一个时间步开始向前遍历 Episode 的原因是回报 $G_{t}$ 是从时间步 $t$ 开始的累积回报，从后向前计算可以避免重复计算回报。

## Every-visit MC Policy Evaluation

在 First-visit MC 中，每个 Episode 中只有第一次访问的状态会被用于更新值函数，因此每个 Episode 利用率较低。

**Every-visit MC Policy Evaluation**：把 Every-visit MC 方法应用于策略评估，该方法利用 Episode 中每一次访问该状态后的回报来更新值函数，从而提高了单个 Episode 的数据利用率。

1. **Initialization**：初始化 $v_{0}(s) \in \mathbb{R}$，访问计数器 $n(s) = 0$。
2. **Infinite Loop**：重复以下步骤，直到收敛：
    1. **Episode Generation**：基于当前策略 $\pi$ 生成一个 Episode $s_{0}, a_{0}, r_{1}, \dots, s_{T - 1}, a_{T - 1}, r_{t}$。
    2. **Backward Update**：
        - 初始化回报 $g = 0$。
        - 从 Episode 的最后一个时间步 $T - 1$ 开始，向前遍历每个时间步 $t$：
            1. $g \leftarrow \gamma g + r_{t + 1}$。
            2. $n(s_{t}) \leftarrow n(s_{t}) + 1$。
            3. $v_{k + 1}(s_{t}) \leftarrow v_{k}(s_{t}) + (g - v_{k}(s_{t})) / n(s_{t})$。

## MC Exploring Starts

为了进行 Value Iteration，必须评估 $q(s, a)$。由于在确定性策略下许多“状态-动作对”可能无法被采样，因此引入探索性开始。

1. **Initialization**：初始化 $q_{0}(s, a) \in \mathbb{R}$，策略 $\pi$，访问计数器 $n(s, a) = 0$。
2. **Infinite Loop**：重复以下步骤：
    1. **Episode Generation**：随机选择起始状态-动作对 $(s_0, a_0)$，随后基于当前策略 $\pi$ 生成一个 Episode $s_{0}, a_{0}, r_{1}, \dots, s_{T - 1}, a_{T - 1}, r_{t}$。
    2. **Backward Update**：
        - 初始化回报 $g = 0$。
        - 从 Episode 的最后一个时间步 $T - 1$ 开始，向前遍历每个时间步 $t$：
            1. $g \leftarrow \gamma g + r_{t + 1}$。
            2. 如果 $(s_{t}, a_{t})$ 是这个 Episode 的第一次访问：
                - $n(s_{t}, a_{t}) \leftarrow n(s_{t}, a_{t}) + 1$。
                - $q_{k + 1}(s_{t}, a_{t}) \leftarrow q_{k}(s_{t}, a_{t}) + (g - q_{k}(s_{t}, a_{t})) / n(s_{t}, a_{t})$。
            3. **Policy Improvement**：对于每个状态 $s$，更新策略 $\pi$ 使其在 $s$ 上选择具有最大 $q(s, a)$ 的动作：
            $$
            \pi(a \mid s) = \begin{cases}
            1, & \text{if } a = \arg\max_{a'} q(s, a') \\
            0, & \text{otherwise}
            \end{cases}, \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
            $$

## MC $\epsilon$-Greedy

**MC $\epsilon$-Greedy**：MC Exploring Starts 依赖于“起始点随机”的假设，这在许多实际场景中难以实现。$\epsilon$-Greedy 方法通过在策略评估和改进中引入随机性，确保所有“状态-动作对”都能被持续探索。

**$\epsilon$-Greedy Policy**：在每个时间步 $t$，以概率 $\epsilon \in (0, 1]$ 选择一个随机动作（探索），以概率 $1 - \epsilon$ 选择当前最优动作（利用）：
$$
\pi(a \mid s) = \begin{cases}
1 - \epsilon + \dfrac{\epsilon}{|\mathcal{A}(s)|}, & \text{if } a = \arg\max_{a'} q_{\pi}(s, a') \\
\dfrac{\epsilon}{|\mathcal{A}(s)|}, & \text{if } a \neq \arg\max_{a'} q_{\pi}(s, a')
\end{cases}, \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$
该策略保证了最优动作被选择的概率始终大于其他动作：
$$
1 - \epsilon + \frac{\epsilon}{|\mathcal{A}(s)|} > \frac{\epsilon}{|\mathcal{A}(s)|}, \quad \forall s \in \mathcal{S}
$$
$\epsilon = 0$ 时，$\epsilon$-Greedy 策略退化为贪婪策略；$\epsilon = 1$ 时，$\epsilon$-Greedy 策略退化为完全随机策略。

### Boltzmann Exploration

$$
\pi(a \mid s) = \frac{\exp\left(\dfrac{q_{\pi}(s, a)}{\tau}\right)}{\sum_{a' \in \mathcal{A}(s)} \exp\left(\dfrac{q_{\pi}(s, a')}{\tau}\right)}, \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$
其中 $\tau > 0$ 为温度参数。

## Appendix

### Policy Update of $\epsilon$-Greedy
$\epsilon$-Greedy 策略的更新方法也是会带来提升的。记 $\pi_{t}(s)$ 是时间步 $t$ 的 $\epsilon$-Greedy 策略，有：
$$
\begin{aligned}
v_{\pi_{t + 1}}(s) &= \sum_{a \in \mathcal{A}(s)} \pi_{t + 1}(a \mid s) q_{\pi_{t}}(s, a) \\
&= \frac{\epsilon}{|\mathcal{A}(s)|} \sum_{a \in \mathcal{A}(s)} q_{\pi_{t}}(s, a) + (1 - \epsilon) \max_{a \in \mathcal{A}(s)} q_{\pi_{t}}(s, a) \\
&\ge \frac{\epsilon}{|\mathcal{A}(s)|} \sum_{a \in \mathcal{A}(s)} q_{\pi_{t}}(s, a) + (1 - \epsilon) \sum_{a \in \mathcal{A}(s)} \frac{\pi_{t}(a \mid s) - \dfrac{\epsilon}{|\mathcal{A}(s)|}}{1 - \epsilon} q_{\pi_{t}}(s, a) \\
&= \sum_{a \in \mathcal{A}(s)} \pi_{t}(a \mid s) q_{\pi_{t}}(s, a) \\
&= v_{\pi_{t}}(s)
\end{aligned}
$$
其中的不等号的原因是，$\pi_{t}$ 也是基于上一轮的 $\epsilon$-Greedy 更新的，有：
$$
\frac{\pi_{t}(a \mid s) - \dfrac{\epsilon}{|\mathcal{A}(s)|}}{1 - \epsilon} = \begin{cases}
1, & \text{if } a = \mathop{\arg\max}_{a' \in \mathcal{A}(s)} q_{\pi_{t - 1}}(s, a') \\
0, & \text{otherwise}
\end{cases}
$$
因此：
$$
\sum_{a \in \mathcal{A}(s)} \frac{\pi_{t}(a \mid s) - \dfrac{\epsilon}{|\mathcal{A}(s)|}}{1 - \epsilon} q_{\pi_{t}}(s, a) = q_{\pi_{t}}\left(s, \mathop{\arg\max}_{a' \in \mathcal{A}(s)} q_{\pi_{t - 1}}(s, a')\right) \le \max_{a \in \mathcal{A}(s)} q_{\pi_{t}}(s, a)
$$
