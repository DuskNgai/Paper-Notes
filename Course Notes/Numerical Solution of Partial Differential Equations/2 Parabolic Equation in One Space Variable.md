# 2 Parabolic Equations in One Space Variable

## 2.2 A Model Problem

求解 1 维热传导方程
$$
\begin{aligned}
u_{t} &= u_{xx} \quad &&\text{in} \quad \Omega = (0, 1) \times (0, T) \\
u(0, t) &= u(1, t) = 0 \quad &&\text{for} \quad t \in (0, T) \\
u(x, 0) &= u^{0}(x) \quad &&\text{for} \quad x \in (0, 1)
\end{aligned}
$$

## 2.3 Series Approximation

分离变量法将 $u(x, t) = X(x)T(t)$ 代入方程，得到
$$
\frac{\dot{T}}{T} = \frac{\ddot{X}}{X} = -\lambda^{2}
$$
其中 $\lambda$ 为常数。分别解得
$$
\begin{aligned}
X(x) &= \sin(n\pi x) \\
T(t) &= e^{-n^2\pi^2 t}
\end{aligned}
$$
所以通解为
$$
u(x, t) = \sum_{n=1}^{\infty} b_n \sin(n\pi x) e^{-n^2\pi^2 t}
$$
其中 $b_n$ 为待定系数。将初始条件代入通解，得到
$$
u^{0}(x) = \sum_{n=1}^{\infty} b_n \sin(n\pi x)
$$
两边同时乘以 $\sin(m\pi x)$ 并在 $(0, 1)$ 上积分，得到
$$
b_m = 2 \int_{0}^{1} u^{0}(x) \sin(m\pi x) \, dx
$$
所以
$$
u(x, t) = \sum_{n=1}^{\infty} 2 \int_{0}^{1} u^{0}(x) \sin(n\pi x) \sin(n\pi x) e^{-n^2\pi^2 t} \, dx
$$

## 2.4 An Explicit Scheme for the Model Problem

用显式差分格式求解热传导方程
$$
\begin{aligned}
\frac{U_{i}^{n+1} - U_{i}^{n}}{\Delta t} &= \frac{U_{i+1}^{n} - 2U_{i}^{n} + U_{i-1}^{n}}{(\Delta x)^2} \\
U_{0}^{n} &= U_{I}^{n} = 0
\end{aligned}
$$
其中 $U_{i}^{n} \approx u(i\Delta x, n\Delta t)$，$i \in \{0, 1, \cdots, I\}$，$n \in \{0, 1, \cdots, N\}$，$\Delta x = \dfrac{1}{I}$，$\Delta t = \dfrac{T}{N}$。因此有
$$
U_{i}^{n+1} = U_{i}^{n} + \mu (U_{i+1}^{n} - 2U_{i}^{n} + U_{i-1}^{n})
$$
其中
$$
\mu = \dfrac{\Delta t}{(\Delta x)^2}
$$

转换为矩阵的形式，得到
$$
\begin{aligned}
\begin{bmatrix}
1 - 2\mu & \mu & 0 & \cdots & 0 \\
\mu & 1 - 2\mu & \mu & \cdots & 0 \\
0 & \mu & 1 - 2\mu & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \mu & 1 - 2\mu
\end{bmatrix}
\begin{bmatrix}
U_{1}^{n} \\
U_{2}^{n} \\
U_{3}^{n} \\
\vdots \\
U_{I-1}^{n}
\end{bmatrix}
&=
\begin{bmatrix}
U_{1}^{n+1} \\
U_{2}^{n+1} \\
U_{3}^{n+1} \\
\vdots \\
U_{I-1}^{n+1}
\end{bmatrix}
\end{aligned}
$$

## 2.5 Difference Notation and Truncation Error

定义前向差分算子
$$
\begin{aligned}
\Delta_{+t} v(x, t) &= v(x, t + \Delta t) - v(x, t) \\
\Delta_{+x} v(x, t) &= v(x + \Delta x, t) - v(x, t)
\end{aligned}
$$
定义后向差分算子
$$
\begin{aligned}
\Delta_{-t} v(x, t) &= v(x, t) - v(x, t - \Delta t) \\
\Delta_{-x} v(x, t) &= v(x, t) - v(x - \Delta x, t)
\end{aligned}
$$
定义中心差分算子
$$
\begin{aligned}
\delta_{t} v(x, t) &= v\left(x, t + \frac{1}{2}\Delta t\right) - v\left(x, t - \frac{1}{2}\Delta t\right) \\
\delta_{x} v(x, t) &= v\left(x + \frac{1}{2}\Delta x, t\right) - v\left(x - \frac{1}{2}\Delta x, t\right)
\end{aligned}
$$
定义二阶中心差分算子
$$
\delta_{x}^{2} v(x, t) = v(x + \Delta x, t) - 2v(x, t) + v(x - \Delta x, t)
$$

用 Taylor 展开分析显式差分格式的截断误差。用前向差分算子表示，有
$$
\begin{aligned}
\Delta_{+t} u(x, t) &= u_{t}\Delta t + \frac{1}{2} u_{tt} (\Delta t)^{2} + \cdots \\
\delta_{x}^{2} u(x, t) &= u_{xx} (\Delta x)^2 + \frac{1}{12} u_{xxxx} (\Delta x)^{4} + \cdots
\end{aligned}
$$

定义截断误差为
$$
\begin{aligned}
T(x, t) &= \frac{\Delta_{+t} u(x, t)}{\Delta t} - \frac{\delta_{x}^{2} u(x, t)}{(\Delta x)^2} \\
&= u_{t} + \frac{1}{2} u_{tt} \Delta t + \cdots - u_{xx} + \frac{1}{12} u_{xxxx} (\Delta x)^{2} + \cdots \\
&= \underbrace{(u_{t} - u_{xx})}_{0} + \frac{1}{2} u_{tt} \Delta t - \frac{1}{12} u_{xxxx} (\Delta x)^{2} + \cdots
\end{aligned}
$$

现在讨论其绝对值上界。在 Taylor 展开中，有
$$
\Delta_{+t} u(x, t) = u_{t}\Delta t + \frac{1}{2} u_{tt}(x, \eta)(\Delta t)^{2}
$$
其中 $\eta \in (t, t + \Delta t)$，$\xi \in (x - \Delta x, x + \Delta x)$。将它们代入截断误差公式，得到
$$
T(x, t) = \frac{1}{2} u_{tt}(x, \eta) \Delta t + \frac{1}{12} u_{xxxx}(\xi, t) (\Delta x)^{2}
$$
因此截断误差的绝对值上界为
$$
\begin{aligned}
|T(x, t)| &\leq \frac{1}{2} M_{tt} \Delta t + \frac{1}{12} M_{xxxx} (\Delta x)^2 \\
&= \frac{1}{2} \Delta t \left[M_{tt} + \frac{1}{6\mu} M_{xxxx}\right] \\
\end{aligned}
$$
其中 $M_{tt}$ 是 $|u_{tt}(x, t)|$ 的上界，$M_{xxxx}$ 是 $|u_{xxxx}(x, t)|$ 的上界，其中 $(x, t) \in [0, 1] \times [0, t_{F}]$，$t_{F}$ 是最终时间。

## 2.6 Convergence of the Explicit Scheme

定义截断误差的最大值为 $\bar{T} = \max_{x, t} |T(x, t)|$，定义 $T^{n}_{i} = T(i\Delta x, n\Delta t)$。

定义误差为 $e_{i}^{n} = U_{i}^{n} - u(i\Delta x, n\Delta t)$。考虑误差在时间上的传播，带入 $U_{i}^{n+1}$ 和 $T(i\Delta x, n\Delta t)$ 的表达式，有
$$
\begin{aligned}
e_{i}^{n+1} &= U_{i}^{n+1} - u(i\Delta x, (n+1)\Delta t) \\
&= U_{i}^{n} + \mu (U_{i+1}^{n} - 2U_{i}^{n} + U_{i-1}^{n}) - \Delta t(T(i\Delta x, n\Delta t) + u(i\Delta x, n\Delta t) + \mu(u(i\Delta x + \Delta x, n\Delta t) - 2u(i\Delta x, n\Delta t) + u(i\Delta x - \Delta x, n\Delta t))) \\
&= e_{i}^{n} + \mu (e_{i+1}^{n} - 2e_{i}^{n} + e_{i-1}^{n}) - \Delta t T^{n}_{i} \\
&= (1 - 2\mu) e_{i}^{n} + \mu (e_{i+1}^{n} + e_{i-1}^{n}) - \Delta t T^{n}_{i}
\end{aligned}
$$

定义 $E^{n} = \max_{i} |e_{i}^{n}|$，考虑 $e_{i}^{n+1}$ 的绝对值上界，有
$$
\begin{aligned}
|e_{i}^{n+1}| &\leq (1 - 2\mu) |E^{n}| + \mu (|E^{n}| + |E^{n}|) + \Delta t |T(i\Delta x, n\Delta t)| \\
&\leq |E^{n}| + \Delta t \bar{T}
\end{aligned}
$$

我们有 $E^{0} = 0$，所以有
$$
E^{n} \leq n \Delta t \bar{T}
$$

## 2.7 Fourier Analysis of the Error

令
$$
U_{i}^{n} = (\lambda(k))^{n} \mathrm{e}^{\mathrm{i}k(i\Delta x)}
$$
带入显式差分格式，有
$$
\begin{aligned}
(\lambda(k)) &= 1 + \mu (\mathrm{e}^{\mathrm{i}k\Delta x} - 2 + \mathrm{e}^{-\mathrm{i}k\Delta x}) \\
&= 1 + 2\mu (\cos(k\Delta x) - 1) \\
&= 1 - 4\mu \sin^2\left(\frac{1}{2}k\Delta x\right)
\end{aligned}
$$
因此称 $\lambda(k)$ 为差分格式的放大因子。

要求 $|\lambda(k)| \leq 1$，有
$$
\begin{aligned}
|\lambda(k)| &\leq 1 \\
|1 - 4\mu \sin^2\left(\frac{1}{2}k\Delta x\right)| &\leq 1 \\
4\mu \sin^2\left(\frac{1}{2}k\Delta x\right) &\leq 2 \\
\mu \sin^2\left(\frac{1}{2}k\Delta x\right) &\leq \frac{1}{2} \\
\mu &\leq \frac{1}{2}
\end{aligned}
$$

## 2.8 An Implicit Method

用隐式差分格式求解热传导方程
$$
\Delta_{-t} U_{i}^{n + 1} = \mu \delta_{x}^{2} U_{i}^{n + 1}
$$

把未知的 $U_{*}^{n  +1}$ 提到左边，转换为矩阵的形式，得到
$$
\begin{aligned}
\begin{bmatrix}
1 + 2\mu & -\mu & 0 & \cdots & 0 \\
-\mu & 1 + 2\mu & -\mu & \cdots & 0 \\
0 & -\mu & 1 + 2\mu & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & -\mu & 1 + 2\mu
\end{bmatrix}^{-1}
\begin{bmatrix}
U_{1}^{n} \\
U_{2}^{n} \\
U_{3}^{n} \\
\vdots \\
U_{I-1}^{n}
\end{bmatrix}
&=
\begin{bmatrix}
U_{1}^{n+1} \\
U_{2}^{n+1} \\
U_{3}^{n+1} \\
\vdots \\
U_{I-1}^{n+1}
\end{bmatrix}
\end{aligned}
$$

## 2.9 The Thomas Algorithm

还是令 $U_{i}^{n} = (\lambda(k))^{n} \mathrm{e}^{\mathrm{i}k(i\Delta x)}$，带入隐式差分格式，有
$$
\begin{aligned}
1 &= -\mu (\lambda(k)) \mathrm{e}^{\mathrm{i}k\Delta x} + (1 + 2\mu) (\lambda(k)) - \mu (\lambda(k)) \mathrm{e}^{-\mathrm{i}k\Delta x} \\
\lambda(k) &= \frac{1}{1 + 4\mu \sin^2\left(\frac{k}{2}\Delta x\right)}
\end{aligned}
$$

恒定有 $|\lambda(k)| \leq 1$，因此隐式差分格式是无条件稳定的。

## 2.9 The Weighted Average or $\theta$-method

显式差分格式
$$
\Delta_{+t} U_{i}^{n} = \mu \delta_{x}^{2} U_{i}^{n}
$$
隐式差分格式
$$
\Delta_{+t} U_{i}^{n+1} = \mu \delta_{x}^{2} U_{i}^{n+1}
$$
加权平均法
$$
\Delta_{+t} U_{i}^{n+1} = \mu [\theta \delta_{x}^{2} U_{i}^{n+1} + (1 - \theta) \delta_{x}^{2} U_{i}^{n}]
$$
即
$$
-\theta\mu U_{i+1}^{n+1} + (1 + 2\theta\mu) U_{i}^{n+1} - \theta\mu U_{i-1}^{n+1} = [1 + (1 - \theta)\mu\delta_{x}^{2}] U_{i}^{n}
$$
其中 $\theta \in [0, 1]$。当 $\theta = 1 / 2$ 时，有
$$
\left(1 - \frac{1}{2}\mu\delta_{x}^{2}\right) U_{i}^{n+1} = \left(1 + \frac{1}{2}\mu\delta_{x}^{2}\right) U_{i}^{n}
$$
写开来就是：
$$
\begin{bmatrix}
1 + \mu & -\frac{1}{2}\mu & 0 & \cdots & 0 \\
-\frac{1}{2}\mu & 1 + \mu & -\frac{1}{2}\mu & \cdots & 0 \\
0 & -\frac{1}{2}\mu & 1 + \mu & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & -\frac{1}{2}\mu & 1 + \mu
\end{bmatrix}
\begin{bmatrix}
U_{1}^{n + 1} \\
U_{2}^{n + 1} \\
U_{3}^{n + 1} \\
\vdots \\
U_{I-1}^{n + 1}
\end{bmatrix} = 
\begin{bmatrix}
1 - \mu & \frac{1}{2}\mu & 0 & \cdots & 0 \\
\frac{1}{2}\mu & 1 - \mu & \frac{1}{2}\mu & \cdots & 0 \\
0 & \frac{1}{2}\mu & 1 - \mu & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & 0 & \frac{1}{2}\mu & 1 - \mu
\end{bmatrix}
\begin{bmatrix}
U_{1}^{n} \\
U_{2}^{n} \\
U_{3}^{n} \\
\vdots \\
U_{I-1}^{n}
\end{bmatrix}
$$

---

带入 $U_{i}^{n} = (\lambda(k))^{n} \mathrm{e}^{\mathrm{i}k(i\Delta x)}$，有
$$
\begin{aligned}
-\theta\mu \mathrm{e}^{\mathrm{i}k\Delta x} (\lambda(k)) + (1 + 2\theta\mu) (\lambda(k)) - \theta\mu \mathrm{e}^{-\mathrm{i}k\Delta x} (\lambda(k)) &= (1 - \theta)\mu \mathrm{e}^{\mathrm{i}k\Delta x} + (1 - 2(1 - \theta)\mu) + (1 - \theta)\mu \mathrm{e}^{-\mathrm{i}k\Delta x} \\
\lambda(k) &= \frac{1 - 4(1 - \theta)\mu \sin^2\left(\frac{1}{2}k\Delta x\right)}{1 + 4\theta\mu \sin^2\left(\frac{1}{2}k\Delta x\right)}
\end{aligned}
$$

$\lambda(k)$ 显然小于 1，但是大于 -1
$$
\begin{aligned}
1 - 4(1 - \theta)\mu \sin^2\left(\frac{1}{2}k\Delta x\right) &> -1 - 4\theta\mu \sin^2\left(\frac{1}{2}k\Delta x\right) \\
\mu(1 - 2\theta) &> \frac{1}{2} \\
\end{aligned}
$$
因此 $\theta \in \left(\dfrac{1}{2}, 1\right)$ 时，加权平均法是无条件稳定的，$\theta \in \left[0, \dfrac{1}{2}\right)$ 时，$\mu \leq \dfrac{1}{2(1 - 2\theta)}$ 时是稳定的。

以下分析截断误差。对显示差分格式，有
$$
u(x, t) = \left[u - \frac{1}{2}\Delta t \frac{\partial u}{\partial t} + \frac{1}{2}\frac{1}{2}\Delta t^2 \frac{\partial^2 u}{\partial t^2} - \frac{1}{2}\frac{1}{6}\Delta t^3 \frac{\partial^3 u}{\partial t^3} + \cdots\right]\left(x + \frac{1}{2}\Delta x\right)
$$
对隐式差分格式，有
$$
u(x, t + \Delta t) = \left[u + \frac{1}{2}\Delta t \frac{\partial u}{\partial t} + \frac{1}{2}\frac{1}{2}\Delta t^2 \frac{\partial^2 u}{\partial t^2} + \frac{1}{2}\frac{1}{6}\Delta t^3 \frac{\partial^3 u}{\partial t^3} + \cdots\right]\left(x + \frac{1}{2}\Delta x\right)
$$
隐式差分格式减去显示差分格式，有
$$
\begin{aligned}
u(x, t + \Delta t) - u(x, t) &= \left[\Delta t \frac{\partial u}{\partial t} + \frac{1}{6}\Delta t^3 \frac{\partial^3 u}{\partial t^3} + \cdots\right]\left(x + \frac{1}{2}\Delta x\right) \\
\end{aligned}
$$
这是在时间上的截断误差，在空间上的截断误差是
$$
u(x + \Delta x, t + \Delta t) - 2u(x, t + \Delta t) + u(x - \Delta x, t + \Delta t) = \left[\Delta x^2 \frac{\partial^2 u}{\partial x^2} + \frac{1}{12}\Delta x^4 \frac{\partial^4 u}{\partial x^4} + \cdots\right](x, t + \Delta t)
$$
空间上的截断误差再在时间上展开，有
$$
\begin{aligned}
&u(x + \Delta x, t + \Delta t) - 2u(x, t + \Delta t) + u(x - \Delta x, t + \Delta t) \\
&= \left[\Delta x^2 \frac{\partial^2 u}{\partial x^2} + \frac{1}{12}\Delta x^4 \frac{\partial^4 u}{\partial x^4} + \cdots\right](x, t) \\
&+ \Delta t \left[\frac{\partial}{\partial t}\left(\Delta x^2 \frac{\partial^2 u}{\partial x^2} + \frac{1}{12}\Delta x^4 \frac{\partial^4 u}{\partial x^4} + \cdots\right) + \frac{1}{2}\frac{\partial^2}{\partial t^2}\left(\Delta x^2 \frac{\partial^2 u}{\partial x^2} + \frac{1}{12}\Delta x^4 \frac{\partial^4 u}{\partial x^4} + \cdots\right) + \cdots\right](x, t) \\
&+ \cdots
\end{aligned}
$$

和 $(x + \Delta x, t) - 2u(x, t) + u(x - \Delta x, t)$ 做 $\theta$-加权平均得到完整的空间上的截断误差。最后的截断误差是
$$
T\left(x, t + \frac{1}{2}\Delta t\right) = \left(\frac{\partial u}{\partial t} - \frac{\partial^2 u}{\partial x^2}\right) + \left[\left(\frac{1}{2} - \theta\right)\frac{\partial^2 u}{\partial t^2} \Delta t - \frac{1}{12}\frac{\partial^4 u}{\partial x^4} \Delta x^2 + \cdots\right] + \left[\frac{1}{24}(\Delta t)^2 \frac{\partial^3 u}{\partial t^3} - \frac{1}{8}(\Delta x)^2 \frac{\partial^4 u}{\partial x^2\partial t^2} + \cdots\right] + \cdots
$$
因此 $\theta = 1/2$ 时，截断误差最小。
