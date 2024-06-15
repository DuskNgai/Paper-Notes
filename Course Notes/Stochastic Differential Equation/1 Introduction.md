# Introduction

## A Motivation

给定一个 SDE：
$$
\begin{cases}
\mathrm{d}\mathbf{X}(t) &= \mathbf{b}(\mathbf{X}(t)) + \mathbf{B}(\mathbf{X}(t))\xi(t) & (t>0) \\
\mathbf{X}(0) &= \mathbf{x}_{0}
\end{cases},
$$

其中 $b:\mathbb{R}^{n}\mapsto\mathbb{R}^{n}$ 是一个给定的光滑向量场，$\mathbf{X}$ 是个 $n$ 维的随机变量，$\mathbf{B}:\mathbb{R}^{n}\mapsto\mathbb{R}^{n\times m}$, $\xi(t)$ 是 $m$ 维“白噪声”。这里就提出了三个问题：
1. 需要严格定义什么是“白噪声”？
2. 这个方程的解 $\mathbf{X}(t)$ 意味着什么？
3. 解的存在性、唯一性、渐进性、关于 $\mathbf{x}_0$、$\mathbf{b}$、$\mathbf{B}$ 的依赖性是什么？ 

## B Some Heuristics

以下的推导不严谨。

设 $m=n$, $\mathbf{x}_{0}=0$, $b\equiv0$, $B\equiv I$，那么方程
$$
\frac{\mathrm{d}\mathbf{X}(t)}{\mathrm{d}t} = \xi(t)
$$
的解是 $n$ 维 Wiener 过程 $\mathbf{W}$，或者 Brown 运动。因此可以定义“白噪声”为 $\xi(t) = \mathrm{d}\mathbf{W}(t)/\mathrm{d}t$，即 Wiener 过程的“导数”。因此，原方程可以写成：
$$
\begin{cases}
\mathrm{d}\mathbf{X}(t) &= \mathbf{b}(\mathbf{X}(t))\mathrm{d}t + \mathbf{B}(\mathbf{X}(t))\mathrm{d}\mathbf{W}(t) \\
\mathbf{X}(0) &= \mathbf{x}_{0}
\end{cases},
$$
即所谓的随机微分方程（SDE），它的解是：
$$
\mathbf{X}(t) = \mathbf{x}_{0} + \int_{0}^{t}\mathbf{b}(\mathbf{X}(s))\mathrm{d}s + \int_{0}^{t}\mathbf{B}(\mathbf{X}(s))\mathrm{d}\mathbf{W}(s).
$$

这里还有好多问题：
1. 什么是 Wiener 过程？
2. 需要定义随机积分 $\int_{0}^{t}\mathbf{B}(\mathbf{X}(s))\mathrm{d}\mathbf{W}(s)$。

以及更深刻的问题：
1. SDE 真的合理地建模了随机的物理运动吗？
2. $\xi(t)$ 真的是白噪声，而不是某种光滑但高度震荡的函数吗？

我们会看到，对于这些问题的不同回答会导出不同的随机微分方程的解。

## C Ito's Formula

考虑一个一维的 SDE：
$$
\mathrm{d}X(t) = b(X(t))\mathrm{d}t + \mathrm{d}W(t).
$$
我们先接受两个结论：
1. 结论一：
    $$
    (\mathrm{d}\mathbf{W}(t))^2 = \mathrm{d}t.
    $$
2. 结论二：
    设 $u:\mathbb{R}\mapsto\mathbb{R}$ 是一个光滑函数，$Y(t) = u(X(t))$，那么 $Y(t)$ 的微分是：
    $$
    \begin{aligned}
    \mathrm{d}Y(t) &= \frac{\mathrm{d}u}{\mathrm{d}X}\mathrm{d}X + \frac{1}{2}\frac{\mathrm{d}^2u}{\mathrm{d}X^2}(\mathrm{d}X)^2 \\
    &= \frac{\mathrm{d}u}{\mathrm{d}X}[b(X(t))\mathrm{d}t + \mathrm{d}W(t)] + \frac{1}{2}\frac{\mathrm{d}^2u}{\mathrm{d}X^2}[b(X(t))\mathrm{d}t + \mathrm{d}W(t)]^2 \\
    &= \left[\frac{\mathrm{d}u}{\mathrm{d}X}b(X(t)) + \frac{1}{2}\frac{\mathrm{d}^2u}{\mathrm{d}X^2}\right]\mathrm{d}t + \frac{\mathrm{d}u}{\mathrm{d}X}\mathrm{d}W(t) + O((\mathrm{d}t)^{3/2})
    \end{aligned}.
    $$
    这里的 $O((\mathrm{d}t)^{3/2})$ 是指高阶项，可以忽略。

因此，我们可以得到 Ito's Formula：
$$
\color{red}
\mathrm{d}Y(t) = \left[\frac{\mathrm{d}u}{\mathrm{d}X}b(X(t)) + \frac{1}{2}\frac{\mathrm{d}^2u}{\mathrm{d}X^2}\right]\mathrm{d}t + \frac{\mathrm{d}u}{\mathrm{d}X}\mathrm{d}W(t)
$$

> 例：以下随机微分方程：
> $$
> \begin{cases}
> \mathrm{d}X(t) &= X(t)\mathrm{d}W(t) \\
> X(0) &= 1
> \end{cases}.
> $$
> 的解是 $X(t) = \exp(W(t) - t/2)$，而不是 $X(t) = \exp(W(t))$。

> 例：定义 $P(t)$ 是 $t\ge0$ 时的随机股票价格，一般建模为：
> $$
> \begin{cases}
> \mathrm{d}P(t) &= \mu P\mathrm{d}t + \sigma P\mathrm{d}W(t) \\
> P(0) &= P_{0}
> \end{cases},
> $$
> 其中 $\mu$ 是股票的平均收益率，也叫漂移率（drift rate），$\sigma$ 是股票的波动率（volatility）。这个模型的解是：
> $$
> P(t) = P_{0}\exp\left(\left(\mu-\frac{1}{2}\sigma^2\right)t + \sigma W(t)\right).
> $$

以上两个例子都可以用 Ito's Formula 来验证。
