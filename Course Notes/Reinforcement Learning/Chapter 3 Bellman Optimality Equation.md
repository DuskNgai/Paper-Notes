# Chapter 3 Bellman Optimality Equation

**Policy Partial Ordering**：在强化学习中，我们可以通过比较不同策略在所有状态下的状态值函数，来定义策略之间的偏序关系。如果策略 $\pi$ 在所有状态下的期望回报都不小于策略 $\pi'$，则称策略 $\pi$ 优于或等于策略 $\pi'$，记作 $\pi \geq \pi'$：
$$
\pi \geq \pi' \iff v_{\pi}(s) \geq v_{\pi'}(s), \quad \forall s \in \mathcal{S}
$$

**Optimal Policy**：基于上述偏序关系，总是存在至少一个策略优于或等于所有其他策略。$\pi_{*}$ 是一个最优策略，当且仅当对于所有其他策略 $\pi$，都有 $\pi_{*} \geq \pi$（即对于所有状态 $s$ 都有 $v_{\pi_{*}}(s) \geq v_{\pi}(s)$）：
$$
\pi_{*} \triangleq \mathop{\arg\max}_{\pi} v_{\pi}(s), \quad \forall s \in \mathcal{S}
$$

**Optimal Value Functions**：最优策略可能不止一个，但所有的最优策略都共享相同的最优状态值函数和最优动作值函数，分别记为 $v_{*}$ 和 $q_{*}$：
$$
v_{*}(s) \triangleq \max_{\pi} v_{\pi}(s), \quad q_{*}(s, a) \triangleq \max_{\pi} q_{\pi}(s, a), \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$
此时，最优策略 $\pi_{*}$ 可以通过最优动作值函数 $q_{*}$ 来定义：
$$
\pi_{*}(a \mid s) = \begin{cases}
1, & \text{if } a = \mathop{\arg\max}_{a' \in \mathcal{A}(s)} q_{*}(s, a') \\
0, & \text{otherwise}
\end{cases}, \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$

Bellman 最优方程是一个描述最优状态值函数和最优动作值函数之间关系的非线性方程，是 Bellman 方程的形式化推广。仿照 Bellman 方程的形式，我们可以得到 Bellman 最优方程。

## Bellman Optimality Equation in Recursive Form

**Bellman Optimality Equation for State Value Function**：
$$
v_{*}(s) = \max_{a \in \mathcal{A}(s)} \mathbb{E}[R_{t + 1} + \gamma v_{*}(S_{t + 1}) \mid S_{t} = s, A_{t} = a], \quad \forall s \in \mathcal{S}
$$
展开后可以得到更具体的形式：
$$
v_{*}(s) = \max_{a \in \mathcal{A}(s)} \left[ \sum_{r} p(r \mid s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s' \mid s, a) v_{*}(s') \right], \quad \forall s \in \mathcal{S}
$$

**Bellman Optimality Equation for Action Value Function**：
$$
q_{*}(s, a) = \mathbb{E}[R_{t + 1} + \gamma \max_{a' \in \mathcal{A}(S_{t + 1})} q_{*}(S_{t + 1}, a') \mid S_{t} = s, A_{t} = a], \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$
展开后可以得到更具体的形式：
$$
q_{*}(s, a) = \sum_{r} p(r \mid s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s' \mid s, a) \max_{a' \in \mathcal{A}(s')} q_{*}(s', a'), \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$

## Bellman Optimality Equation in Explicit Form

**From Action Value to State Value**：
$$
v_{*}(s) = \max_{a \in \mathcal{A}(s)} q_{*}(s, a), \quad \forall s \in \mathcal{S}
$$

**From State Value to Action Value**：
$$
q_{*}(s, a) = \sum_{r} p(r \mid s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s' \mid s, a) v_{*}(s'), \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$

## Bellman Optimality Equation in Matrix Form

$$
\mathbf{v}_{*} = \max_{\pi} \left( \mathbf{r}_{\pi} + \gamma \mathbf{P}_{\pi} \mathbf{v}_{*} \right)
$$

重要性质：
1. 唯一性：基于压缩映射定理（Contraction Mapping Theorem），Bellman 最优方程对于最优状态值函数 $v_{*}$ 有唯一解（但对应的最优策略 $\pi_{*}$ 可能不唯一）。
2. 非线性：由于方程中包含了 $\max$ 算子，Bellman 最优方程是一个非线性方程。因此无法像求解策略评估那样直接求矩阵逆得到闭式解。
3. 求解方法：虽然方程是非线性的，但求解 $v_{*}$ 的问题可以等价转化为一个线性规划问题（在实际强化学习中，通常通过**价值迭代 (Value Iteration)** 或 **策略迭代 (Policy Iteration)** 等动态规划方法来迭代求解）。
