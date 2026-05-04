# Chapter 6 Stochastic Approximation

TD 学习的前置知识。

## Mean Estimation

设 $X_{1}, X_{2}, \dots, X_{n}$ 是从分布 $P$ 中独立同分布（i.i.d.）采样得到的随机变量，令：
$$
\bar{X}_{k} = \frac{1}{k} \sum_{i = 1}^{k} X_{i}
$$
则 $\bar{X}_{k}$ 和 $\bar{X}_{k - 1}$ 的关系可以表示为：
$$
\begin{aligned}
\bar{X}_{k} &= \frac{1}{k} \sum_{i = 1}^{k} X_{i} \\
&= \frac{1}{k} \left( \sum_{i = 1}^{k - 1} X_{i} + X_{k} \right) \\
&= \frac{1}{k} \sum_{i = 1}^{k - 1} X_{i} + \frac{1}{k} X_{k} \\
&= \frac{k - 1}{k} \bar{X}_{k - 1} + \frac{1}{k} X_{k} \\
&= \bar{X}_{k - 1} - \frac{1}{k} \left( \bar{X}_{k - 1} - X_{k} \right) \\
\end{aligned}
$$
因此，我们得到了一个递归的更新公式：
$$
\bar{X}_{k} = \bar{X}_{k - 1} - \frac{1}{k} \left( \bar{X}_{k - 1} - X_{k} \right)
$$
或者：
$$
\bar{X}_{k} = \bar{X}_{k - 1} + \frac{1}{k} \left( X_{k} - \bar{X}_{k - 1} \right)
$$

### Robbins-Monro Algorithm

Robbins-Monro 算法是一种用于求解随机方程的根（即 $f(\theta) = 0$）的迭代方法，其中 $f: \mathbb{R}^{d} \to \mathbb{R}^{d}$ 是一个未知函数，我们无法直接计算其值，但可以在每一步获得带有噪声的观测。

设 $\theta^{*}$ 是方程 $f(\theta) = 0$ 的唯一根。在迭代的第 $k$ 步（$k = 1, 2, \dots$），给定上一步的估计 $\theta_{k - 1}$，我们观测到：
$$
Y_{k} = f(\theta_{k - 1}) + \eta_{k},
$$
其中 $\eta_{k}$ 是随机噪声，满足条件：
$$
\mathbb{E}[\eta_{k} \mid \mathcal{F}_{k - 1}] = 0,
$$
这里 $\mathcal{F}_{k - 1}$ 表示直到第 $k - 1$ 步的所有历史信息。算法的更新规则为：
$$
\theta_{k} = \theta_{k - 1} - \alpha_{k} Y_{k},
$$
其中 $\{\alpha_{k}\}$ 是正的学习率序列，通常要求：
$$
\sum_{k = 1}^{\infty} \alpha_{k} = \infty, \qquad \sum_{k = 1}^{\infty} \alpha_{k}^{2} < \infty.
$$
在适当的正则条件下（如 $f$ 单调、有界噪声方差等），$\theta_{k}$ 几乎必然收敛到 $\theta^{*}$。

### Stochastic Gradient Descent (SGD)

考虑以下优化问题：
$$
\min_{\theta \in \mathbb{R}^{d}} J(\theta) = \mathbb{E}_{X \sim P}[L(\theta, X)],
$$
其中 $L(\theta, X)$ 是损失函数，$P$ 是未知的数据分布。最优解 $\theta^{*}$ 满足最优性条件：
$$
\nabla J(\theta^{*}) = \mathbb{E}_{X \sim P}[\nabla L(\theta^{*}, X)] = 0.
$$

由于分布 $P$ 未知，无法直接计算梯度 $\nabla J(\theta)$，但可以获取独立同分布的样本 $X_1, X_2, \dots$，并利用带噪声的梯度估计进行迭代。

**SGD 迭代公式**（每次使用单个样本 $X_{k}$）：
$$
\theta_{k} = \theta_{k - 1} - \alpha_{k} \, \nabla L(\theta_{k - 1}, X_{k}), \quad k = 1, 2, \dots
$$
其中 $\{\alpha_{k}\}$ 是正的学习率序列，通常要求：
$$
\sum_{k = 1}^{\infty} \alpha_{k} = \infty, \qquad \sum_{k = 1}^{\infty} \alpha_{k}^2 < \infty.
$$

**变体**：
- **Batch Gradient Descent**：使用全部 $N$ 个样本：
  $$
  \theta_{k} = \theta_{k - 1} - \alpha_{k} \cdot \frac{1}{N} \sum_{i=1}^{N} \nabla L(\theta_{k - 1}, X_i).
  $$
- **Mini-Batch Gradient Descent**：使用大小为 $m$ 的小批量样本（可重复采样）：
  $$
  \theta_{k} = \theta_{k - 1} - \alpha_{k} \cdot \frac{1}{m} \sum_{i=1}^{m} \nabla L(\theta_{k - 1}, X_{i}^{(k)}).
  $$

---

Robbins-Monro（RM）算法用于求解未知函数 $f(\theta) = 0$ 的根，其中每一步只能获得带噪声的观测 $Y_{k} = f(\theta_{k - 1}) + \eta_{k}$，更新规则为：
$$
\theta_{k} = \theta_{k - 1} - \alpha_{k} Y_{k}.
$$

在 SGD 中，定义
$$
f(\theta) = \nabla J(\theta) = \mathbb{E}_{X \sim P}[\nabla L(\theta, X)].
$$
则最优解 $\theta^{*}$ 满足 $f(\theta^{*}) = 0$。每一步的观测为：
$$
Y_{k} = \nabla L(\theta_{k - 1}, X_{k}) = f(\theta_{k - 1}) + \eta_{k},
$$
其中噪声 $\eta_{k} = \nabla L(\theta_{k - 1}, X_{k}) - f(\theta_{k - 1})$ 满足 $\mathbb{E}[\eta_{k} \mid \mathcal{F}_{k - 1}] = 0$。因此，SGD 恰好是 RM 算法的一个特例，其中 $f$ 取为期望梯度，噪声由单样本梯度估计引入。
