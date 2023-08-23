# Chapter 10 行波法

## 10.1 一维波动方程的通解

波动方程
$$
\frac{\partial^2{u}}{\partial{t}^2}=a^2\frac{\partial^2{u}}{\partial{x}^2}\tag{10.1.1}
$$
令
$$
\xi=x+at,\eta=x-at\tag{10.1.2}
$$
对 $x$ 的链式法则得到
$$
\begin{align*}
\frac{\partial{u}}{\partial{x}}&=\frac{\partial{u}}{\partial{\xi}}\frac{\partial{\xi}}{\partial{x}}+\frac{\partial{u}}{\partial{\eta}}\frac{\partial{\eta}}{\partial{x}}=\frac{\partial{u}}{\partial{\xi}}+\frac{\partial{u}}{\partial{\eta}}\\
\frac{\partial^2{u}}{\partial{x}^2}&=\frac{\partial}{\partial{\xi}}\left(\frac{\partial{u}}{\partial{\xi}}+\frac{\partial{u}}{\partial{\eta}}\right)\frac{\partial{\xi}}{\partial{x}}+\frac{\partial}{\partial{\eta}}\left(\frac{\partial{u}}{\partial{\xi}}+\frac{\partial{u}}{\partial{\eta}}\right)\frac{\partial{\eta}}{\partial{x}}\\
&=\frac{\partial^2{u}}{\partial{\xi}^2}+2\frac{\partial^2{u}}{\partial{\xi}\partial{\eta}}+\frac{\partial^2{u}}{\partial{\eta}^2}
\end{align*}\tag{10.1.3a}
$$
对 $t$ 的链式法则得到
$$
\begin{align*}
\frac{\partial{u}}{\partial{t}}&=\frac{\partial{u}}{\partial{\xi}}\frac{\partial{\xi}}{\partial{t}}+\frac{\partial{u}}{\partial{\eta}}\frac{\partial{\eta}}{\partial{t}}=a\left(\frac{\partial{u}}{\partial{\xi}}-\frac{\partial{u}}{\partial{\eta}}\right)\\
\frac{\partial^2{u}}{\partial{x}^2}&=a\frac{\partial}{\partial{\xi}}\left(\frac{\partial{u}}{\partial{\xi}}-\frac{\partial{u}}{\partial{\eta}}\right)\frac{\partial{\xi}}{\partial{x}}+a\frac{\partial}{\partial{\eta}}\left(\frac{\partial{u}}{\partial{\xi}}-\frac{\partial{u}}{\partial{\eta}}\right)\frac{\partial{\eta}}{\partial{x}}\\&=a^2\frac{\partial}{\partial{\xi}}\left(\frac{\partial{u}}{\partial{\xi}}-\frac{\partial{u}}{\partial{\eta}}\right)-a^2\frac{\partial}{\partial{\eta}}\left(\frac{\partial{u}}{\partial{\xi}}-\frac{\partial{u}}{\partial{\eta}}\right)\\
&=a^2\left(\frac{\partial^2{u}}{\partial{\xi}^2}-2\frac{\partial^2{u}}{\partial{\xi}\partial{\eta}}+\frac{\partial^2{u}}{\partial{\eta}^2}\right)
\end{align*}\tag{10.1.3b}
$$
带入二者得到
$$
\frac{\partial^2{u}}{\partial{\xi}\partial{\eta}}=0\tag{10.1.4}
$$
求解该方程，先对 $\eta$ 积分得到
$$
\frac{\partial{u}}{\partial{\xi}}=C_1\tag{10.1.5}
$$
其中，$C_1$ 是对 $\eta$ 积分而出现的常数，因此 $C_1$ 必须包含 $\xi$。因此改写为
$$
\frac{\partial{u}}{\partial{\xi}}=f(\xi)\tag{10.1.6}
$$
$f(\xi)$ 是 $\xi$ 的任意可微函数。再对 $\xi$ 积分得到
$$
u=\int f(\xi)\mathrm{d}\xi+C_2\tag{10.1.7}
$$
同理 $C_2$ 必须包含 $\eta$。因此改写为
$$
u(\xi,\eta)=f(\xi)+g(\eta)\tag{10.1.8}
$$
即
$$
u(x,t)=f(x+at)+g(x-at)\tag{10.1.9}
$$
其中 $f$, $g$ 都是二次连续可微函数。因此上述式子是波动方程的通解。

## 10.2 一维波动方程的达朗贝尔公式

### 10.2.1 达朗贝尔公式的推导

考虑带初始条件的无界弦的自由振动：
$$
\begin{cases}
\dfrac{\partial^2{u}}{\partial{t}^2}=a^2\dfrac{\partial^2{u}}{\partial{x}^2}&(-\infty<t<\infty,t>0)\\
u|_{t=0}=\phi(x),\dfrac{\partial{u}}{\partial{t}}\biggr|_{t=0}=\psi(x)&(-\infty<t<\infty)
\end{cases}\tag{10.2.1}
$$
带入 $(10.1.9)$ 的解到边界条件，我们有
$$
\begin{align*}
f(x)+g(x)&=\phi(x)\\
af'(x)-ag'(x)&=\psi(x)
\end{align*}\tag{10.2.2}
$$
下式两边对 $x$ 积分，得到
$$
f(x)-g(x)=\frac{1}{a}\int_0^x\psi(\xi)\mathrm{d}\xi+f(0)+g(0)\tag{10.2.4}
$$
与上式联立可以得到
$$
\begin{align*}
f(x)&=\frac{1}{2}\phi(x)+\frac{1}{2a}\int_0^x\psi(\xi)\mathrm{d}\xi+\frac{f(0)+g(0)}{2}\\
g(x)&=\frac{1}{2}\phi(x)-\frac{1}{2a}\int_0^x\psi(\xi)\mathrm{d}\xi-\frac{f(0)+g(0)}{2}
\end{align*}\tag{10.2.5}
$$
带入通解得到**达朗贝尔（d'Alembert）公式**
$$
\color{red}u(x,t)=\frac{1}{2}[\phi(x+at)+\phi(x-at)]+\frac{1}{2a}\int_{x-at}^{x+at}\psi(\xi)\mathrm{d}\xi\tag{10.2.6}
$$

### 10.2.2 达朗贝尔公式的讨论

达朗贝尔公式中的积分值只依赖于初始速度 $\psi(x)$ 在区间 $[x-at,x+at]$ 内的变化行为，这意味着特解 $u(x,t)$ 只依赖于该区间的初始条件，而与其他点上的初始条件无关。设 $X$ 是该区间内的任意一点，则
$$
x-at\le X\le x+at\tag{10.2.12}
$$
令
$$
x-at=X_1,x+at=X_2\tag{10.2.13}
$$
并改写为
$$
t=\frac{x}{a}-\frac{X_1}{a},t=-\frac{x}{a}+\frac{X_2}{a}\tag{10.2.14}
$$
在 $xOt$ 平面上，上式表示为斜率为 $\pm1/a$，截距分别为 $-X_1/a,X_2/a$ 的两条直线，他们与直线 $t=0$ 围成一个三角形，该三角形内部的点的集合相应于达朗贝尔公式中的积分区域，称为 “**决定区域**”。而这个区域的边界线段 $X_1X_2$ ，即 $[x-at,x+at]$ 相应于达朗贝尔公式中 $t=0$ 时的积分区间，称为 “**依赖区间**”。在 $t=0$ 时刻，依赖区间上任意点 $x$ 的变化范围是
$$
X_1\le x\le X_2\tag{10.2.15}
$$
经过 $t$ 时间后，任意点 $x$ 的变化范围是
$$
X_1-at\le x\le X_2+at\tag{10.2.16}
$$
该区域的边界是
$$
x=X_1-at,x=X_2+at\tag{10.2.17}
$$
这两条直线与直线 $t=0$ 围成的区域相应于初始扰动作用 $t$ 时间后所影响到的区域，称为 “影响区域”。

## 10.6 三维波动方程

$$
\begin{cases}
\dfrac{\partial^2{u}}{\partial{t}^2}=a^2\nabla^2u&(-\infty<x,y,z<+\infty,t>0)\\
u|_{t=0}=\phi(x,y,z)&(-\infty<x,y,z<+\infty)\\
\dfrac{\partial{u}}{\partial{t}}\biggl|_{t=0}=\psi(x,y,z)&(-\infty<x,y,z<+\infty)\\
\end{cases}\tag{10.6.0}
$$

### 10.6.1 三维波动方程的球对称解

波动方程在球坐标系中表示为
$$
\frac{1}{r^2}\frac{\partial}{\partial{r}}\left(r^2\frac{\partial{u}}{\partial{r}}\right)+\frac{1}{r^2\sin\theta}\frac{\partial}{\partial{\theta}}\left(\sin\theta\frac{\partial{u}}{\partial{\theta}}\right)+\frac{1}{r^2\sin^2\theta}\frac{\partial^2{u}}{\partial{\phi}^2}=\frac{1}{a^2}\frac{\partial^2{u}}{\partial{t}^2}\tag{10.6.2}
$$
当 $u$ 不依赖于角变量 $\theta$, $\phi$ 时，化简为
$$
\frac{1}{r^2}\frac{\partial}{\partial{r}}\left(r^2\frac{\partial{u}}{\partial{r}}\right)=\frac{1}{a^2}\frac{\partial^2{u}}{\partial{t}^2}\tag{10.6.3}
$$
由于
$$
\frac{1}{r^2}\frac{\partial}{\partial{r}}\left(r^2\frac{\partial{u}}{\partial{r}}\right)=\frac{\partial^2{u}}{\partial{r}^2}+\frac{2}{r}\frac{\partial{u}}{\partial{r}}=\frac{1}{r}\frac{\partial^2(ru)}{\partial{r}^2}\tag{10.6.4}
$$
改写 $(10.6.3)$ 为
$$
\frac{\partial^2(ru)}{\partial{t}^2}=a^2\frac{\partial^2(ru)}{\partial{r}^2}\tag{10.6.5}
$$
这是一个以 $ru$ 为变量的一维波动方程，通解为
$$
u(r,t)=\frac{f(r+at)}{r}+\frac{g(r-at)}{r}\tag{10.6.6}
$$
这就是三维波动方程的球对称解，其中，$f$ 和 $g$ 是两个任意二次连续可微函数。

### 10.6.2 三维波动方程的泊松公式

考虑三维波动方程的一般情况，我们的目的是求任意 $t$ 时刻在空间任意点的波函数 $u(x,y,z,t)$。对于球对称情况，波函数只是 $r$ 与 $t$ 的函数。如果我们不去考虑波函数本身，而是考虑 $u$ 在以 $(x,y,z)$ 为球心，以 $r$ 为半径的球面上的平均值，则这个平均值在 $(x,y,z)$ 暂时固定后就只与 $r,t$ 有关了。

为此，以 $M:(x,y,z)$ 点为球心，以 $r$ 为半径作一个球面 $S_r^M$，则波函数在球面上的平均值为
$$
\langle u(r,t)\rangle\equiv\frac{1}{4\pi r^2}\iint_{S_r^M}u(M',t)\mathrm{d}S\tag{10.6.7}
$$
其中 $M':(\xi,\eta,\delta)$ 是球面上的动点，$\mathrm{d}S$ 是球面上的面积微元，进而在 $r\to0$ 的情况下求极限
$$
\lim_{r\to0}\langle u(r,t)\rangle=u(x,y,z,t)\tag{10.6.9}
$$
它就是任意 $t$ 时刻在空间中任意点 $(x,y,z)$ 的波函数。用 $\langle u(r,t)\rangle$ 求解 $(10.6.0)$ 的方法用到了达朗贝尔公式，改写它为
$$
u(x,t)=\frac{\partial}{\partial{t}}\left[\frac{t}{2at}\int_{x-at}^{x+at}\phi(\xi)\mathrm{d}\xi\right]+\frac{t}{2at}\int_{x-at}^{x+at}\psi(\xi)\mathrm{d}\xi\tag{10.6.10}
$$
注意到积分 $\langle f(x,t)\rangle\equiv\frac{1}{2at}\int_{x-at}^{x+at}f(\xi)\mathrm{d}\xi$ 是 $f(\xi)$ 在区间 $[x-at,x+at]$ 上的平均值，因此可以改写为
$$
u(x,t)=\frac{\partial}{\partial{t}}\left[t\langle\psi(x,t)\rangle\right]+t\langle\psi(x,t)\rangle\tag{10.6.11}
$$
如上的一维区间平均可以推广到三维球面平均，对应关系如下

|      |      一维区间       |                  三维球面                  |
| :--: | :-----------------: | :----------------------------------------: |
| 中心 |         $x$         |                 $(x,y,z)$                  |
| 半径 |        $at$         |                    $at$                    |
| 表达 | $\xi\in[x-at,x+at]$ | $(\xi-x)^2+(\eta-y)^2+(\delta-z)^2=(at)^2$ |

因此
$$
\langle f(x,y,z,t)\rangle\equiv\frac{1}{4\pi(at)^2}\iint_{S_{at}^{(x,y,z)}}f(\xi,\eta,\delta)\mathrm{d}S\tag{10.6.13}
$$
是 $f(\xi,\eta,\delta)$ 在球面 $S_{at}^{(x,y,z)}$ 上的平均值。对应的解的表达式为
$$
u(x,y,z,t)=\frac{1}{4\pi a}\frac{\partial}{\partial{t}}\left[\iint_{S_{at}^{(x,y,z)}}\frac{\phi(M')}{at}\mathrm{d}S\right]+\frac{1}{4\pi a}\iint_{S_{at}^{(x,y,z)}}\frac{\psi(M')}{at}\mathrm{d}S\tag{10.6.15}
$$
当 $\phi$ 是三次连续可微函数，$\psi$ 是二次连续可微函数时候，$(10.6.15)$ 就是解。

### 10.6.3 泊松公式的物理意义

进一步讨论 $(10.6.15)$ 的物理意义。设初始条件限于三维区域 $T_0\subset\mathbb{R}^3$，$d$ 和 $D$ 分别是考察点 $M$ 点到 $T_0$ 的最小和最大距离。$t$ 时刻 $M$ 点的波函数 $u$ 是由以 $M$ 为中心，$at$ 为半径的球面 $S_{at}^{M}$ 上的初始条件决定的。当 $at<d$，即 $t<d/a$ 时候，$u=0$，这表明扰动的“前锋”还未到达；当 $d\le at\le D$，即 $d/a\le t\le D/a$ 时，$u\ne0$，这表明扰动发生作用；当 $at>D$，即 $t>D/a$ 时，$u=0$，这表明扰动的“阵尾”已经过去。因此，当初始扰动限制在空间某局部范围内时，扰动有清晰的“前锋”和“阵尾”，即惠更斯原理或无后效现象。

从 $(10.6.15)$ 我们也可以得到二维波动方程初始问题的解，如果 $u$ 与 $z$ 无关，则
$$
\begin{cases}
\dfrac{\partial^2{u}}{\partial{t}^2}=a^2\nabla^2u&(-\infty<x,y<+\infty,t>0)\\
u|_{t=0}=\phi(x,y)&(-\infty<x,y<+\infty)\\
\dfrac{\partial{u}}{\partial{t}}\biggl|_{t=0}=\psi(x,y)&(-\infty<x,y<+\infty)\\
\end{cases}\tag{10.6.16}
$$
这里需要把 $(10.6.15)$ 中两个球面积分转化为沿圆域
$$
\Sigma_{at}^{M}:(\xi-x)^2+(\eta-y)^2\le(at)^2\tag{10.6.17}
$$
内的积分。以 $\psi$​ 函数为例，首先
$$
\delta=\pm\sqrt{(at)^2-(\xi-x)^2-(\eta-y)^2}+z
$$
根据第一类曲面积分的性质
$$
\begin{align*}
&\frac{1}{4\pi a}\iint_{S_{at}^{(x,y,z)}}\frac{\psi(M')}{at}\mathrm{d}S\\
&=\frac{1}{4\pi a}2\iint_{\Sigma_{at}^{(x,y)}}\frac{\psi(\xi,\eta)}{at}\sqrt{1+\left(\frac{\partial\delta}{\partial\xi}\right)^2+\left(\frac{\partial\delta}{\partial\eta}\right)^2}\mathrm{d}S\\
&=\frac{1}{2\pi a}\iint_{\Sigma_{at}^{(x,y)}}\frac{\psi(\xi,\eta)}{at}\sqrt{\frac{(at)^2}{(at)^2-(\xi-x)^2-(\eta-y)^2}}\mathrm{d}\xi\mathrm{d}\eta\\
&=\frac{1}{2\pi a}\iint_{\Sigma_{at}^{(x,y)}}\frac{\psi(\xi,\eta)}{\sqrt{(at)^2-(\xi-x)^2-(\eta-y)^2}}\mathrm{d}\xi\mathrm{d}\eta\\
\end{align*}\tag{10.6.21}
$$
因此二维情况的解为
$$
\begin{align*}
u(x,y,t)&=\frac{1}{2\pi a}\frac{\partial}{\partial{t}}\iint_{\Sigma_{at}^{(x,y)}}\frac{\phi(\xi,\eta)}{\sqrt{(at)^2-(\xi-x)^2-(\eta-y)^2}}\mathrm{d}\xi\mathrm{d}\eta\\
&+\frac{1}{2\pi a}\iint_{\Sigma_{at}^{(x,y)}}\frac{\psi(\xi,\eta)}{\sqrt{(at)^2-(\xi-x)^2-(\eta-y)^2}}\mathrm{d}\xi\mathrm{d}\eta\\
\end{align*}\tag{10.6.22}
$$
相比于三维的球面情况，二维圆域的积分在 $at>D$ 时，仍然包含了 $T_0$，因此 $u\ne0$，具有后效特性。

