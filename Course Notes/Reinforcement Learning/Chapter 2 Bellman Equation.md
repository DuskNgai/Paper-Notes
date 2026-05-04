# Chapter 2 Bellman Equation

## Value Functions

**State-Value Function**：在状态 $s$ 下，智能体在未来获得的期望回报，越高越好：
$$
v_{\pi}(s) \triangleq \mathbb{E}_{\pi}[G_{t} \mid S_{t} = s]
$$

**Action-Value Function**：在状态 $s$ 下采取动作 $a$，智能体在未来获得的期望回报，越高越好：
$$
q_{\pi}(s, a) \triangleq \mathbb{E}_{\pi}[G_{t} \mid S_{t} = s, A_{t} = a]
$$

通过求解 Bellman 方程，可以得到状态值函数和动作值函数的数值解，从而评估策略的好坏，并指导策略的改进。

## Bellman Expectation Equations in Recursive Form

**Bellman Equation for State Value Function**：计算状态值函数的递归公式：
$$
v_{\pi}(s) = \mathbb{E}_{\pi}[R_{t + 1} + \gamma v_{\pi}(S_{t + 1}) \mid S_{t} = s], \quad \forall s \in \mathcal{S}
$$
展开后可以得到更具体的形式：
$$
v_{\pi}(s) = \sum_{a \in \mathcal{A}(s)} \pi(a \mid s) \left[ \sum_{r} p(r \mid s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s' \mid s, a) v_{\pi}(s') \right], \quad \forall s \in \mathcal{S}
$$

**Bellman Equation for Action Value Function**：计算动作值函数的递归公式：
$$
q_{\pi}(s, a) = \mathbb{E}_{\pi}[R_{t + 1} + \gamma q_{\pi}(S_{t + 1}, A_{t + 1}) \mid S_{t} = s, A_{t} = a], \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$
展开后可以得到更具体的形式：
$$
q_{\pi}(s, a) = \sum_{r} p(r \mid s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s' \mid s, a) \sum_{a' \in \mathcal{A}(s')} \pi(a' \mid s') q_{\pi}(s', a'), \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
$$

## Bellman Expectation Equations in Explicit Form

**From Action Value to State Value**：状态值函数可以通过动作值函数计算得到：
$$
\begin{aligned}
v_{\pi}(s) &= \mathbb{E}_{\pi}[q_{\pi}(s, A_{t}) \mid S_{t} = s] \\ 
&= \sum_{a \in \mathcal{A}(s)} \pi(a \mid s) q_{\pi}(s, a), \quad \forall s \in \mathcal{S}
\end{aligned}
$$

**From State Value to Action Value**：动作值函数可以通过状态值函数计算得到：
$$
\begin{aligned}
q_{\pi}(s, a) &= \mathbb{E}_{\pi}[R_{t + 1} + \gamma v_{\pi}(S_{t + 1}) \mid S_{t} = s, A_{t} = a] \\
&= \sum_{r} p(r \mid s, a) r + \gamma \sum_{s' \in \mathcal{S}} p(s' \mid s, a) v_{\pi}(s'), \quad \forall s \in \mathcal{S}, a \in \mathcal{A}(s)
\end{aligned}
$$

## Bellman Equations in Matrix Form

将状态值函数的 Bellman 方程表示为矩阵形式：
$$
v_{\pi}(s_{i}) = \underbrace{\sum_{a \in \mathcal{A}(s_{i})} \pi(a \mid s_{i}) \sum_{r} r p(r \mid s_{i}, a)}_{r_{\pi}(s_{i})} + \gamma \sum_{s_{j} \in \mathcal{S}} \underbrace{\left( \sum_{a \in \mathcal{A}(s_{i})} \pi(a \mid s_{i}) p(s_{j} \mid s_{i}, a) \right)}_{P_{\pi}(s_{i}, s_{j})} v_{\pi}(s_{j}), \quad i = 1, 2, \ldots, |\mathcal{S}|
$$
因此得到对应的矩阵方程：
$$
\mathbf{v}_{\pi} = \mathbf{r}_{\pi} + \gamma \mathbf{P}_{\pi} \mathbf{v}_{\pi}
$$

**Policy Evaluation**：给定策略 $\pi$，求解状态值函数 $\mathbf{v}_{\pi}$ 的过程称为策略评估：
- **Closed-form Solution**：当状态空间较小时，可以直接求解线性方程组：$\mathbf{v}_{\pi} = (\mathbf{I} - \gamma \mathbf{P}_{\pi})^{-1} \mathbf{r}_{\pi}$。
- **Iterative Solution**：当状态空间较大时，可以使用迭代方法：$\mathbf{v}_{k + 1} \gets \mathbf{r}_{\pi} + \gamma \mathbf{P}_{\pi} \mathbf{v}_{k}$，直到收敛（是保证收敛的，因为 $\gamma < 1$）。

---

## Appendix

### Bellman Expectation Equations in Recursive Form

**Lemma 1**: Return decomposition
$$
\begin{aligned}
G_{t} &= \sum_{k = 0}^{\infty} \gamma^{k} R_{t + k + 1} \\
&= R_{t + 1} + \sum_{k = 1}^{\infty} \gamma^{k} R_{t + k + 1} \\
&= R_{t + 1} + \gamma \sum_{k = 0}^{\infty} \gamma^{k} R_{t + k + 2} \\
&= R_{t + 1} + \gamma G_{t + 1}
\end{aligned}
$$

**Lemma 2**: Future return of state value function
$$
\begin{aligned}
\mathbb{E}_{\pi}[G_{t + 1} \mid S_{t} = s] &= \mathbb{E}_{\pi}[\mathbb{E}_{\pi}[G_{t + 1} \mid S_{t} = s, S_{t + 1} = s'] \mid S_{t} = s] \\
&= \mathbb{E}_{\pi}[\mathbb{E}_{\pi}[G_{t + 1} \mid S_{t + 1} = s'] \mid S_{t} = s] && \text{(Markov property)} \\
&= \mathbb{E}_{\pi}[v_{\pi}(S_{t + 1}) \mid S_{t} = s] \\
\end{aligned}
$$

**Lemma 3**: Future return of action value function
$$
\begin{aligned}
\mathbb{E}_{\pi}[G_{t + 1} \mid S_{t} = s, A_{t} = a] &= \mathbb{E}_{\pi}[\mathbb{E}_{\pi}[G_{t + 1} \mid S_{t} = s, A_{t} = a, S_{t + 1} = s', A_{t + 1} = a'] \mid S_{t} = s, A_{t} = a] \\
&= \mathbb{E}_{\pi}[\mathbb{E}_{\pi}[G_{t + 1} \mid S_{t + 1} = s', A_{t + 1} = a'] \mid S_{t} = s, A_{t} = a] && \text{(Markov property)} \\
&= \mathbb{E}_{\pi}[q_{\pi}(S_{t + 1}, A_{t + 1}) \mid S_{t} = s, A_{t} = a] \\
\end{aligned}
$$

### Bellman Expectation Equations in Explicit Form

**From Action Value to State Value**
$$
\begin{aligned}
v_{\pi}(s) &= \mathbb{E}_{\pi}[G_{t} \mid S_{t} = s] \\
&= \sum_{a \in \mathcal{A}(s)} \pi(a \mid s) \mathbb{E}_{\pi}[G_{t} \mid S_{t} = s, A_{t} = a] \\
&= \sum_{a \in \mathcal{A}(s)} \pi(a \mid s) q_{\pi}(s, a)
\end{aligned}
$$
