# Chapter 4 Value Iteration & Policy Iteration

**Model-based**：需要环境的动态模型（即状态转移概率和奖励函数）来评估值函数或改进策略。

## Value Iteration

**Value Iteration**：通过迭代更新状态值函数来求解最优策略的方法。每次迭代根据 Bellman 最优方程更新状态值函数，直到收敛到最优状态值函数 $v_{*}$。

1. 初始化：对于所有状态 $s$，初始化 $v_{0}(s)$（可以是任意值，通常初始化为零）。
2. 动作值函数更新：对于每个状态 $s$ 和动作 $a$，根据当前的状态值函数 $v_{k}(s')$ 计算动作值函数 $q_{k}(s, a)$：
   $$
   q_{k}(s, a) \gets \sum_{r} r p(r \mid s, a) + \gamma \sum_{s' \in \mathcal{S}} p(s' \mid s, a) v_{k}(s'), \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
   $$
3. 状态值函数更新：根据 Bellman 最优方程更新状态值函数：
   $$
   v_{k + 1}(s) \gets \max_{a \in \mathcal{A}(s)} q_{k}(s, a), \quad \forall s \in \mathcal{S}
   $$
4. 策略提取：重复步骤 2 和 3，直到 $v_{k}$ 收敛到 $v_{*}$。此时最优策略 $\pi_{*}$ 可以通过以下方式提取：
   $$
   \pi_{*}(a \mid s) = \begin{cases}
   1, & \text{if } a = \mathop{\arg\max}_{a' \in \mathcal{A}(s)} q_{*}(s, a') \\
   0, & \text{otherwise}
   \end{cases}, \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
   $$

## Policy Iteration

**Policy Iteration**：通过交替进行策略评估和策略改进来求解最优策略的方法。

1. 初始化：对于所有状态 $s$，初始化策略 $\pi_{0}(a \mid s)$。
2. **Policy Evaluation**：对于当前策略 $\pi_{k}$，求解其对应的状态值函数 $v_{\pi_{k}}(s)$。若状态空间较大，通常使用迭代法求解：
    $$
    v_{\pi_{k}}^{(j + 1)}(s) \gets \sum_{a \in \mathcal{A}(s)} \pi_{k}(a \mid s) \left[ \sum_{r} p(r \mid s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s' \mid s, a) v_{\pi_{k}}^{(j)}(s') \right], \quad \forall s \in \mathcal{S}
    $$
3. **Policy Improvement**：根据当前的状态值函数 $v_{\pi_{k}}(s)$，贪心地更新策略 $\pi_{k + 1}(a \mid s)$：
    $$
    \pi_{k + 1}(a \mid s) \gets \begin{cases}
    1, & \text{if } a = \mathop{\arg\max}_{a' \in \mathcal{A}(s)} q_{\pi_{k}}(s, a') \\
    0, & \text{otherwise}
    \end{cases}, \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
    $$
4. 重复步骤 2 和 3，直到策略 $\pi_{k}$ 收敛到最优策略 $\pi_{*}$。

## Truncated Policy Iteration

**Truncated Policy Iteration**：在每次策略评估阶段不完全评估状态值函数，而是进行有限次迭代更新。这种方法在某些情况下可以加速收敛。Value Iteration 和 Policy Iteration 都可以看作是 Truncated Policy Iteration 的特例，其中 Value Iteration 在每次策略评估阶段只进行一次迭代更新，而 Policy Iteration 则进行完全评估直到收敛。

## Appendix

### Policy Improvement

> 证明的核心在于局部的策略改进可以导致全局的策略改进。

已知策略 $\pi'$ 定义为：
$$
\pi'(a \mid s) = \mathop{\arg\max}_{a \in \mathcal{A}(s)} q_{\pi}(s, a), \quad \forall s \in \mathcal{S}
$$
新的策略 $\pi'$ 相对于旧的策略 $\pi$ 有：
$$
q_{\pi}(s, \pi'(s)) = \max_{a \in \mathcal{A}(s)} q_{\pi}(s, a) \geq \mathbb{E}_{\pi}[q_{\pi}(s, A_{t + 1}) \mid S_{t} = s] = v_{\pi}(s), \quad \forall s \in \mathcal{S}
$$
利用 Bellman 方程，我们有：
$$
\begin{aligned}
v_{\pi}(s) &\leq q_{\pi}(s, \pi'(s)) \\
&= \mathbb{E}_{\pi'}[R_{t + 1} + \gamma v_{\pi}(S_{t + 1}) \mid S_{t} = s, A_{t} = \pi'(S_{t})] \\
&= \mathbb{E}_{\pi'}[R_{t + 1} + \gamma v_{\pi}(S_{t + 1}) \mid S_{t} = s] && (\pi' \text{ is deterministic})
\end{aligned}
$$
注意 $\mathbb{E}_{\pi'}$ 表示第一动作按 $\pi'$ 选取，而后续状态的期望回报仍沿用旧策略 $\pi$ 的值函数 $v_{\pi}$。
$$
\begin{aligned}
v_{\pi}(s) &\leq \mathbb{E}_{\pi'}[R_{t + 1} + \gamma v_{\pi}(S_{t + 1}) \mid S_{t} = s] \\
&\leq \mathbb{E}_{\pi'}[R_{t + 1} + \gamma \mathbb{E}_{\pi'}[R_{t + 2} + \gamma v_{\pi}(S_{t + 2}) \mid S_{t + 1}] \mid S_{t} = s] \\
&= \mathbb{E}_{\pi'}[R_{t + 1} + \gamma R_{t + 2} + \gamma^{2} v_{\pi}(S_{t + 2}) \mid S_{t} = s] && (\text{LOTE}) \\
&\leq \mathbb{E}_{\pi'}\left[ \sum_{k = 0}^{\infty} \gamma^{k} R_{t + 1 + k} \mid S_{t} = s \right] \\
&= v_{\pi'}(s) && (\text{Definition of } v_{\pi'})
\end{aligned}
$$
因此，对于所有状态 $s$，我们有 $v_{\pi'}(s) \geq v_{\pi}(s)$，这表明 $\pi'$ 是 $\pi$ 的改进策略。
