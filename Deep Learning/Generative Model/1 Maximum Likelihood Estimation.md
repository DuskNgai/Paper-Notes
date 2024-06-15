# Maximum Likelihood Estimation

|                         Symbol                          |               Description               |
| :-----------------------------------------------------: | :-------------------------------------: |
|                        $\theta$                         |            model parameters             |
|                 $\mathbf{x}/\mathbf{X}$                 | explicit sample/explict random variable |
|                 $\mathbf{z}/\mathbf{Z}$                 |  hidden sample/hidden random variable   |
| $\mathcal{D}=\left\{\mathbf{x}^{(n)}\right\}_{n=1}^{N}$ |              training set               |

---

首先要明确，对于式子 $p(\mathbf{x}; \theta)$（通常也写作 $p(\mathbf{x} \mid \theta)$ 或 $p_{\theta}(\mathbf{x})$），其含义取决于已知条件：如果 $\mathbf{x}$ 已知而 $\theta$ 未知，该式表示**似然**（Likelihood）；如果 $\mathbf{x}$ 未知而 $\theta$ 已知，该式表示**条件概率**（Conditional Probability）。在推导过程中，这一点需要明确区分。

---

训练集 $\mathcal{D}$ 中包含了 $N$ 个已知的**独立同分布**（i.i.d.）的显变量样本。极大似然估计的目标是求解参数 $\theta$ 的最优值，使得训练集 $\mathcal{D}$ 的似然函数 $\mathcal{L}(\mathcal{D}; \theta)$ 最大化。训练集的似然函数定义为样本的联合分布：
$$
\begin{align*}
\mathcal{L}(\mathcal{D}; \theta) &= p(\mathcal{D}; \theta) \\
&= p\left(\mathbf{x}^{(1)}, \dots, \mathbf{x}^{(N)}; \theta\right) \\
&= \prod_{n=1}^N p\left(\mathbf{x}^{(n)}; \theta\right) \quad &\text{(i.i.d.)}
\end{align*}
$$
因此，最优参数 $\theta$ 的求解转化为以下优化问题：
$$
\begin{align*}
\theta^* &= \arg\max_{\theta} \mathcal{L}(\mathcal{D}; \theta) \\
&= \arg\max_{\theta} \prod_{n=1}^N p\left(\mathbf{x}^{(n)}; \theta\right) \\
&= \arg\max_{\theta} \sum_{n=1}^N \log p\left(\mathbf{x}^{(n)}; \theta\right) \quad &\text{(Log is monotonic increasing)}
\end{align*}
$$
通常，通过对似然函数求导来找到极大值点以求解最优参数 $\theta$，也可以使用其他估计方法（如不等式放缩）来得到极大值点。

---

> 随机变量 $X \sim \mathcal{N}(\mu, \sigma^2)$，现有其 $N$ 个样本 $\{x^{(n)}\}_{n=1}^{N}$，求 $\mu$ 和 $\sigma^2$ 的极大似然估计。

似然函数表示为：
$$
\begin{align*}
\mathcal{L}\left(\left\{x^{(n)}\right\}_{n=1}^{N}; \mu, \sigma^2\right) &= \sum_{n=1}^N \log p\left(x^{(n)}; \mu, \sigma^2\right) \\
&= \sum_{n=1}^N \log\left[\frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x^{(n)} - \mu)^2}{2\sigma^2}\right)\right] \\
&= -\frac{N}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2}\sum_{n=1}^N (x^{(n)} - \mu)^2
\end{align*}
$$
对于 $\mu$ 和 $\sigma^2$ 求导可得：
$$
\begin{align*}
\frac{\partial \mathcal{L}\left(\{x^{(n)}\}_{n=1}^{N}; \mu, \sigma^2\right)}{\partial \mu} &= \frac{1}{\sigma^2} \sum_{n=1}^N (x^{(n)} - \mu) \\
\frac{\partial \mathcal{L}\left(\{x^{(n)}\}_{n=1}^{N}; \mu, \sigma^2\right)}{\partial \sigma^2} &= -\frac{N}{2\sigma^2} + \frac{1}{2\sigma^4}\sum_{n=1}^N (x^{(n)} - \mu)^2
\end{align*}
$$
令二者为 $0$，得到极大值：
$$
\begin{align*}
\mu^* &= \frac{1}{N} \sum_{n=1}^{N} x^{(n)} = \bar{x} \\
{\sigma^2}^* &= \frac{1}{N} \sum_{n=1}^N (x^{(n)} - \bar{x})^2
\end{align*}
$$

> 随机变量 $X \sim \mathrm{Expo}(\lambda)$，现有其 $N$ 个样本 $\{x^{(n)}\}_{n=1}^{N}$，求 $\lambda$ 的极大似然估计。

似然函数表示为：
$$
\begin{align*}
\mathcal{L}\left(\{x^{(n)}\}_{n=1}^{N}; \lambda\right) &= \sum_{n=1}^N \log p\left(x^{(n)}; \lambda\right) \\
&= \sum_{n=1}^N \log\left[\lambda \exp(-\lambda x^{(n)})\right] \\
&= N \log \lambda - \lambda \sum_{n=1}^N x^{(n)}
\end{align*}
$$
对于 $\lambda$ 求导可得：
$$
\frac{\partial \mathcal{L}\left(\{x^{(n)}\}_{n=1}^{N}; \lambda\right)}{\partial \lambda} = \frac{N}{\lambda} - \sum_{n=1}^N x^{(n)}
$$
令上式为 $0$，得到极大值：
$$
\lambda^* = \frac{N}{\sum_{n=1}^{N} x^{(n)}} = \frac{1}{\bar{x}}
$$

> Story Telling

假设某地区人群的身高服从正态分布 $\mathcal{N}(\mu, \sigma^2)$，随机抽取 $n$ 个人的样本可以用来求得正态分布的参数。
