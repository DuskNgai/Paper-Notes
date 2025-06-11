# Expectation-Maximization

记号同 [Maximum Likelihood Estimation](maximum%20likelihood%20estimation.md)。

---

EM 算法是一种迭代算法，用于求解**含有隐变量**的概率模型的极大似然估计。为了说明引入隐变量的原因，这里引用来自 [What is the Expectation Maximization Algorithm](https://www.nature.com/articles/nbt1406) 的例子：

> 硬币 A 和 B 的参数 $\theta_A$ 和 $\theta_B$ 未知，每次选择哪一枚硬币去抛掷也是未知的，只知道抛掷结果。假如知道每次选择的硬币，则可以利用极大似然估计分别求解 $\theta_A$ 和 $\theta_B$。为了克服这个困难，我们引入一个满足伯努利分布的隐变量 $Z$，表示每次选择的硬币，这样就能利用 EM 算法求解 $\theta_A$ 和 $\theta_B$ 以及 $Z$ 的参数。

---

我们可以将训练集 $\mathcal{D}$ 看作由显变量 $\mathbf{X}$ 和隐变量 $\mathbf{Z}$ 组成的联合分布 $p(\mathbf{X}, \mathbf{Z})$ 采样得到的。假设 $\mathbf{Z}$ 是离散的，一个样本 $\mathbf{x}^{(n)}$ 的**边际似然函数**为：
$$
p\left(\mathbf{x}^{(n)}; \theta\right) = \sum_{i} p\left(\mathbf{x}^{(n)}, \mathbf{z}^{(i)}; \theta\right) = \sum_{i} p\left(\mathbf{x}^{(n)} \mid \mathbf{z}^{(i)}; \theta\right)p\left(\mathbf{z}^{(i)}; \theta\right)
$$
如果 $\mathbf{Z}$ 是连续的，则有：
$$
p\left(\mathbf{x}^{(n)}; \theta\right) = \int p\left(\mathbf{x}^{(n)}, \mathbf{z}; \theta\right) \mathrm{d} \mathbf{z} = \int p\left(\mathbf{x}^{(n)} \mid \mathbf{z}; \theta\right)p(\mathbf{z}; \theta) \mathrm{d} \mathbf{z}
$$
边际似然也称为**证据**（Evidence）。

以下推导以连续的隐变量 $\mathbf{Z}$ 为例。我们的目标是求解参数 $\theta$，使得边际似然取得极大值：
$$
\begin{aligned}
\theta^* &= \mathop{\arg\max}_{\theta} \sum_{n = 1}^N \log p\left(\mathbf{x}^{(n)}; \theta\right) \\
&= \mathop{\arg\max}_{\theta} \sum_{n = 1}^N \log \int p\left(\mathbf{x}^{(n)} \mid \mathbf{z}; \theta\right)p(\mathbf{z}; \theta) \mathrm{d} \mathbf{z}
\end{aligned}
$$
由于隐变量 $\mathbf{Z}$ 的存在，边际似然关于 $\theta$ 的导数变得非常复杂：
$$
\frac{\partial \mathcal{L}}{\partial \theta} = \sum_{n = 1}^N \frac{\partial }{\partial \theta} \log \int p\left(\mathbf{x}^{(n)} \mid \mathbf{z}; \theta\right)p(\mathbf{z}; \theta) \mathrm{d} \mathbf{z}
$$
EM 算法引入了一个任意选取的分布函数 $q(\mathbf{z})$，称为**变分分布**（Variational Distribution），并利用 Jensen 不等式，将 $\log$ 移到最内层，得到对数边际似然的一个下界：
$$
\begin{aligned}
\log p\left(\mathbf{x}^{(n)}; \theta\right) &= \log \int p\left(\mathbf{x}^{(n)}, \mathbf{z}; \theta\right) \mathrm{d} \mathbf{z} \\
&= \log \int q(\mathbf{z}) \frac{p\left(\mathbf{x}^{(n)}, \mathbf{z}; \theta\right)}{q(\mathbf{z})} \mathrm{d} \mathbf{z} \\
&= \log \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \frac{p\left(\mathbf{x}^{(n)}, \mathbf{z}; \theta\right)}{q(\mathbf{z})} \right] \\
&\geq \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \log \frac{p\left(\mathbf{x}^{(n)}, \mathbf{z}; \theta\right)}{q(\mathbf{z})} \right] \quad &\text{(Logarithm is concave)}
\end{aligned}
$$
最后一个不等式定义了**证据下界**（Evidence Lower Bound, ELBO）：
$$
\color{red}\mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta) = \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \log \frac{p(\mathbf{x}, \mathbf{z}; \theta)}{q(\mathbf{z})} \right]
$$
进一步地，我们可以将 ELBO 和边际似然用 KL 散度关联起来：
$$
\begin{aligned}
\mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta) &= \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \log \frac{p(\mathbf{x}, \mathbf{z}; \theta)}{q(\mathbf{z})} \right] \\
&= \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \log \frac{p(\mathbf{z} \mid \mathbf{x}; \theta)p(\mathbf{x}; \theta)}{q(\mathbf{z})} \right] \\
&= \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \log p(\mathbf{x}; \theta)\right] + \mathbb{E}_{\mathbf{z} \sim q(\mathbf{z})} \left[ \log \frac{p(\mathbf{z} \mid \mathbf{x}; \theta)}{q(\mathbf{z})} \right] \\
&= \log p(\mathbf{x}; \theta) - \mathop{\mathrm{KL}}[q(\mathbf{z}) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]
\end{aligned}
$$
即：
$$
\color{red}\log p(\mathbf{x}; \theta) = \mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta) + \mathop{\mathrm{KL}}[q(\mathbf{z}) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]
$$
由于 KL 散度非负，因此 $\mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta) \leq \log p(\mathbf{x}; \theta)$。仅当 KL 散度为 $0$，即 $q(\mathbf{z}) = p(\mathbf{z} \mid \mathbf{x}; \theta)$ 时，ELBO 取得极大值。

这样，求解含有隐变量的概率模型的极大似然估计问题的 EM 算法可以分解为两个步骤：

1. **E 步**：固定参数 $\theta_t$，找到一个尽可能逼近 $p(\mathbf{z} \mid \mathbf{x}; \theta_t)$ 的变分分布 $q_{t + 1}(\mathbf{z})$，使得 KL 散度取到极小值（0）：
    $$
    q_{t + 1}(\mathbf{z}) = \mathop{\arg\min}_{q} \mathop{\mathrm{KL}} [q(\mathbf{z}) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta_t)]
    $$
2. **M 步**：固定变分分布 $q_{t + 1}(\mathbf{z})$，找到一个新的参数 $\theta_{t + 1}$，使得由 ELBO 表示的边际似然取得极大值：
    $$
    \theta_{t + 1} = \mathop{\arg\max}_{\theta} \mathop{\mathrm{ELBO}}(q_{t + 1}, \mathbf{x}; \theta)
    $$

## Proof of Convergence

![em](images/em.png)

第 $t$ 次迭代后，设参数为 $\theta_t$，变分分布为 $q_t(\mathbf{z})$。在 E 步找到变分分布 $q_{t + 1}(\mathbf{z})$，使得 $\mathop{\mathrm{KL}}[q(\mathbf{z}) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)] = 0$，即 $\mathop{\mathrm{ELBO}}\left(q_{t + 1}, \mathbf{x}; \theta_t\right)=\log p\left(\mathbf{x}; \theta_t\right)$。在 M 步找到参数 $\theta_{t + 1}$，使得 $\mathop{\mathrm{ELBO}}\left(q_{t + 1}, \mathbf{x}; \theta_{t + 1}\right)\geq \mathop{\mathrm{ELBO}}\left(q_{t + 1}, \mathbf{x}; \theta_t\right)$，极大化了用 ELBO 表示的边际似然。因此
$$
\log p\left(\mathbf{x}; \theta_{t + 1}\right) \ge \mathop{\mathrm{ELBO}}\left(q_{t + 1}, \mathbf{x}; \theta_{t + 1}\right) \ge \mathop{\mathrm{ELBO}}\left(q_{t + 1}, \mathbf{x}; \theta_t\right) = \log p\left(\mathbf{x}; \theta_t\right)
$$
即每次迭代后边际似然是递增的，由于边际似然是有上界的（$\log p\left(\mathbf{x}; \theta\right) \le 0$），因此EM算法是收敛的。

## Variational Inference

对于绝大部分问题来说，精确推断（即精确计算 $p(\mathbf{z} \mid \mathbf{x}; \theta)$）是非常困难的。因此，我们一般会寻找**形式简单或计算简单**的变分分布 $q^*$ 来近似 $p(\mathbf{z} \mid \mathbf{x}; \theta)$，将精确推断问题转化为泛函优化问题，这就是变分推断方法：
$$
q^*(\mathbf{z}) = \mathop{\arg\min}_{q} \mathop{\mathrm{KL}}[q(\mathbf{z}) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)]
$$
要优化这个目标函数，必须知道后验概率 $p(\mathbf{z} \mid \mathbf{x}; \theta)$ 的具体形式，但好在 EM 算法的核心公式告诉我们：
$$
\begin{aligned}
q^*(\mathbf{z}) &= \mathop{\arg\min}_{q \in \mathcal{Q}} \mathop{\mathrm{KL}}(q(\mathbf{z}) \parallel p(\mathbf{z} \mid \mathbf{x}; \theta)) \\
&= \mathop{\arg\min}_{q \in \mathcal{Q}} \left(\log p(\mathbf{x}; \theta) - \mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta)\right) \\
&= \mathop{\arg\max}_{q \in \mathcal{Q}} \mathop{\mathrm{ELBO}}(q, \mathbf{x}; \theta)
\end{aligned}
$$
这样就能将问题转化为最大化 ELBO。传统方法如 Variational Bayes EM 算法，通过迭代优化 $q$ 的各个分量来求解。对于深度学习，可以使用神经网络来拟合。

---

> Gaussian Mixture Model (GMM) 是由多个 Gaussian 分布组成的模型，其总体密度函数是多个 Gaussian 密度函数的加权组合。假设样本 $x$ 是从 $K$ 个 Gaussian 分布中随机采样得到的，但无法得知具体是从哪个 Gaussian 分布中采样。为了求解，引入隐变量 $z \in \{1, \dots, K\}$ 表示样本 $x$ 是从某个 Gaussian 分布中采样得到的，且 $z$ 服从分类分布，$p(z = k) = \pi_k \ge 0$，其中 $\pi_k$ 表示样本由第 $k$ 个 Gaussian 分布生成的概率，并满足 $\sum_{k = 1}^K \pi_k = 1$。

该模型的参数为 $\boldsymbol{\pi} = \{\pi_1, \dots, \pi_K\}, \boldsymbol{\mu} = \{\mu_1, \dots, \mu_K\}, \boldsymbol{\sigma} = \{\sigma_1, \dots, \sigma_K\}$，显变量为 $\left\{x^{(n)}\right\}_{n = 1}^N$，隐变量为 $z$。首先，获得对数边际似然函数：
$$
\begin{aligned}
\log p\left(x^{(n)}; \boldsymbol{\pi}, \boldsymbol{\mu}, \boldsymbol{\sigma}\right) &= \log \sum_{k = 1}^K p\left(x^{(n)}, z = k; \boldsymbol{\pi}, \boldsymbol{\mu}, \boldsymbol{\sigma}\right) \\
&= \log \sum_{k}^K p\left(x^{(n)} \mid z = k; \boldsymbol{\mu}, \boldsymbol{\sigma}\right)p\left(z = k; \boldsymbol{\pi}\right) \\
&= \log \sum_{k = 1}^K \mathcal{N}\left(x^{(n)}; \mu_k, \sigma_k\right) \pi_k
\end{aligned}
$$
随后是 E 步，固定参数，求解变分分布，即样本 $x^{(n)}$ 属于第 $k$ 个 Gaussian 分布的后验概率分布：
$$
\begin{aligned}
q\left(z = k \mid x^{(n)}\right) &= p\left(z = k \mid x^{(n)}; \boldsymbol{\pi}, \boldsymbol{\mu}, \boldsymbol{\sigma}\right) \\
&= \frac{p\left(x^{(n)}, z = k; \boldsymbol{\pi}, \boldsymbol{\mu}, \boldsymbol{\sigma}\right)}{p\left(x^{(n)}; \boldsymbol{\mu}, \boldsymbol{\sigma}\right)} \\
&= \frac{\mathcal{N}\left(x^{(n)}; \mu_k, \sigma_k\right) \pi_k}{\sum_{k = 1}^K \mathcal{N}\left(x^{(n)}; \mu_k, \sigma_k\right) \pi_k}
\end{aligned}
$$
随后是 M 步，固定变分分布，更新参数：
$$
\begin{aligned}
\mathop{\mathrm{ELBO}}\left(q, \{x^{(n)}\}_{n = 1}^{N}, \boldsymbol{\pi}, \boldsymbol{\mu}, \boldsymbol{\sigma}\right) &= \sum_{n = 1}^{N}\sum_{k = 1}^K q\left(z = k \mid x^{(n)}\right)\log\frac{p\left(x^{(n)}, z = k; \boldsymbol{\pi}, \boldsymbol{\mu}, \boldsymbol{\sigma}\right)}{q\left(z = k \mid x^{(n)}\right)} \\
&= \sum_{n = 1}^{N}\sum_{k = 1}^K q\left(z = k \mid x^{(n)}\right)\log\frac{\mathcal{N}\left(x^{(n)}; \mu_k, \sigma_k\right) \pi_k}{q\left(z = k \mid x^{(n)}\right)} \\
&= \sum_{n = 1}^{N}\sum_{k = 1}^K q\left(z = k \mid x^{(n)}\right)\left[-\frac{(x^{(n)} - \mu_k)^2}{2\sigma_k^2} - \log\sigma_k + \log\pi_k\right] + \text{Constant}
\end{aligned}
$$
利用 Lagrange 乘数法，求解参数，得到
$$
\begin{aligned}
\mu_k^* &= \frac{\sum_{n = 1}^{N} q\left(z = k \mid x^{(n)}\right) x^{(n)}}{\sum_{n = 1}^{N} q\left(z = k \mid x^{(n)}\right)} \\
\sigma_k^* &= \frac{\sum_{n = 1}^{N} q\left(z = k \mid x^{(n)}\right) (x^{(n)} - \mu_k^*)^2}{\sum_{n = 1}^{N} q\left(z = k \mid x^{(n)}\right)} \\
\pi_k^* &= \frac{\sum_{n = 1}^{N} q\left(z = k \mid x^{(n)}\right)}{N}
\end{aligned}
$$

> Story Telling

1. 假设某地区男性人群的身高服从正态分布 $\mathcal{N}(\mu_1, \sigma^2_1)$，女性人群的身高服从正态分布 $\mathcal{N}(\mu_2, \sigma^2_2)$，因此随机抽取 $N$ 个人，其中性别就是一个隐变量。
2. KMeans 算法可以看作是 GMM 算法的一个特例，其中显变量是数据点，隐变量是属于哪个聚类簇，参数是聚类簇的质心和半径。
