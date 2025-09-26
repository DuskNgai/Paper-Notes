# Chapter 3 Stochastic Equations with Gaussian Noise

从不太严谨的角度介绍了随机微分方程的基本概念。

## 3.2 Gaussian Increments and the Continuum Limit

从最简单的、包含随机变量的差分方程出发：
$$
\Delta x(t_{n}) = \epsilon_{n} \sqrt{\Delta t}
$$
其中 $t_{n} = n \Delta t$, $\epsilon_{n} \sim \mathcal{N}(0, 1)$ 是相互独立的随机变量。定义维纳增量 (Wiener increment) $\Delta w_{n} = \epsilon_{n} \sqrt{\Delta t}$，它服从正态分布 $\mathcal{N}(0, \Delta t)$。因此，该差分方程的解就是各个独立增量的累加：
$$
x_{n} \equiv x(t_{n}) = x(0) + \sum_{i = 0}^{n - 1} \Delta w_{i}
$$
由于相互独立的高斯随机变量之和也服从高斯分布，我们可以通过计算 $x_{n}$ 的均值和方差来确定其分布。容易验证：
$$
\mathbb{E}[x_{n}] = x(0), \quad \mathbb{V}[x_{n}] = n \Delta t
$$
因此，$x_n$ 的分布为：
$$
\color{red}
\boxed{
    x_{n} \sim \mathcal{N}(x(0), n \Delta t)
}
$$

现在，我们将这个结论推广到连续的微分方程上。给定一个总时间 $T$，我们将其离散化为 $N$ 个时间步，则 $\Delta t = T / N$。当 $N \to \infty$ 时，差分方程的解就变成了一个积分：
$$
x(T) - x(0) = \lim_{N \to \infty} \sum_{i = 0}^{N - 1} \Delta w_{i} = \int_{0}^{T} \mathrm{d} w(t) \equiv w(T)
$$
这里的 $w(t)$ 就是 Wiener 过程，而 $\mathrm{d}w$ 是其无穷小增量。利用离散情况的结论，我们可以得到 Wiener 过程在 $T$ 时刻的分布：
$$
\color{red}
\boxed{
    w(T) = \int_{0}^{T} \mathrm{d} w(t) \sim \mathcal{N}(0, T)
}
$$

## 3.4 Ito Calculus

接下来，我们将不严谨地证明 $(\mathrm{d}w)^{2} = \mathrm{d}t$。我们依然从离散情况出发，考察 $(\Delta w)^{2}$ 的均值和方差。已知 $\Delta w \sim \mathcal{N}(0, \Delta t)$，因此：
$$
\mathbb{E}\left[(\Delta w)^{2}\right] = \mathbb{V}[\Delta w] + \mathbb{E}[\Delta w]^{2} = \Delta t
$$
$$
\mathbb{V}\left[(\Delta w)^{2}\right] = \mathbb{E}\left[(\Delta w)^{4}\right] - \mathbb{E}\left[(\Delta w)^{2}\right]^{2} = 3 (\Delta t)^{2} - (\Delta t)^{2} = 2 (\Delta t)^{2}
$$
然后考虑 $N$ 步求和的均值和方差：
$$
\mathbb{E}\left[\sum_{i = 0}^{N - 1}(\Delta w_{i})^{2}\right] = N \Delta t, \quad \mathbb{V}\left[\sum_{i = 0}^{N - 1}(\Delta w_{i})^{2}\right] = 2 N (\Delta t)^{2}
$$
最后，我们将其拓展到连续情况。利用 $\Delta t = T / N$ 的关系，并在极限和期望之间不严谨地交换顺序：
$$
\mathbb{E}\left[\int_{0}^{T} (\mathrm{d} w)^{2}\right] = \mathbb{E}\left[\lim_{N \to \infty} \sum_{i = 0}^{N - 1}(\Delta w_{i})^{2}\right] = \lim_{N \to \infty} N \Delta t = T
$$
$$
\mathbb{V}\left[\int_{0}^{T} (\mathrm{d} w)^{2}\right] = \mathbb{V}\left[\lim_{N \to \infty} \sum_{i = 0}^{N - 1}(\Delta w_{i})^{2}\right] = \lim_{N \to \infty} 2N(\Delta t)^{2} = \lim_{\Delta t \to 0} 2T\Delta t = 0
$$
方差为 0 意味着这个积分的结果在极限情况下是完全确定的，没有任何随机性！
$$
\color{red}
\boxed{
    \int_{0}^{T} (\mathrm{d} w)^{2} = T = \int_{0}^{T} \mathrm{d} t
}
$$
因此我们得到 Ito 规则：
$$
\color{red}
\boxed{
    (\mathrm{d} w)^{2} = \mathrm{d} t
}
$$

## 3.5 Ito's Formula: Changing Variables in an SDE

设随机变量 $x$ 满足某个 SDE：
$$
\mathrm{d}x = f \mathrm{d}t + g \mathrm{d}w
$$
那么 $y = y(x, t)$ 的微分形式如下：
$$
\begin{aligned}
\mathrm{d}y &= \frac{\partial y}{\partial t} \mathrm{d}t + \frac{\partial y}{\partial x} \mathrm{d}x + \frac{1}{2} \frac{\partial^{2} y}{\partial x^{2}} (\mathrm{d}x)^{2} + \mathcal{O}((\mathrm{d}t)^{3/2}) \\
\end{aligned}
$$
将 $\mathrm{d}x$ 的表达式代入，并计算 $(\mathrm{d}x)^{2}$：
$$
(\mathrm{d}x)^{2} = (f \mathrm{d}t + g \mathrm{d}w)^{2} = f^{2}(\mathrm{d}t)^{2} + 2fg(\mathrm{d}t)(\mathrm{d}w) + g^{2}(\mathrm{d}w)^{2}
$$
根据 Ito 规则，我们知道 $(\mathrm{d}w)^{2} = \mathrm{d}t$。而 $(\mathrm{d}t)^{2}$ 和 $\mathrm{d}t \mathrm{d}w$ 都是比 $\mathrm{d}t$ 更高阶的无穷小量，因此在极限中趋于零。所以，我们只保留 $g^{2} \mathrm{d}t$ 这一项。代回到 $\mathrm{d}y$ 的展开式中：
$$
\begin{aligned}
\mathrm{d}y &= \frac{\partial y}{\partial t} \mathrm{d}t + \frac{\partial y}{\partial x} (f \mathrm{d}t + g \mathrm{d}w) + \frac{1}{2} \frac{\partial^{2} y}{\partial x^{2}} (g^{2} \mathrm{d}t) + \mathcal{O}((\mathrm{d}t)^{3/2}) \\
&= \left(\frac{\partial y}{\partial t} + f \frac{\partial y}{\partial x} + \frac{1}{2} g^{2} \frac{\partial^{2} y}{\partial x^{2}}\right)\mathrm{d}t + g \frac{\partial y}{\partial x} \mathrm{d}w + \mathcal{O}((\mathrm{d}t)^{3/2})
\end{aligned}
$$

因此得到 Ito 公式：
$$
\color{red}
\boxed{
    \begin{aligned}
    \mathrm{d}y &= \frac{\partial y}{\partial t} \mathrm{d}t + \frac{\partial y}{\partial x} \mathrm{d}x + \frac{1}{2} \frac{\partial^{2} y}{\partial x^{2}} (\mathrm{d}x)^{2} \\
    &= \left(\frac{\partial y}{\partial t} + f \frac{\partial y}{\partial x} + \frac{1}{2} g^{2} \frac{\partial^{2} y}{\partial x^{2}}\right)\mathrm{d}t + g \frac{\partial y}{\partial x} \mathrm{d}w
    \end{aligned}
}
$$

## 3.6 Solving Some SDEs

极少数 SDE 可以解析求解。

### 3.6.1 The Ornstein-Uhlenbeck Process

Ornstein-Uhlenbeck (OU) 过程对应的 SDE 为：
$$
\mathrm{d}x = -\mu x \mathrm{d}t + \sigma \mathrm{d}w
$$
先求不带噪声项的时候的 ODE 解，即 $x(t) = x(0) \exp(-\mu t)$。我们借鉴常微分方程中的常数变易法，设解的形式为 $x(t) = y(t) \exp(-\mu t)$。为了得到 $y(t)$ 满足的方程，我们对 $y(t) = x(t) \exp(\mu t)$ 应用 Ito 公式：
$$
\begin{aligned}
\mathrm{d}y &= \exp(\mu t) \mathrm{d}x + \mu x \exp(\mu t) \mathrm{d}t + 0 \\
&= \exp(\mu t) (-\mu x \mathrm{d}t + \sigma \mathrm{d}w) + \mu x \exp(\mu t) \mathrm{d}t \\
&= \sigma \exp(\mu t) \mathrm{d}w
\end{aligned}
$$
因此：
$$
y(t) = y(0) + \sigma \int_{0}^{t} \exp(\mu s) \mathrm{d}w(s)
$$
积分项是高斯随机变量的加权和，因此其本身也是一个高斯随机变量。我们可以通过离散化的方式来考察这个积分的均值和方差：
$$
\begin{aligned}
\mathbb{E}\left[\int_{0}^{T} \exp(\mu s) \mathrm{d}w(s)\right] &= \mathbb{E}\left[\lim_{N \to \infty} \sum_{i = 0}^{N - 1} \exp(\mu t_{i}) \Delta w_{i}\right] \\
&= \lim_{N \to \infty} \sum_{i = 0}^{N - 1} \exp(\mu t_{i}) \mathbb{E}[\Delta w_{i}] \\
&= 0
\end{aligned}
$$

$$
\begin{aligned}
\mathbb{V}\left[\int_{0}^{T} \exp(\mu s) \mathrm{d}w(s)\right] &= \mathbb{V}\left[\lim_{N \to \infty} \sum_{i = 0}^{N - 1} \exp(\mu t_{i}) \Delta w_{i}\right] \\
&= \lim_{N \to \infty} \sum_{i = 0}^{N - 1} \exp(2 \mu t_{i}) \mathbb{V}[\Delta w_{i}] \\
&= \lim_{N \to \infty} \sum_{i = 0}^{N - 1} \exp(2 \mu t_{i}) \Delta t \\
&= \int_{0}^{T} \exp(2 \mu s) \mathrm{d}s \\
&= \frac{1}{2 \mu} \left(\exp(2 \mu T) - 1\right)
\end{aligned}
$$
因此：
$$
y(T) \sim \mathcal{N}\left(y_{0}, \frac{\sigma^{2}}{2 \mu} \left(\exp(2 \mu T) - 1\right)\right)
$$
代换回 $x(t)$：
$$
\begin{aligned}
x(t) &= \left(y(0) + \sigma \int_{0}^{t} \exp(\mu s) \mathrm{d}w(s)\right) \exp(-\mu t) \\
&= x(0) \exp(-\mu t) + \sigma \int_{0}^{t} \exp(-\mu (t - s)) \mathrm{d}w(s)
\end{aligned}
$$
如果 $\mu$ 和 $\sigma$ 都是 $t$ 的函数，那么：
$$
x(t) = x_{0} \exp\left(-\int_{0}^{t} \mu(s) \mathrm{d}s\right) + \int_{0}^{t} \exp\left(-\int_{s}^{t} \mu(u) \mathrm{d}u\right) \sigma(s) \mathrm{d}w(s)
$$
解的形式和一阶线性微分方程的解形式类似。

### 3.6.2 Linear SDEs

线性 SDE 的一般形式为：
$$
\mathrm{d}x = -\mu x \mathrm{d}t + \sigma x \mathrm{d}w
$$
令 $y = \ln x$，根据 Ito 公式，SDE变为：
$$
\begin{aligned}
\mathrm{d}y &= \frac{1}{x} \mathrm{d}x + \frac{1}{2} \left(-\frac{1}{x^{2}}\right) (\mathrm{d}x)^{2} \\
&= \frac{1}{x} (-\mu x \mathrm{d}t + \sigma x \mathrm{d}w) - \frac{1}{2x^{2}} (\sigma^{2} x^{2} \mathrm{d}t) \\
&= \left(-\mu - \frac{1}{2} \sigma^{2}\right) \mathrm{d}t + \sigma \mathrm{d}w
\end{aligned}
$$
这个 SDE 可以直接积分，得到：
$$
\begin{aligned}
y(t) &= y(0) + \int_{0}^{t} \left(-\mu - \frac{1}{2} \sigma^{2}\right) \mathrm{d}s + \int_{0}^{t} \sigma \mathrm{d}w(s) \\
&= y(0) - \left(\mu + \frac{1}{2} \sigma^{2}\right) t + \sigma w(t)
\end{aligned}
$$
代换回 $x$，有：
$$
x(t) = x(0) \exp\left(-\left(\mu + \frac{1}{2} \sigma^{2}\right) t + \sigma w(t)\right)
$$

### 3.6.3 Ito Stochastic Integrals

总结来说，形如以下的 Ito 积分：
$$
I(t) = \int_{0}^{t} f(s) \mathrm{d}w(s) = \lim_{N \to \infty} \sum_{n = 0}^{N - 1} f(n \Delta t) \Delta W_{n}
$$
是一个满足高斯分布的随机变量，它的均值和方差分别为：
$$
\mathbb{E}[I(t)] = 0, \quad \mathbb{V}[I(t)] = \int_{0}^{t} f^{2}(s) \mathrm{d}s
$$
这里需要注意的是，Ito 积分的定义要求离散化后，被积函数 $f$ 必须在小区间的**左端点**求值。这里的函数可以是一般的确定性函数，甚至可以是随机过程本身。例如，假设 $f$ 是由 Wiener 过程驱动的函数，那么由 Wiener 过程的性质可知，$\Delta W_{n}$ 和之前的增量是独立的，因此也和在区间开始时取值的 $f(n \Delta t)$ 独立，则
$$
\mathbb{E}\left[f(n \Delta t) \Delta W_{n}\right] = \mathbb{E}\left[f(n \Delta t)\right] \mathbb{E}\left[\Delta W_{n}\right] = 0
$$
因此 $\mathbb{E}[I(t)] = 0$ 始终成立。

## 3.7 Deriving Equations for the Means and Variances

通常，为了得到一个随机过程的矩（如均值、方差），我们需要先求解 SDE 得到其概率密度，再通过积分计算。但很多 SDE 无法解析求解。幸运的是，我们可以绕过这一步，直接从 SDE 推导出关于矩的微分方程。

考虑一般的SDE：
$$
\mathrm{d}x = f(x, t) \mathrm{d}t + g(x, t) \mathrm{d}w
$$
对其两边求期望，可以得到均值的 ODE：
$$
\begin{aligned}
\mathbb{E}[\mathrm{d}x] &= \mathbb{E}[f(x, t) \mathrm{d}t + g(x, t) \mathrm{d}w] \\
\mathrm{d} \mathbb{E}[x] &= \mathbb{E}[f(x, t)] \mathrm{d}t + \mathbb{E}[g(x, t) \mathrm{d}w]
\end{aligned}
$$
由于 $g(x, t)$ 是非前瞻的（non-anticipating），它在 $t$ 时刻的值与未来的增量 $\mathrm{d}w$ 独立，因此 $\mathbb{E}[g(x, t) \mathrm{d}w] = \mathbb{E}[g(x, t)]\mathbb{E}[\mathrm{d}w] = 0$。所以：
$$
\frac{\mathrm{d} \mathbb{E}[x]}{\mathrm{d}t} = \mathbb{E}[f(x, t)]
$$
为了得到二阶矩的 ODE，我们先对 $x^2$ 应用伊藤公式：
$$
\begin{aligned}
\mathrm{d}(x^{2}) &= 2x \mathrm{d}x + (\mathrm{d}x)^{2} \\
&= 2x (f \mathrm{d}t + g \mathrm{d}w) + (g \mathrm{d}w)^{2} \\
&= (2x f + g^{2}) \mathrm{d}t + 2x g \mathrm{d}w
\end{aligned}
$$
再对上式两边求期望：
$$
\begin{aligned}
\mathbb{E}[\mathrm{d}(x^{2})] &= \mathbb{E}[(2x f + g^{2}) \mathrm{d}t] + \mathbb{E}[2x g \mathrm{d}w] \\
\mathrm{d}\mathbb{E}[x^{2}] &= \mathbb{E}[2x f + g^{2}] \mathrm{d}t
\end{aligned}
$$
利用方差的定义 $\mathbb{V}[x] = \mathbb{E}[x^{2}] - (\mathbb{E}[x])^{2}$，我们可以得到其 ODE：
$$
\begin{aligned}
\mathrm{d}\mathbb{V}[x] &= \mathrm{d}\mathbb{E}[x^{2}] - \mathrm{d}((\mathbb{E}[x])^{2}) \\
&= \mathrm{d}\mathbb{E}[x^{2}] - 2 \mathbb{E}[x] \mathrm{d}\mathbb{E}[x] \\
&= \left(\mathbb{E}[2x f + g^{2}] - 2 \mathbb{E}[x] \mathbb{E}[f]\right) \mathrm{d}t
\end{aligned}
$$
求解这些 ODE 即可得到均值和方差的时间演化。但是这个方法不总是有效的。对于非线性的 SDE，$\mathbb{E}[f(x, t)]$ 可能会依赖更高阶的矩，导致一个无法闭合的无限矩方程组。例如，考虑如下的 SDE：
$$
\mathrm{d}x = -x^{3} \mathrm{d}t + g(x, t) \mathrm{d}w
$$
对应的均值的 ODE 为：
$$
\mathrm{d}\mathbb{E}[x] = -\mathbb{E}[x^{3}] \mathrm{d}t
$$
为了求解均值 $\mathbb{E}[x]$，我们必须知道三阶矩 $\mathbb{E}[x^{3}]$；而计算三阶矩，需要进一步计算五阶矩，这就产生了依赖，无法闭合。

## 3.8 Multiple Variables and Multiple Noise Sources

### 3.8.1 SDEs with Multiple Noise Sources

SDE 的噪声源可以是多个的，这些噪声可以独立也可以相关。考虑如下的 SDE：
$$
\mathrm{d}x = f(x, t) \mathrm{d}t + g_{1}(x, t) \mathrm{d}w_{1} + g_{2}(x, t) \mathrm{d}w_{2}
$$
规定任何时候都有：
$$
(\mathrm{d} w_{1})^{2} = (\mathrm{d} w_{2})^{2} = \mathrm{d} t, \quad \mathrm{d} w_{1} \mathrm{d} w_{2} = 0
$$
如果噪声是相关的，设噪声 $v_{1}$ 和 $v_{2}$ 是两个相关的 Wiener 过程，我们有：
$$
\underbrace{\begin{bmatrix}v_{1} \\ v_{2}\end{bmatrix}}_{\mathbf{v}} = \underbrace{\begin{bmatrix}\sqrt{1 - \eta^{2}} & \eta \\ \eta & \sqrt{1 - \eta^{2}}\end{bmatrix}}_{\Eta} \underbrace{\begin{bmatrix}w_{1} \\ w_{2}\end{bmatrix}}_{\mathbf{w}}
$$
其中 $\eta \in [-1, 1]$ 是相关系数，$w_{1}$ 和 $w_{2}$ 是两个独立的 Wiener 过程。则对 $\mathrm{d}v_{1}$ 和 $\mathrm{d}v_{2}$ 有：
$$
\mathrm{d}\mathbf{v} = \Eta \mathrm{d}\mathbf{w}
$$
而且：
$$
\mathrm{d}\mathbf{v} \mathrm{d}\mathbf{v}^{T} = \Eta \mathrm{d}\mathbf{w} \mathrm{d}\mathbf{w}^{T} \Eta^{T} = \Eta (I \mathrm{d}t) \Eta^{T} = \Eta \Eta^{T} \mathrm{d}t
$$

因此原来的 SDE 可以写成：
$$
\begin{aligned}
\mathrm{d}x &= f(x, t) \mathrm{d}t + \begin{bmatrix}g_{1}(x, t) & g_{2}(x, t)\end{bmatrix} \mathrm{d} \mathbf{v} \\
&= f(x, t) \mathrm{d}t + \begin{bmatrix}g_{1}(x, t) & g_{2}(x, t)\end{bmatrix} \Eta \mathrm{d} \mathbf{w} \\
\end{aligned}
$$

### 3.8.2 Ito's Formula for Multiple Variables

向量形式的 Ito 公式为：
$$
\mathrm{d}\mathbf{x} = \mathbf{f}(\mathbf{x}, t) \mathrm{d}t + G(\mathbf{x}, t) \mathrm{d}\mathbf{w}
$$
其中 $\mathbf{x} \in \mathbb{R}^{n}$, $\mathbf{f} : \mathbb{R}^{n} \times \mathbb{R} \to \mathbb{R}^{n}$, $G : \mathbb{R}^{n} \times \mathbb{R} \to \mathbb{R}^{n \times m}$, $\mathbf{w} \in \mathbb{R}^{m}$ 是 $m$ 维独立的 Wiener 过程，且有 $\mathrm{d}\mathbf{w}_{i} \mathrm{d}\mathbf{w}_{j} = \delta_{ij} \mathrm{d}t$。如果 $\mathbf{y} = \mathbf{y}(\mathbf{x}, t)$，则：
$$
\mathrm{d}\mathbf{y}_{i} = \frac{\partial \mathbf{y}_{i}}{\partial t} \mathrm{d}t + \sum_{j = 1}^{n} \frac{\partial \mathbf{y}_{i}}{\partial \mathbf{x}_{j}} \mathrm{d}\mathbf{x}_{j} + \frac{1}{2} \sum_{j = 1}^{n} \sum_{k = 1}^{n} \frac{\partial^{2} \mathbf{y}_{i}}{\partial \mathbf{x}_{j} \partial \mathbf{x}_{k}} \mathrm{d}\mathbf{x}_{j} \mathrm{d}\mathbf{x}_{k} + \cdots
$$
带入得到：
$$
\mathrm{d}\mathbf{y}_{i} = \frac{\partial \mathbf{y}_{i}}{\partial t} \mathrm{d}t + \sum_{j = 1}^{n} \frac{\partial \mathbf{y}_{i}}{\partial \mathbf{x}_{j}} \left(\mathbf{f}_{j} \mathrm{d}t + \sum_{k = 1}^{m} G_{jk} \mathrm{d}\mathbf{w}_{k}\right) + \frac{1}{2} \sum_{j = 1}^{n} \sum_{k = 1}^{n} \frac{\partial^{2} \mathbf{y}_{i}}{\partial \mathbf{x}_{j} \partial \mathbf{x}_{k}} \left(\sum_{l = 1}^{m} G_{jl} G_{kl} \mathrm{d}t\right) + \cdots
$$

---

## Exercises

> 1. By expanding the exponential to second order, show that $\exp(\alpha \mathrm{d}t - (\beta^{2}/2) \mathrm{d}t + \beta \mathrm{d}w) = 1 + \alpha \mathrm{d}t + \beta \mathrm{d}w$ to first order in $\mathrm{d}t$.

$$
\begin{aligned}
\exp\left(\alpha \mathrm{d}t - \frac{1}{2} \beta^{2} \mathrm{d}t + \beta \mathrm{d}w\right) &= 1 + \left(\alpha \mathrm{d}t - \frac{1}{2} \beta^{2} \mathrm{d}t + \beta \mathrm{d}w\right) + \frac{1}{2}\left(\dots + \beta \mathrm{d}w\right)^{2} + \mathcal{O}((\mathrm{d}t)^{3/2}) \\
&= 1 + \alpha \mathrm{d}t - \frac{1}{2} \beta^{2} \mathrm{d}t + \beta \mathrm{d}w + \frac{1}{2}(\beta \mathrm{d}w)^{2} + \mathcal{O}((\mathrm{d}t)^{3/2}) \\
&= 1 + \alpha \mathrm{d}t - \frac{1}{2} \beta^{2} \mathrm{d}t + \beta \mathrm{d}w + \frac{1}{2}\beta^{2} \mathrm{d}t + \mathcal{O}((\mathrm{d}t)^{3/2}) \\
&= 1 + \alpha \mathrm{d}t + \beta \mathrm{d}w + \mathcal{O}((\mathrm{d}t)^{3/2})
\end{aligned}
$$

> 2. If the discrete differential equation for $\Delta x$ is
>    $$
>    \Delta x = c x \Delta t + b \Delta w
>    $$
>    and $x(0) = 0$, calculate the probability density for $x(2 \Delta t)$.

$$
\begin{aligned}
x(0) &= 0 \\
x(\Delta t) &= x(0) + c x(0) \Delta t + b \Delta w_{0} = b \Delta w_{0} \\
x(2 \Delta t) &= x(\Delta t) + c x(\Delta t) \Delta t + b \Delta w_{1} \\
&= b \Delta w_{0} + c b \Delta w_{0} \Delta t + b \Delta w_{1} \\
&= b (1 + c \Delta t) \Delta w_{0} + b \Delta w_{1} \\
&\sim \mathcal{N}\left(0, b^{2} (1 + c \Delta t)^{2} \Delta t + b^{2} \Delta t\right)
\end{aligned}
$$

> 3. If $z(t) = \int_{0}^{t} u^{2} \mathrm{d}w(u)$, and $x = a z + t$, what is the probability density for $x$.

$$
\mathbb{V}[z(t)] = \int_{0}^{t} u^{4} \mathrm{d}u = \frac{t^{5}}{5}
$$
因此：
$$
x(t) \sim \mathcal{N}\left(t, a^{2} \frac{t^{5}}{5}\right)
$$

> 4. $x(t) = \exp(-b (w(t))^{2})$
> (1) What values can $x$ take?
> (2) What is the probability density for $x$?
> (3) Calculate $\mathbb{E}[x^{2}]$ using the probability density of $w$.
> (4) Calculate $\mathbb{E}[x^{2}]$ using the probability density of $x$.

(1) $(w(t))^{2} \in [0, +\infty) \implies x(t) \in (0, 1]$.

(2)
$$
\begin{aligned}
\mathbb{P}(x \leq X) &= \mathbb{P}(\exp(-b (w(t))^{2}) \leq X) \\
&= \mathbb{P}((w(t))^{2} \geq -\frac{1}{b} \ln X) \\
&= \mathbb{P}\left(w(t) \geq \sqrt{-\frac{1}{b} \ln X}\right) + \mathbb{P}\left(w(t) \leq -\sqrt{-\frac{1}{b} \ln X}\right) \\
&= 2 \mathbb{P}\left(w(t) \leq -\sqrt{-\frac{1}{b} \ln X}\right) \\
&= 2 \int_{-\infty}^{-\sqrt{-\frac{1}{b} \ln X}} \frac{1}{\sqrt{2 \pi t}} \exp\left(-\frac{w^{2}}{2t}\right) \mathrm{d}w \\
\end{aligned}
$$

$$
\frac{\mathrm{d}}{\mathrm{d}X} \mathbb{P}(x \leq X) = \frac{1}{\sqrt{2 \pi t}} \frac{1}{b X \sqrt{-\frac{1}{b} \ln X}} \exp\left(\frac{1}{2t} \frac{1}{b} \ln X\right)
$$

(3)

$$
\begin{aligned}
\mathbb{E}[x^{2}] &= \int_{0}^{+\infty} \exp(-2b w^{2}) \frac{1}{\sqrt{2 \pi t}} \exp\left(-\frac{w^{2}}{2t}\right) \mathrm{d}w \\
&= \int_{0}^{+\infty} \frac{1}{\sqrt{2 \pi t}} \exp\left(-\left(2b + \frac{1}{2t}\right) w^{2}\right) \mathrm{d}w \\
&= \frac{1}{\sqrt{2 \pi t}} \sqrt{\pi \cdot \frac{2t}{4 b t + 1}} \\
&= \frac{1}{\sqrt{1 + 4 b t}}
\end{aligned}
$$

(4)

Un-integrable.

> 5. Calculate the expectation value of $\exp(\alpha t + \beta w(t))$.

$$
\begin{aligned}
\mathbb{E}[\exp(\alpha t + \beta w(t))] &= \mathbb{E}[\exp(\alpha t)] \mathbb{E}[\exp(\beta w(t))] \\
&= \exp(\alpha t) \int_{-\infty}^{+\infty} \frac{1}{\sqrt{2 \pi t}} \exp\left(-\frac{w^{2}}{2t}\right) \exp(\beta w) \mathrm{d}w \\
&= \exp(\alpha t) \int_{-\infty}^{+\infty} \frac{1}{\sqrt{2 \pi t}} \exp\left(-\frac{(w - \beta t)^{2}}{2t} + \frac{\beta^{2} t}{2}\right) \mathrm{d}w \\
&= \exp\left(\alpha t + \frac{\beta^{2} t}{2}\right)
\end{aligned}
$$

> 6. Then random variable $x$ and $y$ are given by
>    $$
>    \begin{aligned}
>    x(t) &= a + (w(t))^{2} \\
>    y(t) &= \exp(b w(t))
>    \end{aligned}
>    $$
>    (1) Calculate $\mathbb{E}[x(t)]$.
>    (2) Calculate $\mathbb{E}[y(t)]$.
>    (3) Calculate $\mathbb{E}[x(t) y(t)]$.
>    (4) Are $x(t)$ and $y(t)$ independent?

(1)
$$
\mathbb{E}[x(t)] = a + \mathbb{E}[(w(t))^{2}] = a + t
$$

(2)
$$
\mathbb{E}[y(t)] = \exp\left(\frac{b^{2} t}{2}\right)
$$

(3)
$$
\begin{aligned}
\mathbb{E}[x(t) y(t)] &= \mathbb{E}[(a + (w(t))^{2}) \exp(b w(t))] \\
&= a \mathbb{E}[\exp(b w(t))] + \mathbb{E}[(w(t))^{2} \exp(b w(t))] \\
&= a \exp\left(\frac{b^{2} t}{2}\right) + \int_{-\infty}^{+\infty} \frac{1}{\sqrt{2 \pi t}} w^{2} \exp\left(-\frac{w^{2}}{2t}\right) \exp(b w) \mathrm{d}w \\
&= a \exp\left(\frac{b^{2} t}{2}\right) + \exp\left(\frac{b^{2} t}{2}\right) \int_{-\infty}^{+\infty} \frac{1}{\sqrt{2 \pi t}} w^{2} \exp\left(-\frac{(w - b t)^{2}}{2t}\right) \mathrm{d}w \\
&= a \exp\left(\frac{b^{2} t}{2}\right) + \exp\left(\frac{b^{2} t}{2}\right) \mathbb{E}[\epsilon^{2}] && \epsilon \sim \mathcal{N}(b t, t) \\
&= a \exp\left(\frac{b^{2} t}{2}\right) + \exp\left(\frac{b^{2} t}{2}\right) (t + b^{2} t^{2}) \\
\end{aligned}
$$

(4)

$$
\mathbb{E}[x(t)] \mathbb{E}[y(t)] \neq \mathbb{E}[x(t) y(t)] \implies x(t) \text{ and } y(t) \text{ are not independent.}
$$

> 7. The stochastic process $x$ satisfies the SDE
>    $$
>    \mathrm{d}x = -\mu x \mathrm{d}t + \sigma \mathrm{d}w
>    $$
>    (1) Calculate the ODE of $\mathbb{E}[x]$ and $\mathbb{E}[x^{2}]$.
>    (2) Calculate the ODE of $\mathbb{V}[x]$, and solve it.

(1)
$$
\begin{aligned}
\mathrm{d}\mathbb{E}[x] &= -\mu \mathbb{E}[x] \mathrm{d}t \\
\mathrm{d}\mathbb{E}[x^{2}] &= \left(-2 \mu \mathbb{E}[x^{2}] + \sigma^{2}\right) \mathrm{d}t
\end{aligned}
$$

(2)
$$
\begin{aligned}
\mathrm{d}\mathbb{V}[x] &= \left(-2 \mu \mathbb{E}[x^{2}] + \sigma^{2} - 2 \mu (\mathbb{E}[x])^{2}\right) \mathrm{d}t \\
&= \left(-2 \mu \mathbb{V}[x] + \sigma^{2}\right) \mathrm{d}t \\
\mathbb{V}[x(t)] &= \frac{\sigma^{2}}{2 \mu} \left(1 - \exp(-2 \mu t)\right)
\end{aligned}
$$

> 8. The SDE of $x$ is
>    $$
>    \mathrm{d}x = 3a(x^{1 / 3} - x) \mathrm{d}t + 3 \sqrt{a} x^{2 / 3} \mathrm{d}w
>    $$
>    (1) Transform variable to $y = x^{1 / 3}$, so as to get the SDE of $y$.
>    (2) Solve the equation of $y$.
>    (3) Use the solution of $y$ to get $x$, making sure that you write $x(t)$ in terms of $x_0 \equiv x(0)$.

(1)
$$
\begin{aligned}
\mathrm{d}y &= \frac{1}{3} x^{-2 / 3} \mathrm{d}x -\frac{1}{9} x^{-5 / 3} (\mathrm{d}x)^{2} \\
&= \frac{1}{3} x^{-2 / 3} \left(3a(x^{1 / 3} - x) \mathrm{d}t + 3 \sqrt{a} x^{2 / 3} \mathrm{d}w\right) - \frac{1}{9} x^{-5 / 3} (9 a x^{4 / 3} \mathrm{d}t) \\
&= a (x^{-1 / 3} - x^{1 / 3}) \mathrm{d}t - a x^{-1 / 3} \mathrm{d}t + \sqrt{a} \mathrm{d}w \\
&= -a y \mathrm{d}t + \sqrt{a} \mathrm{d}w
\end{aligned}
$$

(2)
$$
y(t) = y(0) \exp(-a t) + \sqrt{a} \int_{0}^{t} \exp(-a (t - s)) \mathrm{d}w(s)
$$

(3)
$$
x(t) = \left(x(0)^{1 / 3} \exp(-a t) + \sqrt{a} \int_{0}^{t} \exp(-a (t - s)) \mathrm{d}w(s)\right)^{3}
$$

> 9. Solve the SDE
>    $$
>    \mathrm{d}x = -\alpha t^{2} x \mathrm{d}t + \beta \mathrm{d}w
>    $$
>    and calculate $\mathbb{E}[x(t)]$ and $\mathbb{V}[x(t)]$. Hint. To do this, you use essentially the same method as for the Ornstein–Uhlenbeck equation: first solve the differential equation with $\beta = 0$, and this tells you how to make a transformation to a new variable, so that the new variable is constant when $\beta = 0$. The SDE for the new variable is then easy to solve.

$$
\begin{aligned}
\int_{0}^{t} \frac{\mathrm{d}x}{x} &= \int_{0}^{t} -\alpha s^{2} \mathrm{d}s \\
\ln x - \ln x(0) &= -\frac{1}{3} \alpha t^{3} \\
x(t) &= x(0) \exp\left(-\frac{1}{3} \alpha t^{3}\right)
\end{aligned}
$$

Let $y(t) = x(t) \exp\left(\dfrac{1}{3} \alpha t^{3}\right)$, then
$$
\begin{aligned}
\mathrm{d}y &= \exp\left(\frac{1}{3} \alpha t^{3}\right) \mathrm{d}x + x \alpha t^{2} \exp\left(\frac{1}{3} \alpha t^{3}\right) \mathrm{d}t + 0 \\
&= \exp\left(\frac{1}{3} \alpha t^{3}\right) (-\alpha t^{2} x \mathrm{d}t + \beta \mathrm{d}w) + x \alpha t^{2} \exp\left(\frac{1}{3} \alpha t^{3}\right) \mathrm{d}t \\
&= \beta \exp\left(\frac{1}{3} \alpha t^{3}\right) \mathrm{d}w
\end{aligned}
$$

Then
$$
\begin{aligned}
y(t) &= y(0) + \beta \int_{0}^{t} \exp\left(\frac{1}{3} \alpha s^{3}\right) \mathrm{d}w(s) \\
x(t) &= x(0) \exp\left(-\frac{1}{3} \alpha t^{3}\right) + \beta \int_{0}^{t} \exp\left(-\frac{1}{3} \alpha (t^{3} - s^{3})\right) \mathrm{d}w(s)
\end{aligned}
$$

> 10. Linear SDE with time-dependent coefficients: obtain the solution of equation
>     $$
>     \mathrm{d}x = - f(t) x \mathrm{d}t + g(t) x \mathrm{d}w
>     $$

Let $y = \ln x$, then
$$
\begin{aligned}
\mathrm{d}y &= \frac{1}{x} \mathrm{d}x + \frac{1}{2} \left(-\frac{1}{x^{2}}\right) (\mathrm{d}x)^{2} \\
&= \frac{1}{x} \left(-f(t) x \mathrm{d}t + g(t) x \mathrm{d}w\right) - \frac{1}{2} \frac{1}{x^{2}} \left(g^{2}(t) x^{2} \mathrm{d}t\right) \\
&= \left(-f(t) - \frac{1}{2} g^{2}(t)\right) \mathrm{d}t + g(t) \mathrm{d}w
\end{aligned}
$$

Then
$$
\begin{aligned}
y(t) &= y(0) + \int_{0}^{t} \left(-f(s) - \frac{1}{2} g^{2}(s)\right) \mathrm{d}s + \int_{0}^{t} g(s) \mathrm{d}w(s) \\
x(t) &= x(0) \exp\left(-\int_{0}^{t} \left(f(s) + \frac{1}{2} g^{2}(s)\right) \mathrm{d}s + \int_{0}^{t} g(s) \mathrm{d}w(s)\right)
\end{aligned}
$$

> 11. Solve the linear SDE
>     $$
>     \mathrm{d}x = -\mu x \mathrm{d}t + \sigma x \mathrm{d}w
>     $$
>     by first changing variable to $y = \ln x$. Then calculate the probability density for $x(t)$.

Let $y = \ln x$, then
$$
\begin{aligned}
\mathrm{d}y &= \frac{1}{x} \mathrm{d}x + \frac{1}{2} \left(-\frac{1}{x^{2}}\right) (\mathrm{d}x)^{2} \\
&= \frac{1}{x} \left(-\mu x \mathrm{d}t + \sigma x \mathrm{d}w\right) - \frac{1}{2} \frac{1}{x^{2}} \left(\sigma^{2} x^{2} \mathrm{d}t\right) \\
&= \left(-\mu - \frac{1}{2} \sigma^{2}\right) \mathrm{d}t + \sigma \mathrm{d}w
\end{aligned}
$$

Then
$$
\begin{aligned}
y(t) &= y(0) + \int_{0}^{t} \left(-\mu - \frac{1}{2} \sigma^{2}\right) \mathrm{d}s + \int_{0}^{t} \sigma \mathrm{d}w(s) \\
&= y(0) - \left(\mu + \frac{1}{2} \sigma^{2}\right) t + \sigma w(t) \\
x(t) &= x(0) \exp\left(-\left(\mu + \frac{1}{2} \sigma^{2}\right) t + \sigma w(t)\right)
\end{aligned}
$$

The probability density for $x(t)$ is
$$
\begin{aligned}
\mathbb{P}(x(t) \leq X) &= \mathbb{P}\left(x(0) \exp\left(-\left(\mu + \frac{1}{2} \sigma^{2}\right) t + \sigma w(t)\right) \leq X\right) \\
&= \mathbb{P}\left(w(t) \leq \frac{1}{\sigma} \left(\ln\frac{X}{x(0)} + \left(\mu + \frac{1}{2} \sigma^{2}\right) t\right)\right) \\
&= \int_{-\infty}^{\frac{1}{\sigma} \left(\ln\frac{X}{x(0)} + \left(\mu + \frac{1}{2} \sigma^{2}\right) t\right)} \frac{1}{\sqrt{2 \pi t}} \exp\left(-\frac{w^{2}}{2t}\right) \mathrm{d}w \\
\end{aligned}
$$

Then
$$
\frac{\mathrm{d}}{\mathrm{d}X} \mathbb{P}(x(t) \leq X) = \frac{1}{X \sigma \sqrt{2 \pi t}} \exp\left(-\frac{1}{2t} \left(\frac{1}{\sigma} \left(\ln\frac{X}{x(0)} + \left(\mu + \frac{1}{2} \sigma^{2}\right) t\right)\right)^{2}\right)
$$

> 12. Solve the SDE
>     $$
>     \begin{aligned}
>     \mathrm{d}x &= p \mathrm{d}w \\
>     \mathrm{d}p &= -\gamma p \mathrm{d}t
>     \end{aligned}
>     $$

$$
p(t) = p(0) \exp(-\gamma t)
$$

$$
x(t) = x(0) + \int_{0}^{t} p(s) \mathrm{d}w(s) = x(0) + p(0) \int_{0}^{t} \exp(-\gamma s) \mathrm{d}w(s)
$$

> 13. Solve the SDE
>     $$
>     \begin{aligned}
>     \mathrm{d}x &= p \mathrm{d}t + q \mathrm{d}v \\
>     \mathrm{d}p &= -\gamma \mathrm{d}w \\
>     \end{aligned}
>     $$
>     where $v$ and $w$ are independent Wiener processes.

$$
p(t) = p(0) - \gamma w(t) 
$$

$$
\begin{aligned}
x(t) &= x(0) + \int_{0}^{t} (p(0) - \gamma w(s)) \mathrm{d}s + \int_{0}^{t} q \mathrm{d}v(s) \\
&= x(0) + \int_{0}^{t} p(0) \mathrm{d}s - \gamma \int_{0}^{t} w(s) \mathrm{d}s + \int_{0}^{t} q \mathrm{d}v(s) \\
&= x(0) + p(0) t - \gamma \int_{0}^{t} w(s) \mathrm{d}s + q v(t)
\end{aligned}
$$

> 14. Solve the SDE
>     $$
>     \mathrm{d}\mathbf{x} = A \mathbf{x} \mathrm{d}t + B \mathbf{x} \mathrm{d}w
>     $$
>     where $A = \begin{bmatrix}0 & \alpha \\ -\alpha & 0\end{bmatrix}$, $B = \begin{bmatrix}\beta & 0 \\ 0 & \beta\end{bmatrix}$.

$AB = BA$, so
$$
\begin{aligned}
\mathbf{x}(t) &= \exp\left(\left(A - \frac{1}{2} B^{2}\right) t + B w(t)\right) \mathbf{x}(0) \\
&= \exp\left(\begin{bmatrix}-\frac{1}{2} \beta^{2} t & \alpha t \\ -\alpha t & -\frac{1}{2} \beta^{2} t\end{bmatrix} + \begin{bmatrix}\beta w(t) & 0 \\ 0 & \beta w(t)\end{bmatrix}\right) \mathbf{x}(0) \\
&= \exp\left(\begin{bmatrix}-\frac{1}{2} \beta^{2} t + \beta w(t) & \alpha t \\ -\alpha t & -\frac{1}{2} \beta^{2} t + \beta w(t)\end{bmatrix}\right) \mathbf{x}(0) \\
&= \exp\left(-\frac{1}{2} \beta^{2} t + \beta w(t)\right) \begin{bmatrix}\cos(\alpha t) & \sin(\alpha t) \\ -\sin(\alpha t) & \cos(\alpha t)\end{bmatrix} \mathbf{x}(0) \\
\end{aligned}
$$
