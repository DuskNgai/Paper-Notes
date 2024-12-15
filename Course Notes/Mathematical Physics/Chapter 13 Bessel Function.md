# Chapter 13 Bessel Function

## 13.1 Some Differential Equations

首先导出 Helmholtz 方程。考虑 3 维波动方程：
$$
\frac{\partial^{2}{v}}{\partial{t}^{2}} = a^{2} \nabla^{2}{v} \tag{13.1.1a}
$$
和 3 维热传导方程：
$$
\frac{\partial{v}}{\partial{t}} = a^{2} \nabla^{2}{v} \tag{13.1.1b}
$$
假设它们存在变量分离的解：
$$
v(\mathbf{r},t) = u(\mathbf{r}) T(t) \tag{13.1.2}
$$
带入方程得到相同形式的空间函数 $u(\mathbf{r})$：
$$
\nabla^{2}{u} + k^{2} u = 0 \tag{13.1.3}
$$
其中 $k$ 是分离变量时候所需的常数。这就是 Helmholtz 方程。

---

将 Helmholtz 方程引入球坐标系中，得到：
$$
\frac{1}{r^{2}} \frac{\partial}{\partial{r}} \left(r^{2} \frac{\partial{u}}{\partial{r}}\right) + \frac{1}{r^{2} \sin \theta} \frac{\partial}{\partial{\theta}} \left(\sin \theta \frac{\partial{u}}{\partial{\theta}}\right) + \frac{1}{r^{2} \sin^{2} \theta} \frac{\partial^{2}{u}}{\partial{\phi}^{2}} + k^{2} u = 0 \tag{13.1.6}
$$
假设其存在变量分离的解：
$$
u(r, \theta, \phi) = R(r) \Theta(\theta) \Phi(\phi) \tag{13.1.7}
$$
将其带入 $(13.1.6)$ 式得到：
$$
\color{red}
\frac{\mathrm{d}}{\mathrm{d}r}\left(r^{2} \frac{\mathrm{d}R}{\mathrm{d}r}\right) + (k^{2} r^{2} - \omega^{2}) R = 0 \tag{13.1.8}
$$
其中 $\omega$ 是分离变量时候所需的常数。这就是球 Bessel 方程。

---

将 Helmholtz 方程引入柱坐标系中，得到：
$$
\frac{1}{r} \frac{\partial}{\partial{r}} \left(r \frac{\partial{u}}{\partial{r}}\right) + \frac{1}{r^{2}} \frac{\partial^{2}{u}}{\partial{\phi}^{2}} + \frac{\partial^{2}{u}}{\partial{z}^{2}} + k^{2} u = 0 \tag{13.1.11}
$$
假设其存在变量分离的解：
$$
u(r, \phi, z) = R(r) \Phi(\phi) \tag{13.1.12}
$$
将其带入 $(13.1.11)$ 式得到：
$$
r^{2} \frac{\mathrm{d}^{2}R}{\mathrm{d}r^{2}} + r \frac{\mathrm{d}R}{\mathrm{d}r} + [(k^{2} + \omega^{2}) r^{2} - \nu^{2}] R = 0 \tag{13.1.13}
$$
令 $x = \sqrt{k^{2} + \omega^{2}}r$，$y(x) = R(r)$，则：
$$
\color{red}
x^{2} \frac{\mathrm{d}^{2}y}{\mathrm{d}x^{2}} + x \frac{\mathrm{d}y}{\mathrm{d}x} + (x^{2} - \nu^{2}) y = 0 \tag{13.1.14}
$$
其中 $\nu$ 是分离变量时候所需的常数。这就是 $\nu$ 阶 Bessel 方程。

## 13.3 Solution of Bessel Function

### 13.3.1 Series Solution of Bessel Function

Bessel 函数的解可以通过广义幂级数展开得到。令：
$$
y = \sum_{k = 0}^{\infty} C_{k} x^{C + k} \quad (C_{0} \ne 0) \tag{13.3.4}
$$
带入 $(13.1.14)$ 式得到:
$$
C_{0}[C^{2} - \nu^{2}] x^{C} + C_{1}[(C + 1)^{2} - \nu^{2}] x^{C + 1} + 
\sum_{k = 2}^{\infty} \{C_{k}[(C + k)^{2} - \nu^{2}] + C_{k - 2}\} x^{C + k} = 0 \tag{13.3.8}
$$
这要求 $x$ 的各次项为零：
$$
\begin{aligned}
C_{0}[C^{2} - \nu^{2}] &= 0\\
C_{1}[(C + 1)^{2} - \nu^{2}] &= 0\\
C_{k}[(C + k)^{2} - \nu^{2}] + C_{k - 2} &= 0 \quad (k \ge 2)
\end{aligned} \tag{13.3.9}
$$
广义幂级数要求 $C_{0} \ne 0$，可以解得 $C$：
$$
C = \pm \nu \tag{13.3.10}
$$

### 13.3.2 Special Solution of Bessel Function

带 $C = \nu$ 入递推关系式得到：
$$
C_{k} = -\frac{C_{k - 2}}{k(k + 2\nu)} \quad (k \ge 2) \tag{13.3.11}
$$
因为这是一个双间隔的递推数列，因此需要分开考察奇数项和偶数项。对于奇数项有：
$$
C_{1}(2\nu + 1) = 0
$$
可得 $C_{1} = 0$，因此奇数项都为 $0$。对于偶数项，我们令 $k = 2m$，则：
$$
C_{2m} = \frac{(-1)^{m} C_{0}}{2^{2m} m!(1 + \nu) \cdots (m + \nu)} \tag{13.3.12}
$$
因此 Bessel 方程的解为：
$$
y = C_{0} \sum_{m = 0}^{\infty} \frac{(-1)^{m}}{2^{2m} m!(1 + \nu) \cdots (m + \nu)} x^{2m + \nu} \tag{13.3.13}
$$
$C_{0}$ 是任意常数，为了让形式紧凑，取：
$$
C_{0} = \frac{1}{2^{\nu} \Gamma(\nu + 1)} \tag{13.3.14}
$$

因此最后得到 $\nu$ 阶第一类 Bessel 函数（Bessel 方程的第一个特解）：
$$
\color{red}
J_{\nu}(x) = \sum_{m = 0}^{\infty} \frac{(-1)^{m}}{m! \Gamma(m + \nu + 1)} \left(\frac{x}{2}\right)^{2m + \nu} \tag{13.3.15}
$$

当 $\nu$ 为自然数时，利用 Gamma 函数性质可以得到：
$$
\color{red}
J_{n}(x) = \sum_{m = 0}^{n} \frac{(-1)^{m}}{m!(m + n)!} \left(\frac{x}{2}\right)^{2m + n} \tag{13.3.17}
$$

考察 $x = 0$ 时候 $J_{\nu}(x)$ 的取值情况。当 $\nu > 0$ 时，$J_{\nu}(0) = 0$。当 $\nu = 0$ 时，$J_{0}(0) = 1$。负数的非整数次幂无良好定义，因此只考虑 $x \ge 0$ 的情况。

### 13.3.3 General Solution of Bessel Function

把 $C = -\nu$ 带入 $(13.3.15)$ 式，得到 Bessel 方程的第二个特解：
$$
J_{-\nu}(x) = \sum_{m = 0}^{\infty} \frac{(-1)^{m}}{m! \Gamma(m - \nu + 1)} \left(\frac{x}{2}\right)^{2m - \nu} \tag{13.3.16}
$$
$J_{\nu}(x)$ 和 $J_{-\nu}(x)$ 是两个线性独立的解。这里我们可以通过考察 $\nu = \pm 1 / 2$ 处的值。其中：
$$
\begin{aligned}
J_{1 / 2}(x) &= \sum_{m = 0}^{\infty} \frac{(-1)^{m}}{m! \Gamma(m + \frac{1}{2} + 1)} \left(\frac{x}{2}\right)^{2m + \frac{1}{2}} \\
&= \frac{1}{\sqrt{\pi}} \sum_{m = 0}^{\infty} \frac{(-1)^{m}2^{2m + 1}}{(2m + 1)!} \left(\frac{x}{2}\right)^{2m + \frac{1}{2}} \\
&= \sqrt{\frac{2}{\pi x}} \sum_{m = 0}^{\infty} \frac{(-1)^{m}}{(2m + 1)!} x^{2m + 1} \\
&= \sqrt{\frac{2}{\pi x}} \sin x
\end{aligned} \tag{13.3.19}
$$
而：
$$
\begin{aligned}
J_{-1 / 2}(x) &= \sum_{m = 0}^{\infty} \frac{(-1)^{m}}{m! \Gamma(m - \frac{1}{2} + 1)} \left(\frac{x}{2}\right)^{2m - \frac{1}{2}} \\
&= \frac{1}{\sqrt{\pi}} \sum_{m = 0}^{\infty} \frac{(-1)^{m}2^{2m}}{(2m )!} \left(\frac{x}{2}\right)^{2m - \frac{1}{2}} \\
&= \sqrt{\frac{2}{\pi x}} \sum_{m = 0}^{\infty} \frac{(-1)^{m}}{(2m)!} x^{2m} \\
&= \sqrt{\frac{2}{\pi x}} \cos x
\end{aligned} \tag{13.3.20}
$$
显然，$J_{1 / 2}(0) = 0$，而 $J_{-1 / 2}(0) = +\infty$，二者是线性独立的。

## 13.4 Property of Bessel Function

### 13.4.1 Generative Function

整数阶 Bessel 函数 $J_{n}(x)$ 的生成函数为：
$$
\color{red}
\exp \left[\frac{x}{2} \left(r - \frac{1}{r}\right)\right] = \sum_{n = -\infty}^{\infty} J_{n}(x) r^{n} \tag{13.4.2}
$$

证明：
$$
\begin{aligned}
\exp \left[\frac{x}{2} \left(r - \frac{1}{r}\right)\right] &= \exp \left(\frac{xr}{2}\right) \exp \left(-\frac{x}{2r}\right) \\
&= \left[\sum_{l = 0}^{\infty} \frac{1}{l!} \left(\frac{xr}{2}\right)^{l}\right] \left[\sum_{k = 0}^{\infty} \frac{1}{k!} \left(-\frac{x}{2r}\right)^{k}\right] && \text{Taylor Expansion} \\
&= \sum_{l = 0}^{\infty} \sum_{k = 0}^{\infty} \frac{(-1)^{k}}{l! k!} \left(\frac{x}{2}\right)^{l + k} r^{l - k} && \text{Cauchy Product} \\
&= \sum_{n = -\infty}^{\infty} \left[\sum_{k = 0}^{\infty} \frac{(-1)^{k}}{(n + k)! k!} \left(\frac{x}{2}\right)^{n + 2k}\right] r^{n} && n = l - k \\
&= \sum_{n = -\infty}^{\infty} J_{n}(x) r^{n}
\end{aligned} \tag{13.4.3}
$$

### 13.4.2 Recurrence Relation

对于任意 $\nu$，都有：
$$
\begin{aligned}
\frac{\mathrm{d}}{\mathrm{d}x} [x^{\nu} J_{\nu}(x)] &= x^{\nu} J_{\nu - 1}(x) \\
\frac{\mathrm{d}}{\mathrm{d}x} [x^{-\nu} J_{\nu}(x)] &= -x^{-\nu} J_{\nu + 1}(x)
\end{aligned} \tag{13.4.6}
$$

以及：
$$
\begin{aligned}
\frac{\mathrm{d}}{\mathrm{d}x} J_{\nu}(x) &= \frac{1}{2}[J_{\nu - 1}(x) - J_{\nu + 1}(x)] \\
J_{\nu - 1}(x) + J_{\nu + 1}(x) &= \frac{2\nu}{x} J_{\nu}(x)
\end{aligned} \tag{13.4.8}
$$

### 13.4.3 Integral Form

$$
J_{n}(x) = \frac{1}{\pi} \int_{0}^{\pi} \cos(x \sin \theta - n \theta) \mathrm{d}\theta
$$

### 13.4.4 Asymptotic Expansion

$$
J_{n}(x) \approx \sqrt{\frac{2}{\pi x}} \cos \left(x - \frac{\pi}{4} - \frac{\pi n}{2}\right)
$$

## 13.5 Orthogonality and Completeness of Bessel Functions

### 13.5.1 Construction of Orthogonality

从渐进公式可以看出，$J_{n}(x)$ 具有无穷多个正零点。我们记 $\mu_{n, m}$ 为 $J_{n}(x)$ 的第 $m$ 个正零点。而且从渐进公式也可以得到：
$$
\lim_{m \to \infty} (\mu_{n, m + 1} - \mu_{n, m}) = \pi \tag{13.5.6}
$$
考虑一般情况，则基于 Bessel 函数的正交函数集合可以写为 $J_{\nu}(\mu_{\nu, m}x)$。它的含义是，对于确定的阶数 $\nu$，相应于不同零点 $\mu_{\nu, m}$ 的函数 $J_{\nu}(\mu_{\nu, m}x)$ 是正交的。且在 $[0, 1]$ 范围内，$J_{\nu}(\mu_{\nu, m}x)$ 具有 $m$ 个零点。

### 13.5.2 Parametric Form of Bessel Functions

参数形式的 Bessel 函数定义为 $J_{\nu}(\lambda x)$，其中 $\lambda > 0$ 为参数。其对应的 Sturm-Liouville 形式方程为：
$$
\frac{\mathrm{d}}{\mathrm{d}x} \left[\lambda x \frac{\mathrm{d}y}{\mathrm{d}x}\right] + \frac{\nu^{2}}{x} y + \lambda^{2} x y = 0
$$

### 13.5.3 Orthogonality of Bessel Functions

以下证明函数集 $J_{\nu}(\mu_{\nu, m}x)$ 是在 $[0, 1]$ 上正交的。

Bessel 方程和一般的 Sturm-Liouville 方程对比可以发现，我们需要证明带权 $\rho(x) = x$ 的正交性，即：
$$
\int_{0}^{1} x J_{\nu}(\mu_{\nu, m}x) J_{\nu}(\mu_{\nu, k}x) \mathrm{d}x = \delta_{m k} \tag{13.5.14}
$$

