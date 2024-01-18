# Maximum Likelihood Estimation

|                         Symbol                          |          Description           |
| :-----------------------------------------------------: | :----------------------------: |
|                        $\theta$                         |        model parameters        |
|                      $\mathbf{X}$                       |   set of explicit variables    |
|                      $\mathbf{Z}$                       |    set of hidden variables     |
|                      $\mathbf{x}$                       | a sample of explicit variables |
| $\mathcal{D}=\left\{\mathbf{x}^{(n)}\right\}_{n=1}^{N}$ |          training set          |

---

首先要明确，对于式子 $p(\mathbf{x};\theta)$（一般也写作 $p(\mathbf{x}\mid\theta)$），如果 $\mathbf{x}$ 已知而 $\theta$ 未知，那么该式表示一个似然（Likelihood）；如果 $\mathbf{x}$ 未知而 $\theta$ 已知，那么该式表示一个条件概率（Probability）。现在，训练集 $\mathcal{D}$ 中包含了 $N$ 个已知的、**独立同分布的**显变量样本，极大似然估计的目标是求解参数 $\theta$ 的最优值，使得训练集 $\mathcal{D}$ 的似然函数 $\mathcal{L}(\mathcal{D};\theta)$ 极大。训练集的似然函数表示为
$$
\begin{align*}
\mathcal{L}(\mathcal{D};\theta)&=p(\mathcal{D};\theta)\\
&=p\left(\mathbf{x}^{(1)},\dots,\mathbf{x}^{(n)};\theta\right)\\
&=\prod_{n=1}^N p\left(\mathbf{x}^{(n)};\theta\right)&\text{(Independent and identically distributed)}
\end{align*}
$$
因此，最优参数 $\theta$ 的求解可以转化为以下优化问题
$$
\begin{align*}
\theta^*&=\mathop{\arg\max}_{\theta}\mathcal{L}(\mathcal{D};\theta)\\
&=\mathop{\arg\max}_{\theta}\prod_{n=1}^N p\left(\mathbf{x}^{(n)};\theta\right)\\
&=\mathop{\arg\max}_{\theta}\sum_{n=1}^N\log p\left(\mathbf{x}^{(n)};\theta\right)&\text{(Logarithm is monotonic increasing)}
\end{align*}
$$
通常通过求导得到极大值点就可以求解出来，也可以用别的估计方法（不等式放缩）求得极大值点。

---

> 假设随机变量 $X\sim\mathcal{N}(\mu,\sigma^2)$，现有其 $N$ 个样本 $\left\{x^{(n)}\right\}_{n=1}^{N}$，求得 $\mu$ 和 $\sigma^2$ 的极大似然估计。

似然函数表示为
$$
\begin{align*}
\mathcal{L}\left(\left\{x^{(n)}\right\}_{n=1}^{N};\mu,\sigma^2\right)&=\sum_{n=1}^N\log p\left(x^{(n)};\mu,\sigma^2\right)\\
&=\sum_{n=1}^N\log\left[\frac{1}{\sqrt{2\pi\sigma^2}}\exp\left(-\frac{\left(x^{(n)}-\mu\right)^2}{2\sigma^2}\right)\right]\\
&=-\frac{n}{2}\log\left(2\pi\sigma^2\right)-\frac{1}{2\sigma^2}\sum_{n=1}^N\left(x^{(n)}-\mu\right)^2
\end{align*}
$$
对于 $\mu$ 和 $\sigma^2$ 求导可得
$$
\begin{align*}
\frac{\partial\mathcal{L}\left(\left\{x^{(n)}\right\}_{n=1}^{N};\mu,\sigma^2\right)}{\partial\mu}&=\frac{1}{\sigma^2}\sum_{n=1}^N\left(x^{(n)}-\mu\right)\\
\frac{\partial\mathcal{L}\left(\left\{x^{(n)}\right\}_{n=1}^{N};\mu,\sigma^2\right)}{\partial\sigma^2}&=\frac{1}{2\sigma^4}\sum_{n=1}^N\left(x^{(n)}-\mu\right)^2-\frac{n}{2\sigma^2}
\end{align*}
$$
令二者为 $0$ 可以得到极大值
$$
\begin{align*}
\mu^*&=\frac{1}{n}\sum_{n=1}^{N}x^{(n)}=\bar{x}\\
{\sigma^2}^*&=\frac{1}{n}\sum_{n=1}^N\left(x^{(n)}-\bar{x}\right)^2
\end{align*}
$$

> 假设随机变量 $X\sim\mathrm{Expo}(\lambda)$，现有其 $N$ 个样本 $\left\{x^{(n)}\right\}_{n=1}^{N}$，求得 $\lambda$ 的极大似然估计。

似然函数表示为
$$
\begin{align*}
\mathcal{L}\left(\left\{x^{(n)}\right\}_{n=1}^{N};\lambda\right)&=\sum_{n=1}^N\log p\left(x^{(n)};\lambda\right)\\
&=\sum_{n=1}^N\log\left[\lambda\exp\left(-\lambda x^{(n)}\right)\right]\\
&=n\log \lambda-\lambda\sum_{n=1}^N x^{(n)}
\end{align*}
$$
对于 $\lambda$ 求导可得
$$
\frac{\partial\mathcal{L}\left(\left\{x^{(n)}\right\}_{n=1}^{N};\lambda\right)}{\partial\lambda}=\frac{n}{\lambda}-\sum_{n=1}^N x^{(n)}
$$
令上式为 $0$ 可以得到极大值
$$
\lambda^*=\frac{n}{\sum_{n=1}^{N}x^{(n)}}=\frac{n}{\bar{x}}
$$

> Story Telling

假设某地区人群的身高服从正态分布 $\mathcal{N}$，因此随机抽取 $n$ 个人，可以求得正态分布的参数。
