# 4 Hyperbolic Equations in One Space Dimension

## 4.1 Characteristics

1D 线性对流方程：
$$
\frac{\partial{u}}{\partial{t}} + a\frac{\partial{u}}{\partial{x}} = 0, \quad u(x, 0) = u^{0}(x)
$$
其特征线是如下 ODE 的解：
$$
\frac{\mathrm{d}x}{\mathrm{d}t} = a(x, t)
$$
因此沿着特征线，解析解为：
$$
\frac{\mathrm{d}u}{\mathrm{d}t} = \frac{\partial{u}}{\partial{t}} + \frac{\mathrm{d}x}{\mathrm{d}t}\frac{\partial{u}}{\partial{x}} = 0
$$
因此把 $u$ 转化为了关于 $t$ 的函数，且沿着特征线，$u$ 保持不变。

举个例子，当 $a$ 是常数时，特征线是线性的：
$$
x - at = \text{constant}
$$
解为：
$$
u(x, t) = u^{0}(x - at)
$$

再举个例子，当 $a$ 是 $u$ 的函数时，因为沿着特征线，$u$ 保持不变。此时的解和上一种情况是一样的：
$$
u(x, t) = u^{0}(x - a(u(x, t))t)
$$

## 4.2 The CFL Condition

用显式差分格式来求解 1D 线性对流方程，有：
$$
\frac{U_{j}^{n + 1} - U_{j}^{n}}{\Delta{t}} + a\frac{U_{j}^{n} - U_{j - 1}^{n}}{\Delta{x}} = 0
$$
令：
$$
\nu = a\frac{\Delta{t}}{\Delta{x}}
$$
则有:
$$
U_{j}^{n + 1} = (1 - \nu)U_{j}^{n} + \nu U_{j - 1}^{n}
$$
在 $(x, t)$ 坐标下，一点的值依赖于它下面和左下的值。这样的依赖关系是递归的，因此产生了 $U_{j}^{n + 1}$ 的依赖域（Domain of Dependence）。

CFL 条件说的是：从一点开始求解方程，最终会到另一个点，其数值解的推进速度不能快于这两个点之间的速度，不然就不能收集到沿途的全部信息。对于 1D 线性对流方程，CFL 条件是:
$$
\nu \leq 1
$$
CFL 是保证收敛的必要条件。当 $a > 0$ 时候，我们需要考虑左下的点；当 $a < 0$ 时候，我们需要考虑右下的点。 因此不如用中心差分统一二者：
$$
\frac{U_{j}^{n + 1} - U_{j}^{n}}{\Delta{t}} + a\frac{U_{j + 1}^{n} - U_{j - 1}^{n}}{2\Delta{x}} = 0
$$

然后考虑稳定性，用 Fourier 分析，带入 $U_{j}^{n} = (\lambda)^{n}\mathrm{e}^{\mathrm{i}k(j\Delta{x})}$，得到：
$$
\lambda = 1 - \nu\mathrm{i}\sin(k\Delta{x})
$$
但 $|\lambda| \le 1$ 恒不成立，在依赖域内都是不稳定的。

---

以下介绍迎风格式（Upwind Scheme）：
$$
U_{j}^{n + 1} = \begin{cases}
U_{j}^{n} - \nu\left(U_{j}^{n} - U_{j - 1}^{n}\right)&a > 0\\
U_{j}^{n} - \nu\left(U_{j + 1}^{n} - U_{j}^{n}\right)&a < 0
\end{cases}
$$

对于 $a > 0$ 的情况，利用 Fourier 分析，得到：
$$
\lambda = 1 - \nu\left(1 - \mathrm{e}^{-\mathrm{i}k\Delta{x}}\right)
$$
则：
$$
|\lambda|^{2} = 1 - 4\nu(1 - \nu)\sin\left(\frac{1}{2}k\Delta{x}\right)^{2}
$$
当 $\nu(1 - \nu) > 0$ 即 $0 < \nu < 1$ 时，$|\lambda|^{2} < 1$，是稳定的。$a < 0$ 的情况同理。

## 4.5 The Lax-Wendroff Scheme

数值迭代的格式可以解释为线性插值，那用更高阶的插值，我们就得到了 Lax-Wendroff 方式。具体来说，它的做法是，先把 $a$ 看作是一个常数，然后把 $u$ 关于 $t$ 做 Taylor 展开：
$$
u(x, t + \Delta{t}) = u(x, t) + \frac{\partial{u}}{\partial{t}}(x, t)\Delta{t} + \frac{1}{2}\frac{\partial^{2}{u}}{\partial{t}^{2}}(x, t)\Delta{t}^{2} + \cdots
$$
已知：
$$
\begin{aligned}
\frac{\partial{u}}{\partial{t}}(x, t) &= -a\frac{\partial{u}}{\partial{x}}(x, t) \\
\frac{\partial^{2}{u}}{\partial{t}^{2}}(x, t) &= -\frac{\partial{a}}{\partial{t}}\frac{\partial{u}}{\partial{x}}(x, t) - a\frac{\partial^{2}{u}}{\partial{x}\partial{t}}(x, t) \\
&= -\frac{\partial{a}}{\partial{t}}\frac{\partial{u}}{\partial{x}}(x, t) - a\left(\frac{\partial^{2}{u}}{\partial{t}\partial{x}}(x, t)\right) \\
&= -\frac{\partial{a}}{\partial{t}}\frac{\partial{u}}{\partial{x}}(x, t) + a\left(\frac{\partial{a}}{\partial{x}}\frac{\partial{u}}{\partial{x}}(x, t) + a\frac{\partial^{2}{u}}{\partial{x}^{2}}(x, t)\right)
\end{aligned}
$$
因此把关于 $t$ 的项用关于 $x$ 的项代替：
$$
\begin{aligned}
U_{j}^{n + 1} &= U_{j}^{n} - \nu\left(\frac{U_{j + 1}^{n} - U_{j - 1}^{n}}{2}\right) \\
&+ \frac{1}{2}(\Delta{t})^{2}\left[-\frac{\partial{a}}{\partial{t}}(x_{j}, t_{n})\left(\frac{U_{j + 1}^{n} - U_{j - 1}^{n}}{2\Delta{x}}\right) + a_{j }^{n}\frac{a_{j + 1 / 2}^{n}(U_{j + 1}^{n} - U_{j}^{n}) - a_{j - 1 / 2}^{n}(U_{j}^{n} - U_{j - 1}^{n})}{(\Delta{x})^{2}}\right]
\end{aligned}
$$

当 $a$ 是常数的时候，就可以化简为：
$$
U_{j}^{n + 1} = U_{j}^{n} - \nu\left(\frac{U_{j + 1}^{n} - U_{j - 1}^{n}}{2}\right) + \frac{\nu^{2}}{2}\left(U_{j + 1}^{n} - 2U_{j}^{n} + U_{j - 1}^{n}\right)
$$
带入 Fourier 分析：
$$
\begin{aligned}
\lambda &= 1 - \frac{\nu}{2}\left(\mathrm{e}^{\mathrm{i}k\Delta{x}} - \mathrm{e}^{-\mathrm{i}k\Delta{x}}\right) + \frac{\nu^{2}}{2}\left(\mathrm{e}^{\mathrm{i}k\Delta{x}} - 2 + \mathrm{e}^{-\mathrm{i}k\Delta{x}}\right) \\
&= 1 - \mathrm{i}\nu\sin(k\Delta{x}) - 2\nu^{2}\sin^{2}\left(\frac{1}{2}k\Delta{x}\right)
\end{aligned}
$$
其模长的平方：
$$
|\lambda|^{2} = 1 - 4\nu^{2}(1 - \nu^{2})\sin^{4}\left(\frac{1}{2}k\Delta{x}\right)
$$
当 $|\nu| \le 1$ 时，$|\lambda|^{2} < 1$，是稳定的。

