# Chapter 7 Fokker-Planck Equation and Reaction Diffusion Systems

## 7.1 Deriving the Fokker-Planck Equation

我们考虑一个由如下 SDE 描述的随机过程 $\mathbf{x}_{t}$：
$$
\mathrm{d}\mathbf{x}_{t} = \mathbf{f}(\mathbf{x}_{t}, t) \mathrm{d}t + G(\mathbf{x}_{t}, t) \mathrm{d}\mathbf{w}_{t}
$$

其中 $\mathbf{x}_{t} \in \mathbb{R}^{n}$ 是系统的状态向量，$\mathbf{f}(\mathbf{x}_{t}, t): \mathbb{R}^{n} \times \mathbb{R} \mapsto \mathbb{R}^{n}$ 是漂移项，描述了系统的确定性演化。$G(\mathbf{x}_{t}, t): \mathbb{R}^{n} \times \mathbb{R} \mapsto \mathbb{R}^{n \times m}$ 是扩散项的系数矩阵，而 $\mathrm{d}\mathbf{w}_{t} \in \mathbb{R}^{m}$ 是一个 $m$ 维独立的 Wiener 过程，满足 $\mathrm{d}\mathbf{w}_{t} \mathrm{d}\mathbf{w}_{t}^{\top} = I \mathrm{d}t$。

我们的目标是推导描述 $\mathbf{x}_t$ 概率密度函数 $\mathbb{P}(\mathbf{x}_{t}, t)$ 演化的 PDE，即 Fokker-Planck 方程。为了实现这一目标，我们首先考虑一个关于 $\mathbf{x}_{t}$ 的任意光滑函数 $u(\mathbf{x}_{t}): \mathbb{R}^{n} \mapsto \mathbb{R}$。根据 Ito 引理，$u$ 的微分 $\mathrm{d}u$ 可以表示为：
$$
\mathrm{d}u = (\nabla_{\mathbf{x}_{t}} u)^{\top} \mathrm{d}\mathbf{x}_{t} + \frac{1}{2} \mathrm{d}\mathbf{x}_{t}^{\top} H_{u} \mathrm{d}\mathbf{x}_{t}
$$

其中 $H_{u}$ 是 $u$ 关于 $\mathbf{x}_{t}$ 的 Hessian 矩阵。我们将 SDE 代入 $\mathrm{d}u$ 的中。首先看第一项：
$$
(\nabla_{\mathbf{x}_{t}} u)^{\top} \mathrm{d}\mathbf{x}_{t} = (\nabla_{\mathbf{x}_{t}} u)^{\top} (\mathbf{f} \mathrm{d}t + G \mathrm{d}\mathbf{w}_{t}) = (\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f} \mathrm{d}t + (\nabla_{\mathbf{x}_{t}} u)^{\top} G \mathrm{d}\mathbf{w}_{t}
$$
然后是第二项。在展开时，我们只保留 $\mathrm{d}t$ 阶的项，因为 $\mathrm{d}t$ 的更高次幂在 Ito 微积分中是小量。具体来说，$(\mathrm{d}t)^{2}$ 阶的项可以忽略，而 $(\mathrm{d}\mathbf{w}_t)^{2}$ 阶的项根据 Ito 规则会变为 $\mathrm{d}t$ 阶。
$$
\begin{aligned}
\frac{1}{2} \mathrm{d}\mathbf{x}_{t}^{\top} H_{u} \mathrm{d}\mathbf{x}_{t} &= \frac{1}{2} (\mathbf{f} \mathrm{d}t + G \mathrm{d}\mathbf{w}_{t})^{\top} H_{u} (\mathbf{f} \mathrm{d}t + G \mathrm{d}\mathbf{w}_{t}) \\
&= \frac{1}{2} ( (G \mathrm{d}\mathbf{w}_{t})^{\top} H_{u} (G \mathrm{d}\mathbf{w}_{t}) + O(\mathrm{d}t) ) \\
&= \frac{1}{2} \mathrm{d}\mathbf{w}_{t}^{\top} G^{\top} H_{u} G \mathrm{d}\mathbf{w}_{t} + O(\mathrm{d}t)
\end{aligned}
$$

由于这是一个标量，它等于其自身的迹。利用迹的循环性质得到：
$$
\begin{aligned}
\mathrm{d}\mathbf{w}_{t}^{\top} G^{\top} H_{u} G \mathrm{d}\mathbf{w}_{t} &= \operatorname{Tr}(\mathrm{d}\mathbf{w}_{t}^{\top} G^{\top} H_{u} G \mathrm{d}\mathbf{w}_{t}) \\
&= \operatorname{Tr}(G^{\top} H_{u} G \mathrm{d}\mathbf{w}_{t} \mathrm{d}\mathbf{w}_{t}^{\top}) \\
&= \operatorname{Tr}(G^{\top} H_{u} G \cdot I \mathrm{d}t) \\
&= \operatorname{Tr}(G^{\top} H_{u} G) \mathrm{d}t
\end{aligned}
$$
将所有项合并，$u$ 的微分方程为：
$$
\mathrm{d}u = \left[(\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f} + \frac{1}{2} \operatorname{Tr}(G^{\top} H_{u} G)\right] \mathrm{d}t + (\nabla_{\mathbf{x}_{t}} u)^{\top} G \mathrm{d}\mathbf{w}_{t}
$$

现在，我们对上式两边取期望。由于 $\mathbb{E}[\mathrm{d}\mathbf{w}_t] = 0$，最后一项的期望为零。因此，$\mathbb{E}[u]$ 的演化满足一个 ODE：
$$
\frac{\mathrm{d}}{\mathrm{d}t} \mathbb{E}[u] = \mathbb{E}\left[(\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f} + \frac{1}{2} \operatorname{Tr}(G^{\top} H_{u} G)\right]
$$

设 $\mathbf{x}_t$ 在 $t$ 时刻的概率密度函数为 $\mathbb{P}(\mathbf{x}_{t}, t)$。我们可以将期望表示为积分形式：
$$
\frac{\mathrm{d}}{\mathrm{d}t} \int_{\mathbb{R}^{n}} u \mathbb{P} \,\mathrm{d}\mathbf{x}_{t} = \int_{\mathbb{R}^{n}} \left[(\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f} + \frac{1}{2} \operatorname{Tr}(G^{\top} H_{u} G)\right] \mathbb{P} \,\mathrm{d}\mathbf{x}_{t}
$$
假设函数足够光滑，左侧可以交换求导和积分的顺序：
$$
\int_{\mathbb{R}^{n}} u \left[\frac{\partial}{\partial t} \mathbb{P}\right] \,\mathrm{d}\mathbf{x}_{t} = \int_{\mathbb{R}^{n}} \left[(\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f} + \frac{1}{2} \operatorname{Tr}(G^{\top} H_{u} G)\right] \mathbb{P} \,\mathrm{d}\mathbf{x}_{t}
$$

为了简化表示，我们引入一个线性微分算子 $\mathcal{L}$，它作用在函数 $u$ 上：
$$
\mathcal{L}u := (\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f} + \frac{1}{2} \operatorname{Tr}(G^{\top} H_{u} G)
$$
利用 $L^{2}$ 空间上的内积 $\langle a, b \rangle = \int a(\mathbf{x}_{t})b(\mathbf{x}_{t})\mathrm{d}\mathbf{x}_{t}$，上面的等式可以简洁地写为：
$$
\left\langle u, \frac{\partial}{\partial t} \mathbb{P}\right\rangle = \left\langle \mathcal{L}u, \mathbb{P}\right\rangle
$$
根据泛函分析的理论，对于任何线性算子 $\mathcal{L}$，必然存在一个伴随算子 $\mathcal{L}^{*}$，对于任意 L2 空间上的函数 $u, v$，它满足如下关系：
$$
\left\langle \mathcal{L}u, v\right\rangle = \left\langle u, \mathcal{L}^{*} v\right\rangle
$$
将这个关系应用到我们的问题中，得到：
$$
\left\langle u, \frac{\partial}{\partial t} \mathbb{P}\right\rangle = \left\langle \mathcal{L} u, \mathbb{P}\right\rangle = \left\langle u, \mathcal{L}^{*} \mathbb{P}\right\rangle
$$

由于这个等式必须对任意的测试函数 $u$ 都成立，根据变分法的基本引理，我们必然得到描述概率密度 $\mathbb{P}$ 演化的方程：
$$
\frac{\partial}{\partial t} \mathbb{P} = \mathcal{L}^{*} \mathbb{P}
$$

接下来的任务，就是通过分部积分的方法，求出伴随算子 $\mathcal{L}^{*}$ 的具体形式。我们将 $\mathcal{L}u$ 分成两部分处理：漂移项 $\mathcal{L}_{\mathbf{f}}u = (\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f}$ 和扩散项 $\mathcal{L}_{G}u = \dfrac{1}{2} \operatorname{Tr}(G^{\top} H_{u} G)$。

---

首先是与漂移项 $\mathbf{f}$ 相关的部分。我们希望将 $\langle (\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f}, \mathbb{P} \rangle$ 转化为 $\langle u, \mathcal{L}_{\mathbf{f}}^{*} \mathbb{P} \rangle$ 的形式。

$$
\begin{aligned}
\left\langle (\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f}, \mathbb{P}\right\rangle &= \int_{\mathbb{R}^{n}} (\nabla_{\mathbf{x}_{t}} u)^{\top} \mathbf{f} \mathbb{P} \,\mathrm{d}\mathbf{x}_{t} \\
&= \int_{\mathbb{R}^{n}} \sum_{i = 1}^n \frac{\partial u}{\partial x_i} \mathbf{f}_i \mathbb{P} \,\mathrm{d}\mathbf{x}_{t}
\end{aligned}
$$
对每一项应用分部积分：
$$
\int_{-\infty}^{\infty} \frac{\partial u}{\partial x_i} (\mathbf{f}_i \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t} = [u (\mathbf{f}_i \mathbb{P})]_{-\infty}^{\infty} - \int u \frac{\partial}{\partial x_i} (\mathbf{f}_i \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t}
$$
由于概率密度函数及其导数在无穷远处为零，边界项消失。因此：
$$
\begin{aligned}
\int_{\mathbb{R}^{n}} \sum_{i = 1}^n \frac{\partial u}{\partial x_i} (\mathbf{f}_i \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t} &= - \int_{\mathbb{R}^{n}} u \sum_{i = 1}^n \frac{\partial}{\partial x_i} (\mathbf{f}_i \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t} \\
&= \left\langle u, - \sum_{i = 1}^n \frac{\partial}{\partial x_i} (\mathbf{f}_i \mathbb{P}) \right\rangle
\end{aligned}
$$
所以，漂移项的伴随算子为：
$$
\mathcal{L}_{\mathbf{f}}^{*} \mathbb{P} = - \sum_{i = 1}^{n} \frac{\partial}{\partial x_{i}} (\mathbf{f}_{i} \mathbb{P})
$$

---

接下来是与扩散项 $G$ 相关的部分。扩散项可以改写为：
$$
\frac{1}{2} \operatorname{Tr}(G^{\top} H_{u} G) = \frac{1}{2} \operatorname{Tr}(H_{u} G G^{\top}) = \frac{1}{2} \sum_{i = 1}^{n} \sum_{j = 1}^{n} \frac{\partial^{2} u}{\partial x_i \partial x_j} [GG^{\top}]_{ij}
$$

我们希望将 $\langle \mathcal{L}_{G} u, \mathbb{P} \rangle$ 转化为 $\langle u, \mathcal{L}_{G}^{*} \mathbb{P} \rangle$ 的形式。

$$
\begin{aligned}
\left\langle \mathcal{L}_{G} u, \mathbb{P}\right\rangle &= \frac{1}{2} \int_{\mathbb{R}^{n}} \sum_{i = 1}^{n} \sum_{j = 1}^{n} \frac{\partial^{2} u}{\partial x_{i} \partial x_{j}} [GG^{\top}]_{ij} \mathbb{P} \,\mathrm{d}\mathbf{x}_{t} \\
\end{aligned}
$$
对每一项应用两次分部积分。第一次分部积分：
$$
\int_{\mathbb{R}^{n}} \frac{\partial^{2} u}{\partial x_{i} \partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t} = \left[\frac{\partial u}{\partial x_{i}} ([GG^{\top}]_{ij} \mathbb{P})\right]_{-\infty}^{\infty} - \int_{\mathbb{R}^{n}} \frac{\partial u}{\partial x_{i}} \frac{\partial}{\partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t}
$$
边界项为零。继续第二次分部积分：
$$
-\int_{\mathbb{R}^{n}} \frac{\partial u}{\partial x_{i}} \frac{\partial}{\partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t} = - \left[u \frac{\partial}{\partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P})\right]_{-\infty}^{\infty} + \int_{\mathbb{R}^{n}} u \frac{\partial^{2}}{\partial x_{i} \partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t}
$$
边界项再次为零。将所有项加起来：
$$
\begin{aligned}
\left\langle \mathcal{L}_{G} u, \mathbb{P}\right\rangle &= \frac{1}{2} \int_{\mathbb{R}^{n}} u \sum_{i = 1}^{n} \sum_{j = 1}^{n} \frac{\partial^{2}}{\partial x_{i} \partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P}) \,\mathrm{d}\mathbf{x}_{t} \\
&= \left\langle u, \frac{1}{2} \sum_{i = 1}^{n} \sum_{j = 1}^{n} \frac{\partial^{2}}{\partial x_{i} \partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P}) \right\rangle
\end{aligned}
$$
所以，扩散项的伴随算子为：
$$
\mathcal{L}_{G}^{*} \mathbb{P} = \frac{1}{2} \sum_{i = 1}^{n} \sum_{j = 1}^{n} \frac{\partial^{2}}{\partial x_{i} \partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P})
$$

---

合并漂移项和扩散项的伴随算子，我们得到完整的伴随算子 $\mathcal{L}^{*} \mathbb{P}$：

$$
\mathcal{L}^{*} \mathbb{P} = - \sum_{i = 1}^{n} \frac{\partial}{\partial x_{i}} (\mathbf{f}_{i} \mathbb{P}) + \frac{1}{2} \sum_{i = 1}^{n} \sum_{j = 1}^{n} \frac{\partial^{2}}{\partial x_{i} \partial x_{j}} ([GG^{\top}]_{ij} \mathbb{P})
$$

因此，描述概率密度函数 $\mathbb{P}(\mathbf{x}_{t}, t)$ 演化的 Fokker-Planck 方程为：

$$
\frac{\partial}{\partial t} \mathbb{P}(\mathbf{x}_{t}, t) = - \sum_{i = 1}^{n} \frac{\partial}{\partial x_{i}} (\mathbf{f}_{i}(\mathbf{x}_{t}, t) \mathbb{P}(\mathbf{x}_{t}, t)) + \frac{1}{2} \sum_{i = 1}^{n} \sum_{j = 1}^{n} \frac{\partial^{2}}{\partial x_{i} \partial x_{j}} ([GG^{\top}]_{ij}(\mathbf{x}_{t}, t) \mathbb{P}(\mathbf{x}_{t}, t))
$$

对于 1D 的 SDE：
$$
\mathrm{d}x = f(x, t) \mathrm{d}t + g(x, t) \mathrm{d}w
$$
其 Fokker-Planck 方程为：
$$
\frac{\partial}{\partial t} \mathbb{P}(x, t) = - \frac{\partial}{\partial x} [f(x, t) \mathbb{P}(x, t)] + \frac{1}{2} \frac{\partial^{2}}{\partial x^{2}} [g^{2}(x, t) \mathbb{P}(x, t)]
$$

## 7.2 The Probability Current

对于单变量的 FP 方程，定义其概率流量为：
$$
J(x, t) = f(x, t) \mathbb{P}(x, t) - \frac{1}{2} \frac{\partial}{\partial x} [g^{2}(x, t) \mathbb{P}(x, t)]
$$
$J(x, t)$ 描述了在固定时间 $t$ 时刻，通过位置 $x$ 的概率净流量。如果 $J(x, t)$ 为正，意味着概率正在向 $x$ 增大的方向流动；如果为负，则向 $x$ 减小的方向流动。

## 7.4 Stationary Solution for One Dimension

对于一维的 Fokker-Planck 方程，我们考虑其在稳态情况下的解。所谓稳态，是指概率密度函数 $\mathbb{P}(x, t)$ 不随时间变化，即 $\partial \mathbb{P} / \partial t = 0$。在这种情况下，Fokker-Planck 方程简化为：
$$
\frac{\partial}{\partial t} \mathbb{P}(x, t) = - \frac{\partial}{\partial x} J(x, t) = 0, \quad x \in [a, b]
$$
这意味着概率流 $J(x, t)$ 在整个空间区间 $[a, b]$ 内必须是一个常数。
$$
\frac{\partial}{\partial x} J(x, t) = 0 \implies J(x, t) = C
$$
同时，由于稳态，$f$ 和 $g$ 也不随时间变化。

---

如果边界条件是反射的，即在边界处 $J = 0$，这意味着在区域 $[a, b]$ 的边界处没有概率流出或流入，即 $J(a) = 0$ 和 $J(b) = 0$。由于 $J(x)$ 在稳态下是常数，那么这个常数 $C$ 必须为零。
$$
\frac{\partial}{\partial x} [g^{2}(x) \mathbb{P}(x)] = 2 f(x) \mathbb{P}(x)
$$
解得：
$$
\mathbb{P}(x) = \frac{1}{Z} \frac{1}{g^{2}(x)} \exp\left(\int_{a}^{x} \frac{2f(u)}{g^{2}(u)} \,\mathrm{d}u\right)
$$
其中 $Z = \int_{a}^{b} \mathbb{P}(x) \,\mathrm{d}x$ 为归一化系数。

---

如果边界条件是周期性的，这意味着 $J(a) = J(b)$。周期性边界条件允许存在一个非零的常数概率流 $J_{0}$。因此，稳态的概率流为：
$$
J(x) = J_{0} \implies \frac{\partial}{\partial x} [g^{2}(x) \mathbb{P}(x)] = 2 f(x) \mathbb{P}(x) - 2 J_{0}
$$
设 $\phi(x) = g^{2}(x) \mathbb{P}(x)$：
$$
\frac{\partial}{\partial x} \phi(x) = \frac{2 f(x)}{g^{2}(x)} \phi(x) - 2 J_{0}
$$
带入求解公式得到：
$$
\mathbb{P}(x) = \frac{1}{g^{2}(x)} \left[g^{2}(a) \mathbb{P}(a) \exp \left(\int_{a}^{x} \frac{2 f(u)}{g^{2}(u)} \,\mathrm{d} u \right) - 2 J_{0} \int_{a}^{x} \exp \left(\int_{u}^{x} \frac{2 f(v)}{g^{2}(v)} \,\mathrm{d} v \right) \mathrm{d} u\right]
$$
定义；
$$
h(x) = \exp \left(\int_{a}^{x} \frac{2 f(u)}{g^{2}(u)} \,\mathrm{d} u \right)
$$
用来化简上式，得到：
$$
\mathbb{P}(x) = \frac{h(x)}{g^{2}(x)} \left[\mathbb{P}(a) \frac{g^{2}(a)}{h(a)} - 2 J_{0} \int_{a}^{x} \frac{1}{h(u)} \,\mathrm{d} u\right]
$$

利用：
$$
\mathbb{P}(b) = \frac{h(b)}{g^{2}(b)} \left[\mathbb{P}(a) \frac{g^{2}(a)}{h(a)} - 2 J_{0} \int_{a}^{b} \frac{1}{h(u)} \,\mathrm{d} u\right] = \mathbb{P}(a)
$$
得到 $J_{0}$：
$$
J_{0} = \frac{\mathbb{P}(a)}{2 \int_{a}^{b} \frac{1}{h(u)} \,\mathrm{d} u} \left[\frac{g^{2}(a)}{h(a)} - \frac{g^{2}(b)}{h(b)}\right]
$$
因此，不带 $J_{0}$ 的稳态概率流为：
$$
\mathbb{P}(x) = \mathbb{P}(a) \frac{\frac{g^{2}(b)}{h(b)} \int_{a}^{x} \frac{1}{h(u)} \,\mathrm{d} u - \frac{g^{2}(a)}{h(a)} \int_{x}^{b} \frac{1}{h(u)} \,\mathrm{d} u}{\frac{g^{2}(x)}{h(x)} \int_{a}^{b} \frac{1}{h(u)} \,\mathrm{d} u}
$$

