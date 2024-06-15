# 2 A Crash Course in Basic Probability Theory

不是从实分析的角度出发，非常不严谨地描述了一些概率论和测度论的基本概念。

## A Basic Definitions

集合系 $\mathcal{U}$ 是由空间 $\Omega$ 的子集构成的集合，即 $\mathcal{U}\subseteq2^{\Omega}$。

$\sigma$ 域是定义在空间 $\Omega$ 上的一个集合系 $\mathcal{U}$，且满足：
1. $\emptyset, \Omega\in\mathcal{U}$.
2. 若 $A\in\mathcal{U}$，则 $A^{c}\in\mathcal{U}$，其中 $A^{c}=\Omega\backslash A$.
3. 若 $A_{1}, A_{2}, \ldots\in\mathcal{U}$，则 $\bigcup_{i=1}^{\infty}A_{i}, \bigcap_{i=1}^{\infty}A_{i}\in\mathcal{U}$.

概率测度是定义在 $\sigma$ 域 $\mathcal{U}$ 上的函数 $P:\mathcal{U}\mapsto[0, 1]$，且满足：
1. $P(\emptyset)=0, P(\Omega)=1$.
2. 若 $A_{1}, A_{2}, \ldots\in\mathcal{U}$，则 $P\left(\bigcup_{i=1}^{\infty}A_{i}\right)\leq\sum_{i=1}^{\infty}P(A_{i})$.
3. 若 $A_{1}, A_{2}, \ldots\in\mathcal{U}$ 两两互斥，则 $P\left(\bigcup_{i=1}^{\infty}A_{i}\right)=\sum_{i=1}^{\infty}P(A_{i})$.

概率空间是一个三元组 $(\Omega, \mathcal{U}, P)$，其中 $\Omega$ 是样本空间，$\mathcal{U}$ 是 $\Omega$ 上的 $\sigma$ 域，$P$ 是定义在 $\mathcal{U}$ 上的概率测度。我们把 $A\in\mathcal{U}$ 称为事件，$\omega\in\Omega$ 称为样本点，$P(A)$ 称为事件 $A$ 的概率。

$n$ 维随机变量是定义在概率空间 $(\Omega, \mathcal{U}, P)$ 上的函数 $X:\Omega\mapsto\mathbb{R}^{n}$，且对于 $\mathbb{R}^{n}$ 上的 Borel 集合 $\mathcal{B}$ 中的每个 $B$，$X^{-1}(B)\in\mathcal{U}$。

指示函数 $\chi_{A}:\Omega\mapsto\{0, 1\}$ 是一个随机变量，对于事件 $A\in\mathcal{U}$，$\chi_{A}(\omega)=1$ 当且仅当 $\omega\in A$。

简单随机变量是有限个实数 $a_{1}, a_{2}, \ldots, a_{n}$ 和指示函数 $\chi_{A_{1}}, \chi_{A_{2}}, \ldots, \chi_{A_{n}}$ 的线性组合，即 $X=\sum_{i=1}^{n}a_{i}\chi_{A_{i}}$。

## B Expectation and Variance

令 $\Omega, \mathcal{U}, P$ 是一个概率空间，$X=\sum_{i=1}^{n}a_{i}\chi_{A_{i}}$ 是一个简单随机变量，$X$ 的积分定义为：
$$
\int_{\Omega}X\mathrm{d}P=\sum_{i=1}^{n}a_{i}P(A_{i}).
$$
$X$ 的期望值定义为：
$$
\mathbb{E}[X]=\int_{\Omega}X\mathrm{d}P.
$$
$X$ 的方差定义为：
$$
\mathrm{Var}[X]=\int_{\Omega}(X-\mathbb{E}[X])^{2}\mathrm{d}P.
$$


