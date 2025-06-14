# Maximum Likelihood Estimation

|                           Symbol                            |               Description               |
| :---------------------------------------------------------: | :-------------------------------------: |
|                          $\theta$                           |            model parameters             |
|                $\mathbf{x}^{(n)}/\mathbf{x}$                | explicit sample/explict random variable |
|                $\mathbf{z}^{(i)}/\mathbf{z}$                |  hidden sample/hidden random variable   |
| $\mathcal{D} = \left\{\mathbf{x}^{(n)}\right\}_{n = 1}^{N}$ |              training set               |

---

## Likelihood v.s. Probability Distribution

在统计学中，表达式 $p(\mathbf{x}; \theta)$（等价形式有 $p(\mathbf{x} \mid \theta)$ 或 $p_{\theta}(\mathbf{x})$）的含义取决于已知条件：

- **当 $\mathbf{x}$ 已知而 $\theta$ 未知**：该式表示**似然函数**（Likelihood），描述不同参数下观测到当前数据的可能性；
- **当 $\theta$ 已知而 $\mathbf{x}$ 未知**：该式表示**概率分布**（Probability Distribution），描述在固定参数下数据的分布特性。

这一根本区别是理解极大似然估计的核心基础。

## Probabilistic Functions

每个概率分布都通过一个概率函数描述：
- **连续型分布**：概率密度函数（Probability Density Function, PDF）；
- **离散型分布**：概率质量函数（Probability Mass Function, PMF）。

这些函数由参数决定，例如：
- 均匀分布：参数为最小值 $a$ 和最大值 $b$；
- 高斯分布：参数为均值 $\mu$ 和方差 $\sigma^{2}$。

我们用 $\theta$ 表示任意分布的参数。

## Maximum Likelihood Estimation (MLE)

给定包含 $N$ 个**独立同分布**（i.i.d.）样本的训练集 $\mathcal{D}$，MLE 的目标是找到最优参数 $\theta^{*}$ 最大化训练集的似然函数：
$$
\begin{aligned}
p(\mathcal{D}; \theta) &=  p\left(\mathbf{x}^{(1)}, \dots, \mathbf{x}^{(N)}; \theta\right) && \text{(Joint Distribution)} \\
&= \prod_{n = 1}^{N} p\left(\mathbf{x}^{(n)}; \theta\right) && \text{(i.i.d.)}
\end{aligned}
$$

为简化计算，通常使用**对数似然函数**：
$$
\begin{aligned}
\theta^{*} &= \arg\max_{\theta} \log p(\mathcal{D}; \theta) \\
&= \arg\max_{\theta} \log \left( \prod_{n = 1}^{N} p\left(\mathbf{x}^{(n)}; \theta\right) \right) \\
&= \arg\max_{\theta} \sum_{n = 1}^{N} \log p\left(\mathbf{x}^{(n)}; \theta\right)
\end{aligned}
$$
这里记：
$$
\mathcal{L}(\mathcal{D}; \theta) = \sum_{n = 1}^{N} \log p\left(\mathbf{x}^{(n)}; \theta\right)
$$

## Examples of MLE

### Gaussian Distribution

> 随机变量 $X \sim \mathcal{N}(\mu, \sigma^{2})$，样本集 $\left\{x^{(n)}\right\}_{n = 1}^{N}$，求 $\mu$ 和 $\sigma^{2}$ 的 MLE。

似然函数：
$$
\begin{aligned}
\mathcal{L}(\mathcal{D}; \mu, \sigma^{2}) &= \sum_{n = 1}^{N} \log \left[ \frac{1}{\sqrt{2\pi\sigma^{2}}} \exp\left(-\frac{(x^{(n)} - \mu)^{2}}{2\sigma^{2}}\right) \right] \\
&= -\frac{N}{2}\log(2\pi\sigma^{2}) - \frac{1}{2\sigma^{2}}\sum_{n = 1}^{N} (x^{(n)} - \mu)^{2}
\end{aligned}
$$

求偏导：
$$
\begin{aligned}
\frac{\partial \mathcal{L}}{\partial \mu} &= \frac{1}{\sigma^{2}} \sum_{n = 1}^{N} (x^{(n)} - \mu) \\
\frac{\partial \mathcal{L}}{\partial \sigma^{2}} &= -\frac{N}{2\sigma^{2}} + \frac{1}{2\sigma^4}\sum_{n = 1}^{N} (x^{(n)} - \mu)^{2}
\end{aligned}
$$

令导数为零解得：
$$
\boxed{
\begin{aligned}
\mu^* &= \frac{1}{N} \sum_{n = 1}^{N} x^{(n)} = \bar{x} \\
{\sigma^{2}}^* &= \frac{1}{N} \sum_{n = 1}^{N} (x^{(n)} - \bar{x})^{2}
\end{aligned}
}
$$

容易验证所求参数为所求函数的极大值点。

### Exponential Distribution

> 随机变量 $X \sim \mathrm{Expo}(\lambda)$，样本集 $\left\{x^{(n)}\right\}_{n = 1}^{N}$，求 $\lambda$ 的 MLE。

似然函数：
$$
\begin{aligned}
\mathcal{L}(\mathcal{D}; \lambda) &= \sum_{n = 1}^{N} \log \left[ \lambda \exp\left(-\lambda x^{(n)}\right) \right] \\
&= N \log \lambda - \lambda \sum_{n = 1}^{N} x^{(n)}
\end{aligned}
$$

求导数：
$$
\frac{\partial \mathcal{L}}{\partial \lambda} = \frac{N}{\lambda} - \sum_{n = 1}^{N} x^{(n)}
$$

令导数为零解得：
$$
\boxed{\lambda^* = \frac{N}{\sum_{n = 1}^{N} x^{(n)}} = \frac{1}{\bar{x}}}
$$

### Uniform Distribution

> 随机变量 $X \sim \mathrm{Uniform}(a, b)$，样本集 $\left\{x^{(n)}\right\}_{n = 1}^{N}$，求 $a$ 和 $b$ 的 MLE。

似然函数：
$$
\begin{aligned}
\mathcal{L}(\mathcal{D}; a, b) &= \sum_{n = 1}^{N} \log \left[ \frac{1}{b - a} \right] \\
&= -N \log(b - a)
\end{aligned}
$$

优化问题：
$$
a^*, b^* = \arg\min_{a,b} \log(b - a) \quad \text{s.t.} \quad a \leq \min_{n} x^{(n)},  b \geq \max_{n} x^{(n)}
$$

解得：
$$
\boxed{a^* = \min_{n} x^{(n)}, \quad b^* = \max_{n} x^{(n)}}
$$

### Story Telling

假设某地区成年男性的身高服从正态分布 $\mathcal{N}(\mu, \sigma^{2})$，则其参数可以通过极大似然估计（MLE）来求解。

## Discussion

MLE 的核心挑战之一是模型选择，即**如何选择合适的概率概率函数 $p(\cdot; \theta)$**？这其实是一种结构先验假设，决定了模型的能力和复杂度。如果选择的模型过于简单，可能无法捕捉数据的真实分布；如果过于复杂，则可能导致过拟合。

贝叶斯方法通过引入参数先验 $p(\theta)$ 来解决这个问题，形成最大后验估计（MAP）：
$$
\theta^{*}_{\text{MAP}} = \arg\max_{\theta} p(\mathcal{D} \mid \theta)p(\theta)
$$
