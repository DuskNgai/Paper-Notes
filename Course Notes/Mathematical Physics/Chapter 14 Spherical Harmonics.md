# Chapter 14 Spherical Harmonics

## 14.1 Legendre 方程的引入

Laplace 方程在球坐标系中的表示为
$$
\frac{1}{r^2}\frac{\partial}{\partial{r}}\left(r^2\frac{\partial{u}}{\partial{r}}\right)+\frac{1}{r^2\sin\theta}\frac{\partial}{\partial\theta}\left(\sin\theta\frac{\partial{u}}{\partial\theta}\right)+\frac{1}{r^2\sin^2\theta}\frac{\partial^2{u}}{\partial\phi^2}=0\tag{14.1.1}
$$
设方程 $(14.1.1)$ 有变量分离的形式解
$$
u(r,\theta,\phi)=R(r)\Theta(\theta)\Phi(\phi)\tag{14.1.2}
$$
带入 $(14.1.2)$ 式：
$$
\frac{\Theta\Phi}{r^2}\frac{\mathrm{d}}{\mathrm{d}r}\left(r^2\frac{\mathrm{d}R}{\mathrm{d}r}\right)+\frac{R\Phi}{r^2\sin\theta}\frac{\mathrm{d}}{\mathrm{d}\theta}\left(\sin\theta\frac{\mathrm{d}\Theta}{\mathrm{d}\theta}\right)+\frac{R\Theta}{r^2\sin^2\theta}\frac{\mathrm{d}^2\Phi}{\mathrm{d}\phi^2}=0\tag{14.1.3}
$$
两边同乘 $\dfrac{r^2}{R\Theta\Phi}$ 并且移项后得到：
$$
\frac{1}{R}\frac{\mathrm{d}}{\mathrm{d}r}\left(r^2\frac{\mathrm{d}R}{\mathrm{d}r}\right)=-\frac{1}{\Theta\sin\theta}\frac{\mathrm{d}}{\mathrm{d}\theta}\left(\sin\theta\frac{\mathrm{d}\Theta}{\mathrm{d}\theta}\right)-\frac{1}{\Phi\sin^2\theta}\frac{\mathrm{d}^2\Phi}{\mathrm{d}\phi^2}\tag{14.1.4}
$$
左右两侧分别为关于 $r$ 的函数和关于 $\theta, \phi$ 的函数，二者只可能等于某个常数，令这个常数为 $l(l+1)$，因此：
$$
\frac{1}{R}\frac{\mathrm{d}}{\mathrm{d}r}\left(r^2\frac{\mathrm{d}R}{\mathrm{d}r}\right)=l(l+1)\\
r^2\frac{\mathrm{d}^2R}{\mathrm{d}r^2}+2r\frac{\mathrm{d}R}{\mathrm{d}r}-l(l+1)R=0\tag{14.1.5a}
$$

$$
\frac{1}{\Theta\sin\theta}\frac{\mathrm{d}}{\mathrm{d}\theta}\left(\sin\theta\frac{\mathrm{d}\Theta}{\mathrm{d}\theta}\right)+\frac{1}{\Phi\sin^2\theta}\frac{\mathrm{d}^2\Phi}{\mathrm{d}\phi^2}=-l(l+1)\tag{14.1.5b}
$$

$(14.1.5a)$ 式为欧拉方程：

---

令 $r=e^t$，则：
$$
\begin{align*}
\frac{\mathrm{d}R}{\mathrm{d}r}&=\frac{\mathrm{d}R}{\mathrm{d}t}\frac{\mathrm{d}t}{\mathrm{d}r}=\frac{1}{r}\frac{\mathrm{d}R}{\mathrm{d}t}\\
\frac{\mathrm{d}^2R}{\mathrm{d}r^2}&=\frac{\mathrm{d}}{\mathrm{d}r}\left(\frac{\mathrm{d}R}{\mathrm{d}r}\right)=\frac{\mathrm{d}}{\mathrm{d}r}\left(\frac{1}{r}\frac{\mathrm{d}R}{\mathrm{d}t}\right)\\
&=-\frac{1}{r^2}\frac{\mathrm{d}R}{\mathrm{d}t}+\frac{1}{r}\frac{\mathrm{d}}{\mathrm{d}r}\left(\frac{\mathrm{d}R}{\mathrm{d}t}\right)\\
&=-\frac{1}{r^2}\frac{\mathrm{d}R}{\mathrm{d}t}+\frac{1}{r}\frac{\mathrm{d}}{\mathrm{d}t}\left(\frac{\mathrm{d}R}{\mathrm{d}t}\right)\frac{\mathrm{d}t}{\mathrm{d}r}\\
&=-\frac{1}{r^2}\frac{\mathrm{d}R}{\mathrm{d}t}+\frac{1}{r^2}\frac{\mathrm{d}^2R}{\mathrm{d}t^2}\\
&=\frac{1}{r^2}\left(\frac{\mathrm{d}^2R}{\mathrm{d}t^2}-\frac{\mathrm{d}R}{\mathrm{d}t}\right)
\end{align*}
$$
带入 $(14.1.5a)$ 式：

$$
\frac{\mathrm{d}^2R}{\mathrm{d}t^2}+\frac{\mathrm{d}R}{\mathrm{d}t}-l(l+1)R=0\\
$$
解得：
$$
\color{red}\begin{align*}
R&=Ae^{lt}+Be^{-(l+1)t}\\
&=Ar^l+B\frac{1}{r^{l+1}}
\end{align*}\tag{14.1.6}
$$

---

$(14.1.5b)$ 式左右两边同时乘以 $\sin^2\theta$，并且移项：
$$
\frac{\sin\theta}{\Theta}\frac{\mathrm{d}}{\mathrm{d}\theta}\left(\sin\theta\frac{\mathrm{d}\Theta}{\mathrm{d}\theta}\right)+\frac{1}{\Phi}\frac{\mathrm{d}^2\Phi}{\mathrm{d}\phi^2}=-l(l+1)\sin\theta\\
\frac{\sin\theta}{\Theta}\frac{\mathrm{d}}{\mathrm{d}\theta}\left(\sin\theta\frac{\mathrm{d}\Theta}{\mathrm{d}\theta}\right)+l(l+1)\sin\theta=-\frac{1}{\Phi}\frac{\mathrm{d}^2\Phi}{\mathrm{d}\phi^2}\tag{14.1.7}
$$
同理，方程左右必须等于某个常数，令这个常数为 $m^2$：
$$
\frac{\sin\theta}{\Theta}\frac{\mathrm{d}}{\mathrm{d}\theta}\left(\sin\theta\frac{\mathrm{d}\Theta}{\mathrm{d}\theta}\right)+l(l+1)\sin\theta=m^2\\
\frac{\sin\theta}{\Theta}\left(\cos\theta\frac{\mathrm{d}\Theta}{\mathrm{d}\theta}+\sin\theta\frac{\mathrm{d}^2\Theta}{\mathrm{d}\theta^2}\right)+l(l+1)\sin^2\theta=m^2\tag{14.1.8a}
$$

$$
-\frac{1}{\Phi}\frac{\mathrm{d}^2\Phi}{\mathrm{d}\phi^2}=m^2\tag{14.1.8b}
$$

$(14.1.8b)$ 式的通解为：
$$
\color{red}\Phi(\phi)=C_1\cos m\phi+C_2\sin m\phi\tag{14.1.9}
$$
对于 $(14.1.8a)$ 式，进行变量代换：
$$
x=\cos\theta,y=\Theta(\theta)
$$
可得：
$$
\begin{align*}
\frac{\mathrm{d}\Theta}{\mathrm{d}\theta}&=\frac{\mathrm{d}y}{\mathrm{d}x}\frac{\mathrm{d}x}{\mathrm{d}\theta}=-\frac{\mathrm{d}y}{\mathrm{d}x}\sin\theta\\
\frac{\mathrm{d}^2\Theta}{\mathrm{d}\theta^2}&=-\frac{\mathrm{d}}{\mathrm{d}\theta}\left(\frac{\mathrm{d}y}{\mathrm{d}x}\sin\theta\right)\\
&=-\frac{\mathrm{d}}{\mathrm{d}\theta}\left(\frac{\mathrm{d}y}{\mathrm{d}x}\right)\sin\theta-\frac{\mathrm{d}y}{\mathrm{d}x}\cos\theta\\
&=-\frac{\mathrm{d}}{\mathrm{d}x}\left(\frac{\mathrm{d}y}{\mathrm{d}x}\right)\frac{\mathrm{d}x}{\mathrm{d}\theta}\sin\theta-\frac{\mathrm{d}y}{\mathrm{d}x}\cos\theta\\
&=\frac{\mathrm{d}^2y}{\mathrm{d}x^2}\sin^2\theta-\frac{\mathrm{d}y}{\mathrm{d}x}\cos\theta\\
\end{align*}\tag{14.1.10}
$$
带入 $(14.1.8a)$ 式：
$$
(1-x^2)\frac{\mathrm{d}^2y}{\mathrm{d}x^2}-2x\frac{\mathrm{d}y}{\mathrm{d}x}+\left[l(l+1)-\frac{m^2}{1-x^2}\right]y=0\tag{14.1.11}
$$
$(1.6)$ 式即为 $l$ 阶**连带 Legendre 方程**。$m=0$ 时，为 $l$ 阶 **Legendre 方程**。

## 14.2 Legendre 多项式

先考虑 $m=0$ 的情况。改写 Legendre 方程为：
$$
\frac{\mathrm{d}^2y}{\mathrm{d}x^2}-\frac{2x}{1-x^2}\frac{\mathrm{d}y}{\mathrm{d}x}+\frac{l(l+1)}{1-x^2}y=0\tag{14.1.13}
$$

在 $x=0$ 处，
$$
P(x)=\frac{2x}{1-x^2}\to0\\
Q(x)=\frac{l(l+1)}{1-x^2}\to l(l+1)
$$
均为有限值，他们必然在 $x=0$ 处解析，在这点有级数解，设解为：
$$
y=\sum_{k=0}^{\infty}C_kx^k\tag{14.2.2}
$$
带入 $(14.1.11)$​ 式：
$$
(1-x^2)\sum_{k=\color{red}2}^{\infty}k(k-1)C_kx^{k-2}-2x\sum_{k=\color{red}1}^{\infty}kC_kx^{k-1}+l(l+1)\sum_{k=0}^{\infty}C_kx^k=0\\
\sum_{k=0}^{\infty}(k+2)(k+1)C_{k+2}x^k-\sum_{k=\color{red}2}^{\infty}k(k-1)C_kx^k-2\sum_{k=\color{red}1}^{\infty}kC_kx^k+l(l+1)\sum_{k=0}^{\infty}C_kx^k=0\\
\left[\sum_{k=0}^{\infty}(k+2)(k+1)C_{k+2}-\sum_{k=\color{red}0}^{\infty}k(k-1)C_k-2\sum_{k=\color{red}0}^{\infty}kC_k+l(l+1)\sum_{k=0}^{\infty}C_k\right]x^k=0\\
\sum_{k=0}^{\infty}[(k+2)(k+1)C_{k+2}-(k-l)(k+l+1)C_k]x^k=0\\
$$
因此需要上述各项为 $0$，得到解的系数的递推公式：
$$
(k+2)(k+1)C_{k+2}-(k-l)(k+l+1)C_k=0\\
C_{k+2}=\frac{(k-l)(k+l+1)}{(k+2)(k+1)}C_k\tag{14.2.5}
$$
从 $C_0$ 开始递推偶数项：
$$
C_2=\frac{-l(l+1)}{2!}C_0\\
C_4=\frac{(2-l)(3+l)}{4\cdot3}C_2=\frac{(2-l)(0-l)(l+1)(3+l)}{4!}C_0\\
\cdots\\
C_{2k}=\frac{(2k-2-l)\cdots(0-l)(1+l)\cdots(2k-1+l)}{(2k)!}C_0
$$
从 $C_1$ 开始递推奇数项：

$$
C_3=\frac{(1-l)(2+l)}{3!}C_1\\
C_5=\frac{(3-l)(4+l)}{5\cdot4}C_3=\frac{(3-l)(1-l)(2+l)(4+l)}{5!}C_1\\
\cdots\\
C_{2k+1}=\frac{(2k-1-l)\cdots(1-l)(2+l)\cdots(2k+l)}{(2k+1)!}C_1
$$
因此得到 Legendre 方程的解：
$$
\color{red}\begin{align*}
y&=y_0(x)+y_1(x)\\
&=C_0\left[1+\sum_{k=1}^{\infty}\frac{(2k-2-l)\cdots(0-l)(1+l)\cdots(2k-1+l)}{(2k)!}x^{2k}\right]\\
&+C_1\left[x+\sum_{k=1}^{\infty}\frac{(2k-1-l)\cdots(1-l)(2+l)\cdots(2k+l)}{(2k+1)!}x^{2k+1}\right]
\end{align*}\tag{14.2.7}
$$
奇偶部分的级数是 Legendre 方程两个线性独立的解，称为 Legendre 函数。其中，$y_0(x)$ 是偶函数，$y_1(x)$ 是奇函数。

按照达朗贝尔判别法，Legendre 函数收敛半径为：

$$
R=\lim_{k\to\infty}\left|\frac{C_k}{C_{k+2}}\right|=\lim_{k\to\infty}\left|\frac{(k+2)(k+1)}{(k-l)(k+l+1)}\right|=1
$$
因此 Legendre 函数收敛域为 $x\in(-1,1)$ ，对应 $\theta\in(0,\pi)$。

---

$$
\begin{align*}
\lim_{k\to\infty}k\left(\frac{(2k+2)(2k+1)}{(2k-l)(2k+l+1)}-1\right)&=\lim_{k\to\infty}k\left(\frac{4k^2+6k+2}{4k^2+2k-l^2-l}-1\right)\\
&=\lim_{k\to\infty}\left(\frac{4k^2+(2+l^2+l)k}{k^2+k-l^2-l}\right)\\
&=4>1
\end{align*}
$$

因此由 Raabe 判别法可得，$y_0(x)$ 在 $x=1$ 处发散。同理 $y_1(x)$ 在 $x=-1$ 处发散。

---

观察可得，当某个因子出现在系数中时候，它会在之后的所有项中出现。特别是，当 $l$ 取整数时候，对于某个 $k$，$2k-l$ 或 $2k-1-l$ 的其中一个数会变成 $0$，即 $y_0(x)$ 或 $y_1(x)$ 会从无穷级数变为有限的级数，发散的问题就不存在了：

**如果 $l$ 是正偶数， $y_0(x)$ 变为 $l$ 次多项式；如果 $l$ 是正奇数， $y_1(x)$ 变为 $l$ 次多项式。$l$ 为负数时候可以得到类似结论。**

对于 $y_0(x)$ 或 $y_1(x)$，取最高次项的系数 $C_l$ 为
$$
C_l=\frac{(2l)!}{2^l(l!)^2}\tag{14.2.11}
$$
时候，可以使得各项的系数保持简洁：
$$
C_{l-2}=-\frac{l(l-1)}{2(2l-1)}C_l=-\frac{(2l-2)!}{2^l(l-1)!(l-2)!}\\
C_{l-4}=-\frac{(l-2)(l-3)}{4(2l-3)}C_{l-2}=\frac{(2l-4)!}{2^l2!(l-2)!(l-4)!}\\
C_{l-2k}=(-1)^{k}\frac{(2l-2k)!}{2^lk!(l-k)!(l-2k)!}\quad k\in\{0,\dots,l/2\}
$$
因此，$l$ 次的 Legendre 多项式为：
$$
P_{l}(x)=\frac{1}{2^l}\sum_{k=0}^{K}(-1)^{k}\frac{(2l-2k)!}{k!(l-k)!(l-2k)!}x^{l-2k}\tag{14.2.17}
$$
其中，$l$ 为偶数时候 $K=l/2$，$l$ 为奇数时候 $K=(l-1)/2$。可以看到当 $l$ 为偶数时，$P_l(x)$ 为偶函数；当 $l$ 为奇数时，$P_l(x)$ 为奇函数。

以下是前几阶的 Legendre 多项式：
$$
\begin{align*}
P_0(x)&=1&P_1(x)&=x\\
P_2(x)&=\frac{1}{2}(3x^2-1)&P_3(x)&=\frac{1}{2}(5x^3-3x)\\
P_4(x)&=\frac{1}{8}(35x^4-30x^2+3)&P_5(x)&=\frac{1}{8}(63x^5-70x^3+15x)
\end{align*}
$$

以 $\theta$ 为变量：
$$
\begin{align*}
P_0(x)&=1&P_1(x)&=\cos\theta\\
P_2(x)&=\frac{1}{4}(3\cos2\theta+1)&P_3(x)&=\frac{1}{8}(5\cos3\theta+3\cos\theta)\\
P_4(x)&=\frac{1}{64}(35\cos4\theta+20\cos2\theta+9)&P_5(x)&=\frac{1}{128}(63\cos5\theta+35\cos3\theta+30\cos\theta)
\end{align*}
$$

$l$ 取整数时候，通解 $(14.2.7)$ 包含一个被截断的解函数和一个无穷级数，因此需要考虑
$$
\begin{align*}
y(x)&=A[y_0(x)+y_1(x)]+B[Y_0(x)+Y_1(x)]\\
&=AP_l(x)+BQ_l(x)
\end{align*}
$$
无穷级数的解才是最终解。

## 14.3 Legendre 多项式的基本性质

### 14.3.1 微分表示

Rodriques 公式：
$$
P_l(x)=\frac{1}{2^ll!}\frac{\mathrm{d}^l}{\mathrm{d}x^l}(x^2-1)^l\tag{14.3.1}
$$

<blockquote style="border-left: 5px solid #4545aa; border-radius: 3px 0 0 3px; padding: 10px 15px; background-color: rgba(70, 70, 188, 0.1)">
    证明
</blockquote>

二项式定理展开：
$$
\begin{align*}
(x^2-1)^l&=\sum_{k=0}^{l}\binom{l}{k}x^{2l-2k}(-1)^{k}\\
&=\sum_{k=0}^{l}\frac{(-1)^{k}l!}{k!(l-k)!}x^{2l-2k}\\
\end{align*}
$$
因此：
$$
\begin{align*}
\frac{1}{2^ll!}\frac{\mathrm{d}^l}{\mathrm{d}x^l}(x^2-1)^l&=\frac{1}{2^ll!}\frac{\mathrm{d}^l}{\mathrm{d}x^l}\sum_{k=0}^{l}\frac{(-1)^{k}}{k!(l-k)!}x^{2l-2k}\\
&=\frac{1}{2^l}\sum_{k=0}^{l}\frac{(-1)^{k}}{k!(l-k)!}\frac{\mathrm{d}^l}{\mathrm{d}x^l}x^{2l-2k}\\
&=\frac{1}{2^l}\sum_{k=0}^{K}\frac{(-1)^{k}}{k!(l-k)!}\frac{\mathrm{d}^l}{\mathrm{d}x^l}x^{2l-2k}\\
&=\frac{1}{2^l}\sum_{k=0}^{K}\frac{(-1)^{k}(2l-2k)!}{k!(l-k)!(l-2k)!}x^{l-2k}\\
&=P_l(x)
\end{align*}
$$

$l$ 为偶数时候 $K=l/2$，$l$ 为奇数时候 $K=(l-1)/2$。

### 14.3.2 积分表示

Pass.

### 14.3.3 生成函数

由二项式定理
$$
(1+v)^p=\sum_{k=0}^{\infty}\frac{p!}{(p-k)!k!}v^k,(|v|<1)\tag{14.3.7}
$$
取 $p=-1/2$，我们得到
$$
\begin{align*}
\frac{1}{\sqrt{1+v}}&=\sum_{k=0}^{\infty}\frac{\left(-\dfrac{1}{2}\right)!}{\left(-\dfrac{1}{2}-k\right)!k!}v^k\\
&=\sum_{k=0}^{\infty}\frac{\left(-\dfrac{1}{2}\right)\left(-\dfrac{3}{2}\right)\dots\left(-\dfrac{1-2k}{2}\right)}{k!}v^k\\
&=\sum_{k=0}^{\infty}(-1)^{k}\frac{\left(\dfrac{1}{2}\right)\left(\dfrac{2}{2}\right)\left(\dfrac{3}{2}\right)\dots\left(\dfrac{2k-1}{2}\right)\left(\dfrac{2k}{2}\right)}{(k!)^2}v^k\\
&=\sum_{k=0}^{\infty}(-1)^{k}\frac{(2k)!}{2^{2k}(k!)^2}v^k
\end{align*}\tag{14.3.9}
$$
令 $v=-2rx+r^2$
$$
\begin{align*}
\frac{1}{\sqrt{1-2rx+r^2}}&=[1-r(2x-r)]^{-1/2}\\
&=\sum_{k=0}^{\infty}(-1)^{k}\frac{(2k)!}{2^{2k}(k!)^2}r^k(2x-r)^k\\
&=\sum_{k=0}^{\infty}(-1)^{k}\frac{(2k)!}{2^{2k}(k!)^2}r^k\sum_{j=0}^{k}\binom{k}{j}(2x)^{k-j}(-r)^{j}\\
&=\sum_{l=0}^{\infty}P_l(x)r^l
\end{align*}\tag{14.3.12}
$$
因此是生成函数。

### 14.3.4 递推公式

#### 1


$$
(n+1)P_{n+1}(x)=(2n+1)xP_n(x)-nP_{n-1}(x)
$$

<blockquote style="border-left: 5px solid #4545aa; border-radius: 3px 0 0 3px; padding: 10px 15px; background-color: rgba(70, 70, 188, 0.1)">
    证明
</blockquote>
对 $(14.3.12)$ 两边对 $r$ 求导
$$
\begin{align*}
\frac{x-r}{(1-2rx+r^2)^{3/2}}&=\sum_{l=0}^{\infty}P_l(x)lr^{l-1}\\
\frac{x-r}{(1-2rx+r^2)^{1/2}}&=(1-2rx+r^2)\sum_{l=0}^{\infty}P_l(x)lr^{l-1}\\
(x-r)\sum_{l=0}^{\infty}P_l(x)r^l&=(1-2rx+r^2)\sum_{l=0}^{\infty}P_l(x)lr^{l-1}\\
\sum_{l=0}^{\infty}P_l(x)r^l(2l+1)x&=\sum_{l=0}^{\infty}P_l(x)r^{l+1}(l+1)+\sum_{l=0}^{\infty}P_l(x)r^{l-1}l\\
\sum_{l=0}^{\infty}P_l(x)r^{l+1}(l+1)&=\sum_{l=0}^{\infty}P_l(x)r^l(2l+1)x-\sum_{l=0}^{\infty}P_l(x)r^{l-1}l\\
\end{align*}
$$

对比两边系数，可以得到
$$
(n+1)P_{n+1}(x)=(2n+1)xP_n(x)-nP_{n-1}(x)\tag{14.3.36}
$$

#### 2

$$
P_n(x)=P'_{n+1}(x)-2xP'_n(x)+P'_{n-1}(x)
$$

#### 3

$$
xP'_{n}(x)-P'_{n-1}(x)=nP_n(x)
$$

#### 4

$$
P'_{n+1}(x)-P'_{n-1}(x)=(2n+1)P_n(x)
$$

#### 5

$$
nP'_{n+1}(x)+(n+1)P'_{n-1}(x)=(2n+1)xP'_n(x)
$$

#### 6

$$
P'_{n+1}(x)=(n+1)P_n(x)+xP'_n(x)
$$

## 14.4 Legendre 多项式的正交完备性

### 14.4.1 正交性

<blockquote style="border-left: 5px solid #b94263; border-radius: 3px 0 0 3px; padding: 10px 15px; background-color: rgba(185, 66, 110, 0.1)">
    例题
</blockquote>

设 $f(x)$ 是一个 $k$ 阶多项式，证明当 $0<k<l$ 时，$f(x)$ 与 $l$ 阶 Legendre 多项式 $P_l(x)$ 在 $[-1,1]$ 上正交，即
$$
\int_{-1}^{1}f(x)P_l(x)\mathrm{d}x=0\tag{14.3.84}
$$

> 利用 Rodriques 公式 $(14.3.1)$：
> $$
> \begin{align*}
> &\quad\ \int_{-1}^{1}f(x)P_l(x)\mathrm{d}x\\
> &=\frac{1}{2^ll!}\int_{-1}^{1}f(x)\frac{\mathrm{d}^l}{\mathrm{d}x^l}(x^2-1)^l\mathrm{d}x\\
> &=\frac{1}{2^ll!}\left[\underbrace{f(x)\frac{\mathrm{d}^{l-1}}{\mathrm{d}x^{l-1}}(x^2-1)^l}_{\text{containing factor }(x^2-1)}\right]_{-1}^{1}-\frac{1}{2^ll!}\int_{-1}^{1}\frac{\mathrm{d}f}{\mathrm{d}x}\frac{\mathrm{d}^{l-1}}{\mathrm{d}x^{l-1}}(x^2-1)^l\mathrm{d}x\\
> &=0-\frac{1}{2^ll!}\int_{-1}^{1}\frac{\mathrm{d}f}{\mathrm{d}x}\frac{\mathrm{d}^{l-1}}{\mathrm{d}x^{l-1}}(x^2-1)^l\mathrm{d}x\\
> &=\frac{(-1)^{k}}{2^ll!}\int_{-1}^{1}\underbrace{\frac{\mathrm{d}^kf}{\mathrm{d}x^k}}_{\text{constant}}\frac{\mathrm{d}^{l-k}}{\mathrm{d}x^{l-k}}(x^2-1)^l\mathrm{d}x\\
> &=0
> \end{align*}
> $$

因此我们可以得知：
$$
\color{red}\int_{-1}^{1}P_l(x)P_m(x)\mathrm{d}x\begin{cases}=0&l\ne m\\\ne0&l=m\end{cases}\tag{14.4.3}
$$

### 14.4.2 模值

现在证明
$$
\color{red}\int_{-1}^{1}P_l^2(x)\mathrm{d}x=\frac{2}{2l+1}\tag{14.4.7}
$$
利用生成函数：
$$
\begin{align*}
\frac{1}{1-2rx+r^2}&=\left[\sum_{l=0}^{\infty}P_l(x)r^l\right]\left[\sum_{m=0}^{\infty}P_m(x)r^m\right]\\
&=\sum_{l=0}^{\infty}\sum_{m=0}^{\infty}P_l(x)P_m(x)r^{l+m}
\end{align*}\tag{14.4.8}
$$
两边对 $x$ 在区间 $[-1,1]$ 积分，并利用正交性
$$
\begin{align*}
\int_{-1}^{1}\frac{1}{1-2rx+r^2}\mathrm{d}x&=\int_{-1}^{1}\sum_{l=0}^{\infty}\sum_{m=0}^{\infty}P_l(x)P_m(x)r^{l+m}\mathrm{d}x\\
-\frac{1}{2r}\ln(1-2rx+r^2)|_{-1}^1&=\sum_{l=0}^{\infty}\sum_{m=0}^{\infty}r^{l+m}\int_{-1}^{1}P_l(x)P_m(x)\mathrm{d}x\\
-\frac{1}{2r}\ln\left(\frac{1-2r+r^2}{1+2r+r^2}\right)&=\sum_{l=0}^{\infty}r^{2l}\int_{-1}^{1}P_l^2(x)\mathrm{d}x\\
\frac{1}{r}\ln\left(\frac{1+r}{1-r}\right)&=\sum_{l=0}^{\infty}r^{2l}\int_{-1}^{1}P_l^2(x)\mathrm{d}x\\
\sum_{l=0}^{\infty}r^{2l}\frac{2}{2l+1}&=\sum_{l=0}^{\infty}r^{2l}\int_{-1}^{1}P_l^2(x)\mathrm{d}x
\end{align*}\tag{14.4.9}
$$
最后的等号是 $\ln(1+r)$ 的 Taylor 展开。

### 14.4.3 完备性

$$
\color{red}\int_{-1}^{1}P_l(x)P_m(x)\mathrm{d}x=\frac{2}{2l+1}\delta_{lm}\tag{14.4.18}
$$

设 $f(x)$ 在 $[-1,1]$ 上是分段光滑的，则 $f(x)$ 可以按 Legendre 多项式级数展开，即
$$
f(x)=\sum_{m=0}^{\infty}C_mP_m(x)\tag{14.4.19}
$$
系数 $C_m$ 可以用点乘得到：
$$
\begin{align*}
\int_{-1}^{1}f(x)P_l(x)\mathrm{d}x&=\int_{-1}^{1}\sum_{m=0}^{\infty}C_mP_m(x)P_l(x)\mathrm{d}x\\
&=\sum_{m=0}^{\infty}C_m\frac{2}{2m+1}\delta_{ml}\\
&=C_l\frac{2}{2l+1}
\end{align*}
$$
因此系数 $C_m$ 就是
$$
C_m=\frac{2m+1}{2}\int_{-1}^{1}f(x)P_l(x)\mathrm{d}x\tag{14.4.20}
$$
其收敛性由 Dirichlet 定理保证。

## 15.2 角向解：球谐函数

### 15.2.2 连带 Legendre 函数

利用 Legendre 多项式
$$
(1-x^2)\frac{\mathrm{d}^{2}P_{l}(x)}{\mathrm{d}x^{2}}-2x\frac{\mathrm{d}P_{l}(x)}{\mathrm{d}x}+l(l+1)P_{l}(x)=0\tag{15.2.19}
$$
两边对 $x$ 求一阶导到 $m$ 阶导：
$$
(1-x^2)\frac{\mathrm{d}^{1+2}P_{l}(x)}{\mathrm{d}x^{1+2}}-2(1+1)x\frac{\mathrm{d}^{1+1}P_{l}(x)}{\mathrm{d}x^{1+1}}+[l(l+1)-1(1+1)]\frac{\mathrm{d}^{1}P_{l}(x)}{\mathrm{d}x^{1}}=0\\
(1-x^2)\frac{\mathrm{d}^{2+2}P_{l}(x)}{\mathrm{d}x^{2+2}}-2(2+1)x\frac{\mathrm{d}^{2+1}P_{l}(x)}{\mathrm{d}x^{2+1}}+[l(l+1)-2(2+1)]\frac{\mathrm{d}^{2}P_{l}(x)}{\mathrm{d}x^{2}}=0\\
\vdots\\
(1-x^2)\frac{\mathrm{d}^{m+2}P_{l}(x)}{\mathrm{d}x^{m+2}}-2(m+1)x\frac{\mathrm{d}^{m+1}P_{l}(x)}{\mathrm{d}x^{m+1}}+[l(l+1)-m(m+1)]\frac{\mathrm{d}^{m}P_{l}(x)}{\mathrm{d}x^{m}}=0
$$

因为 $P_l(x)$ 是 $l$ 阶多项式，因此 $m\in\{0,\dots,l\}$ 以保证等式左边不为常数 $0$。因此 $P_l(x)$ 的 $m$ 阶导数满足：
$$
(1-x^2)\frac{\mathrm{d}^{2}P_{l}^{(m)}(x)}{\mathrm{d}x^{2}}-2(m+1)x\frac{\mathrm{d}P_{l}^{(m)}(x)}{\mathrm{d}x}+[l(l+1)-m(m+1)]P_{l}^{(m)}(x)=0\tag{15.2.25}
$$

引入变换：

$$
\begin{align*}
w(x)&=(1-x^2)^{m/2}\frac{\mathrm{d}^{m}P_{l}(x)}{\mathrm{d}x^{m}}\\
\frac{\mathrm{d}w}{\mathrm{d}x}&=-mx(1-x^2)^{m/2-1}\frac{\mathrm{d}^{m}P_{l}(x)}{\mathrm{d}x^{m}}\\
&+(1-x^2)^{m/2}\frac{\mathrm{d}^{m+1}P_{l}(x)}{\mathrm{d}x^{m+1}}\\
\frac{\mathrm{d}^2w}{\mathrm{d}x^2}&=-m(1-x^2)^{m/2-1}\frac{\mathrm{d}^{m}P_{l}(x)}{\mathrm{d}x^{m}}\\
&+m(m-2)x^2(1-x^2)^{m/2-2}\frac{\mathrm{d}^{m}P_{l}(x)}{\mathrm{d}x^{m}}\\
&-2mx(1-x^2)^{m/2-1}\frac{\mathrm{d}^{m+1}P_{l}(x)}{\mathrm{d}x^{m+1}}\\
&+(1-x^2)^{m/2}\frac{\mathrm{d}^{m+2}P_{l}(x)}{\mathrm{d}x^{m+2}}\\
\end{align*}\tag{15.2.26}
$$
带入 $(15.2.25)$ 式：
$$
(1-x^2)\frac{\mathrm{d}^{2}w(x)}{\mathrm{d}x^{2}}-2x\frac{\mathrm{d}w}{\mathrm{d}x}+[l(l+1)-\frac{m^2}{1-x^2}]w=0
$$
因此 $w(x)$ 就是 $(15.2.25)$ 式的解，也是连带 Legendre 方程的解，记为：
$$
\color{red}P_{l}^{m}(x)=(-1)^m(1-x^2)^{m/2}\frac{\mathrm{d}^{m}P_{l}(x)}{\mathrm{d}x^{m}}\tag{15.2.28}
$$
称为 $m$ 阶**连带 Legendre 多项式**。

因此如果 $l\in[0,n]$，对于每一个 $l$，$m\in\{0,\dots,l\}$。

$(14.1.9)$ 可以改写为
$$
\Phi(\phi)=Ce^{im\phi},m\in\{0,\pm1,\dots,\pm l\}
$$
现在独立的解的个数是 $2l+1$ 个，$m$ 的取值也已经扩大，因此我们需要扩展连带 Legendre 多项式中 $m$ 的取值。利用 Rodriques 公式：
$$
P_{l}^{m}(x)=\frac{(-1)^m}{2^ll!}(1-x^2)^{m/2}\frac{\mathrm{d}^{l+m}}{\mathrm{d}x^{l+m}}(x^2-1)^l
$$
将 $m$ 换成 $-m$，则：
$$
P_{l}^{-m}(x)=\frac{(-1)^m}{2^ll!}(1-x^2)^{-m/2}\frac{\mathrm{d}^{l-m}}{\mathrm{d}x^{l-m}}(x^2-1)^l
$$
可以验证，二者相差常数倍：
$$
\begin{align*}
\frac{P_l^m(x)}{P_l^{-m}(x)}&=\frac{(1-x^2)^{m}\dfrac{\mathrm{d}^{l+m}}{\mathrm{d}x^{l+m}}(x^2-1)^l}{\dfrac{\mathrm{d}^{l-m}}{\mathrm{d}x^{l-m}}(x^2-1)^l}\\
&=\frac{(-1)^mx^{2m}\dfrac{(2l)!}{2^ll!(l-m)!}x^{l-m}}{\dfrac{(2l)!}{2^ll!(l+m)!}x^{l+m}}\\
&=(-1)^m\frac{(l+m)!}{(l-m)!}
\end{align*}
$$
因此
$$
P_l^m(x)=(-1)^m\frac{(l+m)!}{(l-m)!}P_l^{-m}(x)\tag{15.2.33}
$$
以下是前几阶的连带 Legendre 多项式：
$$
\begin{align*}
P_0^0(x)&=1\\
P_1^{-1}&=\frac{1}{2}\sqrt{1-x^2}&P_1^0&=x\\
P_1^1&=-\sqrt{1-x^2}\\
P_2^{-2}&=\frac{1}{8}(1-x^2)&P_2^{-1}&=\frac{1}{2}x\sqrt{1-x^2}&P_2^0&=\frac{1}{2}(3x^2-1)\\
P_2^2&=3(1-x^2)&P_2^1&=-3x\sqrt{1-x^2}
\end{align*}
$$

### 15.2.3 连带 Legendre 多项式的性质

连带 Legendre 多项式满足正交性。

其模长满足
$$
\color{red}\int_{-1}^{1}[P_l^m(x)]^2\mathrm{d}x=\frac{2}{2l+1}\frac{(l+m)!}{(l-m)!}\quad(m\le l)\tag{15.2.37}
$$
证明 pass。

### 15.2.4 Sphere Harmonics

把角度函数 $Y(\theta,\phi)=\Theta(\theta)\Phi(\phi)$ 的两部分解拼起来：
$$
\begin{align*}
Y_{l}^{m}(\theta,\phi)&=\Theta_{l}^{m}(\theta)\Phi(\phi)\\
&=P_{l}^{m}(\cos\theta)\exp(im\phi)
\end{align*}\tag{15.2.46}
$$
其中 $l\in\{0,\dots,n\},m\in\{0,\dots,\pm l\}$。

### 15.2.5 Sphere Harmonics 的性质

考查球谐函数的正交性：
$$
\begin{align*}
&\quad\ \int_{\mathbb{S}^2}[Y_l^m]^*Y_{l'}^{m'}\mathrm{d}\Omega\\
&=\int_0^{\pi}\int_{0}^{2\pi}[Y_l^m]^*(\theta,\phi)Y_{l'}^{m'}(\theta,\phi)\sin\theta\mathrm{d}\theta\mathrm{d}\phi\\
&=\int_0^{\pi}P_l^m(\cos\theta)P_{l'}^{m'}(\cos\theta)\sin\theta\mathrm{d}\theta\int_{0}^{2\pi}\exp(-im\phi)\exp(im'\phi)\mathrm{d}\phi\\
&=\int_{-1}^{1}P_l^m(x)P_{l'}^{m'}(x)\mathrm{d}\theta\int_{0}^{2\pi}\exp[i(m'-m)\phi]\mathrm{d}\phi\\
&=\frac{2}{2l+1}\frac{(l+m)!}{(l-m)!}\cdot2\pi\delta_{mm'}^2\\
&=\frac{4\pi}{2l+1}\frac{(l+m)!}{(l-m)!}\delta_{mm'}^2
\end{align*}
$$
归一化后得到：
$$
Y_{l}^{m}(\theta,\phi)=\sqrt{\frac{(2l+1)}{4\pi}\frac{(l-m)!}{(l+m)!}}P_{l}^{m}(\cos\theta)\exp(im\phi)\\
l\in\{0,\dots,n\},m\in\{-l,\dots,l\},\theta\in[0,\pi],\phi\in[0,2\pi]\tag{15.2.69}
$$
