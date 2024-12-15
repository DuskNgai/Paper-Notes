# Chapter 9 Sturm-Liouville Theorem

## 9.1 Sturm-Liouville Eigenvalue Problem

任何一个二阶线性常微分方程都可以化成如下的形式：
$$
\frac{\mathrm{d}}{\mathrm{d}x} \left[k(x) \frac{\mathrm{d}y}{\mathrm{d}x}\right] - q(x) y + \lambda \rho(x) y = 0 \quad (0 < x < L) \tag{9.1.1}
$$
其中 $k(x)$, $q(x)$, $\rho(x)$ 都是 $x$ 的函数，$\lambda$ 是常数。这个方程称为 Sturm-Liouville 方程。

以下一般性地讨论 Sturm-Liouville 本征值问题。需要规定 $k(x)$, $k'(x)$, $q(x)$, $\rho(x)$ 在开区间 $(0, L)$ 上是连续的，且 $k(x), \rho(x) > 0$。在这样的条件下，存在无穷多个实的本征值，并构成一个递增序列：
$$
\lambda_{1} < \lambda_{2} < \cdots < \lambda_{n} < \cdots \tag{9.1.2}
$$

设 $\lambda_{m}$ 和 $\lambda_{n}$ 是两个不同的本征值，对应的本征函数为 $y_{m}$ 和 $y_{n}$，则有：
$$
\frac{\mathrm{d}}{\mathrm{d}x} \left[k(x) \frac{\mathrm{d}y_{m}}{\mathrm{d}x}\right] - q(x) y_{m} + \lambda_{m} \rho(x) y_{m} = 0 \tag{9.1.3}
$$
和：
$$
\frac{\mathrm{d}}{\mathrm{d}x} \left[k(x) \frac{\mathrm{d}y_{n}}{\mathrm{d}x}\right] -p  q(x) y_{n} + \lambda_{n} \rho(x) y_{n} = 0 \tag{9.1.4}
$$
两个式子分别乘以 $y_{n}$ 和 $y_{m}$，并且相减，得到：
$$
y_{n} \frac{\mathrm{d}}{\mathrm{d}x} \left[k \frac{\mathrm{d}y_{m}}{\mathrm{d}x}\right] - y_{m} \frac{\mathrm{d}}{\mathrm{d}x} \left[k \frac{\mathrm{d}y_{n}}{\mathrm{d}x}\right] + (\lambda_{m} - \lambda_{n}) \rho y_{m}y_{n} = 0 \tag{9.1.5}
$$
两边从 $0$ 到 $L$ 积分，得到：
$$
\begin{aligned}
&\quad\ \ \int_{0}^{L} y_{n} \frac{\mathrm{d}}{\mathrm{d}x} \left[k \frac{\mathrm{d}y_{m}}{\mathrm{d}x}\right] - y_{m} \frac{\mathrm{d}}{\mathrm{d}x} \left[k \frac{\mathrm{d}y_{n}}{\mathrm{d}x}\right] + (\lambda_{m} - \lambda_{n}) \rho y_{m}y_{n} \mathrm{d}x \\
&= \int_{0}^{L} \frac{\mathrm{d}}{\mathrm{d}x} \left[ky_{n}\frac{\mathrm{d}y_{m}}{\mathrm{d}x} - ky_{m}\frac{\mathrm{d}y_{n}}{\mathrm{d}x}\right] \mathrm{d}x + (\lambda_{m} - \lambda_{n}) \int_{0}^{L} \rho y_{m}y_{n} \mathrm{d}x \\
&= \left[ky_{n}\frac{\mathrm{d}y_{m}}{\mathrm{d}x} - ky_{m}\frac{\mathrm{d}y_{n}}{\mathrm{d}x}\right]_{0}^{L} + (\lambda_{m} - \lambda_{n}) \int_{0}^{L} \rho y_{m}y_{n} \mathrm{d}x \\
&= -k(0)\begin{vmatrix}y_{n}(0) & y_{m}(0) \\ y'_{n}(0) & y'_{m}(0)\end{vmatrix} + k(L)\begin{vmatrix}y_{n}(L) & y_{m}(L) \\ y'_{n}(L) & y'_{m}(L)\end{vmatrix} \\
&+ (\lambda_{m} - \lambda_{n}) \int_{0}^{L} \rho y_{m}y_{n} \mathrm{d}x \\
&= 0
\end{aligned}
$$

因此就有：
$$
\int_{0}^{L} \rho y_{m}y_{n} \mathrm{d}x = \frac{k(0)\begin{vmatrix}y_{n}(0) & y_{m}(0) \\ y'_{n}(0) & y'_{m}(0)\end{vmatrix} - k(L)\begin{vmatrix}y_{n}(L) & y_{m}(L) \\ y'_{n}(L) & y'_{m}(L)\end{vmatrix}}{\lambda_{m} - \lambda_{n}} \tag{9.1.6}
$$
这表示说，只要边界条件的取值满足分子为 0，则本征函数集 $\{y_{n}(x)\}$ 在 $[0, L]$ 上是带权正交的。

如果函数 $f(x)$ 在 $[0, L]$ 上具有连续二阶导数并且满足和本征函数集 $\{y_{n}(x)\}$ 一致的边界条件，那么 $f(x)$ 可以展开为绝对且一致收敛的级数：
$$
f(x) = \sum_{n=1}^{\infty} C_{n} y_{n}(x) \tag{9.1.8}
$$
这种性质称为完备性。其中系数 $C_{n}$ 可以利用正交性来计算：
$$
\begin{aligned}
\int_{0}^{L} \rho(x) y_{n}(x) f(x) \mathrm{d}x &= \int_{0}^{L} \rho(x) y_{n}(x) \left[\sum_{m=1}^{\infty} C_{m} y_{m}(x)\right] \mathrm{d}x \\
&= \sum_{m=1}^{\infty} C_{m} \int_{0}^{L} \rho(x) y_{n}(x) y_{m}(x) \mathrm{d}x \\
&= C_{n} \int_{0}^{L} \rho(x) y_{n}(x) y_{n}(x) \mathrm{d}x
\end{aligned} \tag{9.1.9}
$$

---

Sturm-Liouvielle 方程提供了一种寻找合适含义的完备正交基函数的方式。
