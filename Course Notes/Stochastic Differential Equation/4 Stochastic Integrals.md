# 4 Stochastic Integrals

## 4.1 Motivation

为了解决 [1 Introduction](1%20Introduction.md) 中的积分问题，我们首先需要定义随机过程对于时间的积分：
$$
\int_{0}^{T}X({t})\mathrm{d}t.
$$
我们知道对于给定的时间 $t$，$X(t)$ 是一个随机变量，因此积分也是一个随机变量。

然后，我们还需要定义随机过程对于 Wiener 过程的积分：
$$
\int_{0}^{T}g(t)\mathrm{d}W.
$$
Paley-Wiener-Zygmund 用分部积分法定义了这个积分。假设 $g:[0, T]\mapsto\mathbb{R}$ 是一个 C1 函数，且 $g(0)=g(T)=0$，那么
$$
\int_{0}^{T}g(t)\mathrm{d}W = -\int_{0}^{T}\frac{\mathrm{d}g(t)}{\mathrm{d}t}W({t})\mathrm{d}t.
$$
即转化为了对于时间的积分。这个积分有如下性质：
1. 期望为 $0$。
    > $$
    > \mathbb{E}\left[\int_{0}^{T}g(t)\mathrm{d}W\right] = -\int_{0}^{T}\frac{\mathrm{d}g(t)}{\mathrm{d}t}\mathbb{E}\left[W({t})\right]\mathrm{d}t = 0
    > $$.
2. 平方的期望为 $\int_{0}^{T}g^{2}({t})\mathrm{d}t$。
    > $$
    > \begin{aligned}
    > \mathbb{E}\left[\left(\int_{0}^{T}g(t)\mathrm{d}W\right)^{2}\right] &= \mathbb{E}\left[\left(-\int_{0}^{T}\frac{\mathrm{d}g(t)}{\mathrm{d}t}W({t})\mathrm{d}t\right)\left(-\int_{0}^{T}\frac{\mathrm{d}g(s)}{\mathrm{d}s}W({s})\mathrm{d}s\right)\right]\\
    > &= \int_{0}^{T}\int_{0}^{T}\frac{\mathrm{d}g(t)}{\mathrm{d}t}\frac{\mathrm{d}g(s)}{\mathrm{d}s}\underbrace{\mathbb{E}\left[W({t})W({s})\right]}_{=\min(t, s)}\mathrm{d}t\mathrm{d}s\\
    > &= \int_{0}^{T}\frac{\mathrm{d}g(t)}{\mathrm{d}t}\left[\int_{0}^{t}s\frac{\mathrm{d}g(s)}{\mathrm{d}s}\mathrm{d}s + \int_{t}^{T}t\frac{\mathrm{d}g(s)}{\mathrm{d}s}\mathrm{d}s\right]\mathrm{d}t\\
    > &= -\int_{0}^{T}\frac{\mathrm{d}g(t)}{\mathrm{d}t}\int_{0}^{t}g(s)\mathrm{d}s\mathrm{d}t\\
    > &= \int_{0}^{T}g^{2}({t})\mathrm{d}t
    > \end{aligned}
    > $$.

将上述两个性质推广到 L2 空间，我们可以用一系列 C1 函数的极限来定义随机过程对于 Wiener 过程的积分：
$$
\int_{0}^{T}g(t)\mathrm{d}W = \lim_{n\to\infty}\int_{0}^{T}g_{n}(t)\mathrm{d}W,
$$
其中 $g_{n}$ 是一系列 C1 函数，且 $g_{n}\to g$。

---

考虑这个积分
$$
\int_{0}^{T}W\mathrm{d}W
$$
取对于时间的一个分割 $P=\{0=t_{0}<t_{1}<\cdots<t_{n}=T\}$，定义 $|P| = \max_{i}|t_{i + 1} - t_{i}|$，$\tau_{i} = (1 - \lambda)t_{i} + \lambda t_{i + 1}$，其中 $0\leq\lambda\leq 1$。我们定义 Riemann 和为：
$$
R(P, \lambda) = \sum_{i=0}^{n-1}W(\tau_{i})(W(t_{i + 1}) - W(t_{i})).
$$
当分割 $P$ 趋于无穷小且 $\lambda$ 固定时，有：
$$
\lim_{n\to\infty}R(P, \lambda) = \frac{W(T)^{2}}{2} + \left(\lambda - \frac{1}{2}\right)T.
$$
证明略。

## B Definition and Properties of Ito's Integral

Ito 积分选取 $\lambda = 0$，定义 Ito 积分为：
$$
\int_{0}^{T}W\mathrm{d}W = \frac{W(T)^{2}}{2} - \frac{T}{2}.
$$
为什么要选取 $\lambda = 0$？因为当 $\lambda = 0$ 时，我们选择了分割的左端点。事实上，当我们站在左端点的时刻，我们不可能知道左端点到右端点之间的值，因此更符合随机过程的性质。这类随机过程叫 Non-anticipating Process。

设 $W$ 是在概率空间 $(\Omega, \mathcal{F}, \mathbb{P})$ 上的 1D Wiener 过程。定义两个 $\sigma$ 域：
1. 过去（History）：$\mathcal{W}(t) = \mathcal{U}(W(s):0\leq s\leq t)$。
2. 未来（Future）：$\mathcal{W}^{+}(t) = \mathcal{U}(W(s) - W(t):s\geq t)$。

非预期 $\sigma$ 域 $\mathcal{F}$ 定义为：
1. $\mathcal{F}(s)\subseteq \mathcal{F}(t)$，$\forall 0\leq s\leq t$。
2. $\mathcal{W}(t)\subseteq \mathcal{F}(t)$，$\forall t\geq 0$。
3. $\mathcal{F}(t)$ 与 $\mathcal{W}^{+}(t)$ 独立，$\forall t\geq 0$。

一句话来概括就是，非预期 $\sigma$ 域 $\mathcal{F}$ 是包含了过去信息的 $\sigma$ 域，但是不包含未来信息，且和白噪声有关。

非预期随机过程定义为一个 $\mathcal{F}$ 可测的随机过程。在这个域上定义的积分甚至不能交换积分次序，在此基础上，还需要定义 Progresive 过程。具体定义略。一般都在 Progresive 过程上定义 Ito 积分。

定义 $\mathbb{L}^{2}[0, T]$ 空间：所有满足 $\mathbb{E}\left[\int_{0}^{T}X^{2}(t)\mathrm{d}t\right]<\infty$ 的随机过程 $X(t)$。

定义 $\mathbb{L}^{1}[0, T]$ 空间：所有满足 $\mathbb{E}\left[\int_{0}^{T}|X(t)|\mathrm{d}t\right]<\infty$ 的随机过程 $X(t)$。


