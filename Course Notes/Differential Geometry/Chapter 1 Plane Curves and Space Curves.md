# Chapter 1 Plane Curves and Space Curves

## 1.1 Concept of Curves

In differential geometry, the most useful way to express a curve is by "parametric representation".
$$
x=x(t),y=y(t)\tag{1.1.1}
$$
In differential geometry, we assume that functions $x(t)$ and $y(t)$ are not only continuous, but also differentiable in $t$, at least two or three times. Sometimes we assume that $x(t), y(t)$ are not differentiable at some points.

## 1.2 **Local Theorems** on Plane Curves

|     Symbols     |         Descriptions          |
| :-------------: | :---------------------------: |
| $\mathbf{p}(t)$ |          vector form          |
|    $\dot{x}$    |   $\mathrm{d}x/\mathrm{d}t$   |
|   $\ddot{x}$    | $\mathrm{d}^2x/\mathrm{d}t^2$ |

Let $s$ be the distance the point moved between time $0$ and $t$.
$$
s(t)=\int_0^t|\dot{\mathbf{p}}(u)|\mathrm{d}u\tag{1.2.6}
$$
Thus:
$$
\dot{s}(t)=|\dot{\mathbf{p}}(t)|\tag{1.2.7}
$$
If $\dot{\mathbf{p}}(t)\ne\mathbf{0}$, (so that $\dot{s}(t)>0$) for all $t$, then $s$ will be a monotone increasing function of $t$, and it is possible to write $t$ as a function of $s$. We can express the curve using the length parameter $s$.
$$
\mathbf{p}=\mathbf{p}(s)=(x(s),y(s))\tag{1.2.8}
$$
Then the length of curve represented by $s$ is:
$$
\begin{align*}
|\mathbf{p}'(s)|&=\sqrt{\left(\frac{\mathrm{d}x}{\mathrm{d}s}\right)^2+\left(\frac{\mathrm{d}y}{\mathrm{d}s}\right)^2}=\sqrt{\left(\frac{\mathrm{d}x}{\mathrm{d}t}\frac{\mathrm{d}t}{\mathrm{d}s}\right)^2+\left(\frac{\mathrm{d}y}{\mathrm{d}t}\frac{\mathrm{d}t}{\mathrm{d}s}\right)^2}\\
&=\sqrt{\left(\frac{\mathrm{d}x}{\mathrm{d}t}\right)^2+\left(\frac{\mathrm{d}y}{\mathrm{d}t}\right)^2}\frac{\mathrm{d}t}{\mathrm{d}s}=\frac{\mathrm{d}s}{\mathrm{d}t}\frac{\mathrm{d}t}{\mathrm{d}s}\\
&=1
\end{align*}\tag{1.2.10}
$$
Let $\mathbf{e}_1(s)=\mathbf{p}'(s)$​, the equation of the tangent line at that point is
$$
\mathbf{p}(s)=\mathbf{p}(s_0)+\mathbf{e}_1(s_0)(s-s_0)\tag{1.2.12}
$$
Also, it is the first order approximation at $s_0$.

Let $\mathbf{e}_2(s_0)$ be a vector perpendicular to $\mathbf{e}_1(s_0)$ with length 1. We choose $\mathbf{e}_2(s_0)$ the one pointing to the left of $\mathbf{e}_1(s_0)$, that is:
$$
\mathbf{e}_2(s_0)=\begin{bmatrix}0&-1\\1&0\end{bmatrix}\mathbf{e}_1(s_0)
$$

> Let
> $$
> x(t)=r\cos t,y(t)=r\sin t\tag{1.2.14}
> $$
> Then the length parameter is:
> $$
> s=\int_0^t\sqrt{\dot{x}^2(u)+\dot{y}^2(u)}\mathrm{d}u=\int_0^tr\mathrm{d}u=rt\tag{1.2.15}
> $$
>  Then replace $t$ by $s$
> $$
> \begin{align*}
> x(s)&=r\cos\frac{s}{r}&y(t)&=r\sin\frac{s}{r}\tag{1.2.16}\\
> \dot{x}(s)&=-\sin\frac{s}{r}&\dot{y}(t)&=\cos\frac{s}{r}\tag{1.2.17}\\
> \mathbf{e}_1(s)&=\left(-\sin\frac{s}{r},\cos\frac{s}{r}\right)&\mathbf{e}_2(s)&=\left(-\cos\frac{s}{r},-\sin\frac{s}{r}\right)\tag{1.2.18}
> \end{align*}
> $$
> By differentiating $\mathbf{e}_1$ and $\mathbf{e}_2$ by $s$:
> $$
> \begin{align*}
> \mathbf{e}_1'(s)&=\left(-\frac{1}{r}\cos\frac{s}{r},-\frac{1}{r}\sin\frac{s}{r}\right)=\frac{1}{r}\mathbf{e}_2(s)\\
> \mathbf{e}_2'(s)&=\left(\frac{1}{r}\sin\frac{s}{r},-\frac{1}{r}\cos\frac{s}{r}\right)=-\frac{1}{r}\mathbf{e}_1(s)
> \end{align*}\tag{1.2.20}
> $$

In general:
$$
\mathbf{e}_1\cdot\mathbf{e}_1=1,\mathbf{e}_2\cdot\mathbf{e}_2=1,\mathbf{e}_1\cdot\mathbf{e}_2=0\tag{1.2.21}
$$
By differentiating, we get:
$$
2\mathbf{e}_1'\cdot\mathbf{e}_1=0
$$
Then $\mathbf{e}_1'$ is perpendicular to $\mathbf{e}_1$, then:
$$
\mathbf{e}_1'(s)=\kappa(s)\mathbf{e}_2(s)\tag{1.2.22}
$$
We can obtain the relationship by:
$$
\begin{align*}
\mathbf{e}_1'\cdot\mathbf{e}_2+\mathbf{e}_1\cdot\mathbf{e}_2'&=0\\
\kappa\mathbf{e}_2\cdot\mathbf{e}_2+\mathbf{e}_1\cdot\mathbf{e}_2'&=0\\
\kappa+\mathbf{e}_1\cdot\mathbf{e}_2'&=0\\
\mathbf{e}_2'&=-\kappa\mathbf{e}_1
\end{align*}\tag{1.2.23}
$$
Therefore:
$$
\begin{bmatrix}\mathbf{e}_1'\\\mathbf{e}_2'\end{bmatrix}=\begin{bmatrix}0&\kappa\\-\kappa&0\end{bmatrix}\begin{bmatrix}\mathbf{e}_1\\\mathbf{e}_2\end{bmatrix}\tag{1.2.24}
$$
We call this **$\kappa(s)$ the curvature of curve $\mathbf{p}(s)$.**

**Theorem 1.2.1** The curvature of $\mathbf{p}(s)$ is identically $0$, if and only if $\mathbf{p}(s)$ is a line.

> Sufficiency: Since curvature is identically 0, then $\mathbf{e}_1'(s)=\mathbf{0}$, and $\mathbf{e}_1(s)=(x'(s),y'(s))$ is a constant vector independent of $s$. Therefore, $\mathbf{p}(s)$ is a line.
>
> Necessity: And the curvature of the line is identically 0.

**Gauss Map**: as a point on the curve $\mathbf{p}$ moves from $\mathbf{p}(s)$ to $\mathbf{p}(s+\Delta{s})$ by a small distance $\Delta{s}$, $\mathbf{e}_2$ moves from $\mathbf{e}_2(s)$ to $\mathbf{e}_2(s+\Delta{s})$. Since
$$
\mathbf{e}_2(s+\Delta{s})=\mathbf{e}_2(s)+\mathbf{e}'_2(s)\Delta{s}+\cdots=\mathbf{e}_2(s)-\kappa(s)\mathbf{e}_1(s)\Delta{s}+\cdots\tag{1.2.27}
$$
therefore $\mathbf{e}_2(s)$ moves by distance $\kappa(s)\Delta{s}$ in the direction of $-\mathbf{e}_1(s)$.

Therefore, as $\mathbf{p}$ moves with speed 1, $\mathbf{e}_2$ rotates on the unit circle with speed $\kappa$. When $\kappa$ is negative, $\mathbf{e}_2$ rotates clockwise.

**Theorem 1.2.2** The curvature of $\mathbf{p}(s)$ is constant $\kappa\ne0$, if and only if $\mathbf{p}(s)$ is a circle with radius $1/\kappa$.

> Sufficiency: The center is $\mathbf{p}(s)+\mathbf{e}_2(s)/\kappa$ since differentiated by $s$ yield $\mathbf{0}$. The vector from any point to the center is $-\mathbf{e}_2(s)/\kappa$, which has a constant length $1/|\kappa|$.
>
> Necessity: And the curvature of the circle is constant $\kappa\ne0$.

**Theorem 1.2.3** A necessary and sufficient condition for the curvatures $\kappa(s)$ and $\bar{\kappa}(s)$ of two plane curves $\mathbf{p}(s)$ and $\bar{\mathbf{p}}(s)$ to coincide is that by suitable rotation and parallel displacement $\bar{\mathbf{p}}(s)$ can be superimposed on $\mathbf{p}(s)$.

> Sufficiency:
>
> Rotate $\mathbf{p}$ by an orthogonal matrix (rotation matrix) $A$ of size 2 and then displace it in parallel by a vector $\mathbf{a}$ so that the resulting curve $\bar{\mathbf{p}}$ is:
> $$
> \bar{\mathbf{p}}=A\mathbf{p}+\mathbf{a}\tag{1.2.29}
> $$
> Then:
> $$
> \bar{\mathbf{e}}_1=\bar{\mathbf{p}}'=A\mathbf{p}'=A\bar{\mathbf{e}}_1,\bar{\mathbf{e}}_2=A\mathbf{e}_2\tag{1.2.30}
> $$
> Necessity:
>
> By applying appropriate rotation and parallel displacement to $\mathbf{p}$ so that:
> $$
> \mathbf{p}(s_0)=\bar{\mathbf{p}}(s_0),\mathbf{e}_1(s_0)=\bar{\mathbf{e}}_1(s_0),\mathbf{e}_2(s_0)=\bar{\mathbf{e}}_2(s_0)\tag{1.2.33}
> $$
> for a fixed value $s_0$.
>
> Let:
> $$
> \begin{align*}
> \mathbf{e}_1&=(\xi_{11},\xi_{12})&\mathbf{e}_2&=(\xi_{21},\xi_{22})\\
> \bar{\mathbf{e}}_1&=(\bar{\xi}_{11},\bar{\xi}_{12})&\bar{\mathbf{e}}_2&=(\bar{\xi}_{21},\bar{\xi}_{22})\tag{1.2.34}\\
> X&=\begin{bmatrix}\xi_{11}&\xi_{12}\\\xi_{21}&\xi_{22}\end{bmatrix}&
> \bar{X}&=\begin{bmatrix}\bar{\xi}_{11}&\bar{\xi}_{12}\\\bar{\xi}_{21}&\bar{\xi}_{22}\end{bmatrix}\tag{1.2.35}
> \end{align*}
> $$
> Since $\mathbf{p}(s_0)=\bar{\mathbf{p}}(s_0)$, in order to show $\mathbf{p}(s)=\bar{\mathbf{p}}(s)$, we have only to prove:
> $$
> \frac{\mathrm{d}}{\mathrm{d}s}(\mathbf{p}(s)-\bar{\mathbf{p}}(s))=0
> $$
> The it is suffice to prove:
> $$
> \begin{align*}
> &(\xi_{11}-\bar{\xi}_{11})^2+(\xi_{21}-\bar{\xi}_{21})^2\\
> &=\xi_{11}^2+\xi_{21}^2+\bar{\xi}_{11}^2+\bar{\xi}_{21}^2-2(\xi_{11}\bar{\xi}_{11}+\xi_{21}\bar{\xi}_{21})\\
> &=2-2(\xi_{11}\bar{\xi}_{11}+\xi_{21}\bar{\xi}_{21})
> \end{align*}\tag{1.2.38}
> $$
> Since we have $\mathbf{e}'_1=\kappa\mathbf{e}_2$, $\mathbf{e}'_2=-\kappa\mathbf{e}_1$, then:
> $$
> \begin{align*}
> &\frac{\mathrm{d}}{\mathrm{d}s}(\xi_{11}\bar{\xi}_{11}+\xi_{21}\bar{\xi}_{21})\\
> &=\xi'_{11}\bar{\xi}_{11}+\xi_{11}\bar{\xi}'_{11}+\xi'_{21}\bar{\xi}_{21}+\xi_{21}\bar{\xi}'_{21}\\
> &=\kappa(\xi_{21}\bar{\xi}_{11}+\xi_{11}\bar{\xi}_{21}-\xi_{11}\bar{\xi}_{21}-\xi_{21}\bar{\xi}_{11})\\
> &=0
> \end{align*}\tag{1.2.37}
> $$
> And:
> $$
> \xi_{11}(s_0)\bar{\xi}_{11}(s_0)+\xi_{21}(s_0)\bar{\xi}_{21}(s_0)=\xi_{11}^2(s_0)+\xi_{21}^2(s_0)=1
> $$
> Then $\xi_{11}\bar{\xi}_{11}+\xi_{21}\bar{\xi}_{21}\equiv1$ and $(\xi_{11}-\bar{\xi}_{11})^2+(\xi_{21}-\bar{\xi}_{21})^2\equiv0$. It also holds for $(\xi_{12}-\bar{\xi}_{12})^2+(\xi_{22}-\bar{\xi}_{22})^2\equiv0$.

**Problem 1.2.1** Prove the equation of the curvature.

> First is length parameter $s$:
> $$
> \begin{align*}
> s(t)&=\int_{\alpha}^{t}\sqrt{\dot{x}^2(u)+\dot{y}^2(u)}\mathrm{d}u\\
> \frac{\mathrm{d}s}{\mathrm{d}t}&=\dot{s}(t)=\sqrt{\dot{x}^2(t)+\dot{y}^2(t)}
> \end{align*}
> $$
> Next is component vector expressed in $s$:
> $$
> \begin{align*}
> \mathbf{e}_1(s)&=\frac{\mathrm{d}\mathbf{p}}{\mathrm{d}t}\frac{\mathrm{d}t}{\mathrm{d}s}=\frac{1}{\sqrt{\dot{x}^2(t)+\dot{y}^2(t)}}(\dot{x}(t),\dot{y}(t))\\
> \mathbf{e}_2(s)&=\frac{1}{\sqrt{\dot{x}^2(t)+\dot{y}^2(t)}}(-\dot{y}(t),\dot{x}(t))
> \end{align*}
> $$
> Finally the curvature:
> $$
> \begin{align*}
> &\frac{\mathrm{d}\mathbf{e}_1}{\mathrm{d}s}=\frac{\mathrm{d}\mathbf{e}_1}{\mathrm{d}t}\frac{\mathrm{d}t}{\mathrm{d}s}\\
> &=\frac{\dot{x}(t)\ddot{y}(t)-\ddot{x}(t)\dot{y}(t)}{[\dot{x}^2(t)+\dot{y}^2(t)]^{2}}(-y(t),x(t))\\
> &=\kappa(s)\mathbf{e}_2(s)
> \end{align*}
> $$
> Then:
> $$
> \kappa(s)=\frac{\dot{x}(t)\ddot{y}(t)-\ddot{x}(t)\dot{y}(t)}{[\dot{x}^2(t)+\dot{y}^2(t)]^{3/2}}\tag{1.2.50}
> $$

**Problem 1.2.2** When the curve is given as $r=F(\theta)$ in terms of the polar coordinates $(r,\theta)$, find the curvature.

> From polar to Cartesian:
> $$
> \begin{align*}
> x&=r\cos\theta&y&=r\sin\theta\\
> \frac{\mathrm{d}x}{\mathrm{d}\theta}&=\frac{\mathrm{d}r}{\mathrm{d}\theta}\cos\theta-r\sin\theta&\frac{\mathrm{d}y}{\mathrm{d}\theta}&=\frac{\mathrm{d}r}{\mathrm{d}\theta}\sin\theta+r\cos\theta\\
> \frac{\mathrm{d}^2x}{\mathrm{d}\theta^2}&=\frac{\mathrm{d}^2r}{\mathrm{d}\theta^2}\cos\theta-2\frac{\mathrm{d}r}{\mathrm{d}\theta}\sin\theta-r\cos\theta&\frac{\mathrm{d}^2y}{\mathrm{d}\theta^2}&=\frac{\mathrm{d}^2r}{\mathrm{d}\theta^2}\sin\theta+2\frac{\mathrm{d}r}{\mathrm{d}\theta}\cos\theta-r\sin\theta\\
> 
> \end{align*}
> $$
> Replace them with above expression:
> $$
> \kappa(s)=\frac{r^2+2\left(\dfrac{\mathrm{d}r}{\mathrm{d}\theta}\right)-r\dfrac{\mathrm{d}^2r}{\mathrm{d}\theta^2}}{\left\{r^2+\left(\dfrac{\mathrm{d}r}{\mathrm{d}\theta}\right)^2\right\}^{3/2}}
> $$
> 

**Problem 1.2.3** Calculate the curvature of the curve $x=\cosh t$, $y=\sinh t$.

> First derivatives:
> $$
> \begin{align*}
> \dot{x}&=y&\dot{y}=x\\
> \ddot{x}&=x&\ddot{y}=y\\
> \end{align*}
> $$
> Then curvature:
> $$
> \kappa(t)=\frac{\dot{x}(t)\ddot{y}(t)-\ddot{x}(t)\dot{y}(t)}{[\dot{x}^2(t)+\dot{y}^2(t)]^{3/2}}=\frac{y^2-x^2}{[x^2+y^2]^{3/2}}=-\frac{1}{[\cosh^2t+\sinh^2t]^{3/2}}
> $$

**Problem 1.2.4** Let a be a positive constant number.
$$
y=a\cosh\frac{x}{a}
$$
Find the expression of the curve in terms of the parameter $s$.

> First $s(t)$:
> $$
> \begin{align*}
> s(t)&=\int_0^t\sqrt{1+\sinh^2\frac{u}{a}}\mathrm{d}u\\
> &=\int_0^t\cosh\frac{u}{a}\mathrm{d}u\\
> &=a\sinh\frac{t}{a}
> \end{align*}
> $$
> Then:
> $$
> t(s)=a\sinh^{-1}\frac{s}{a}
> $$
> Since:
> $$
> x=t,y=a\cosh\frac{t}{a}
> $$
> Then:
> $$
> x=a\sinh^{-1}\frac{s}{a},y=a\cosh\sinh^{-1}\frac{s}{a}=\sqrt{s^2+a^2}
> $$

## 1.3 **Global Theorems** on Plane Curves

We call a point $\mathbf{p}(s_0)$ where $\kappa'(s_0)=0$ a *vertex* of the curve.

At the point where $\kappa$ has its maximum or minimum, its derivative $\kappa'$ is $0$. Hence the four points where the ellipse intersects the $x$- and $y$- axis are its vertices. Since $\kappa$ is monotone increasing or decreasing in each of the four parts parted by the vertices, the word “vertex” seems appropriate at least for an ellipse.

If $\mathbf{p}(s_1)\ne\mathbf{p}(s_2)$ for other parameter values $s_1,s_2$ differ from $a,b$, it is called a simple closed curve.

When a line segment connecting arbitrary two points of a simple closed curve does not go outside the region enclosed by the curve, the curve is called an *oval* or a *convex closed curve*.

**Theorem 1.3.1** Every oval has at least four vertices.

> Let $\mathbf{p}(s) (a \le s \le b)$ be an oval. Consider the curvature $\kappa$ as a continuous function deﬁned on a closed interval $[a, b]$. Then as we have learned in calculus, $\kappa$ has the maximum and minimum. Thus assume that it has its <u>maximum at point $M$</u> on $\mathbf{p}$ and its <u>minimum at point $N$</u>. Using rotation and parallel displacement, move $M$ and $N$ so that they will be located on the x-axis.
>
> First assume that $M$ and $N$ are the only vertices. Then $\kappa$ is monotone increasing between $N$ and $M$, where $\kappa > 0$, and is monotone decreasing between $M$ and $N$, where $\kappa < 0$. Without loss of generality, the part above the $x$-axis is increasing part. Then:
> $$
> \int_a^b\kappa'(s)y(s)\mathrm{d}s>0\tag{1.3.1}
> $$
> Integrating by parts:
> $$
> \begin{align*}
> \int_a^b\kappa'(s)y(s)\mathrm{d}s&=[\kappa(s)y(s)]|_a^b-\int_a^b\kappa(s)y'(s)\mathrm{d}s\\
> &=-\int_a^b\kappa(s)y'(s)\mathrm{d}s
> \end{align*}\tag{1.3.2}
> $$
> Since $\kappa(a)=\kappa(b)$ and $y(a)=y(b)$. On the other hand, comparing the second component of $\mathbf{e}'_2=-\kappa\mathbf{e}_1=-\kappa\mathbf{p}'$, then:
> $$
> \xi'_{22}=-\kappa\xi_{11}=-\kappa y'\tag{1.3.3}
> $$
> Then:
> $$
> \int_a^b-\kappa(s)y'(s)\mathrm{d}s=\int_a^b\xi'_{22}(s)\mathrm{d}s=\xi_{22}(s)|_a^b=0\tag{1.3.4}
> $$
> This is a contradiction and we know that there exists at least another vertex $P$, other than $M$ and $N$.
>
> Let $P$ be located on the arc between $N$ and $M$. If there are no other vertices other than the three mentioned above, on the arc between $M$ and $N$ we still have $\kappa'<0$. If there is no point other than $P$ where $\kappa'=0$ on the arc between $N$ and $M$, this means that $\kappa$ does not decrease between $N$ and $M$. Thus when $\kappa$ moves from $N$ to $M$, at points other than $P$ we have $\kappa'>0$. Thus at points other than $M$, $N$, and $P$, we get $\kappa'(s)y(s)>0$, and this again contradicts.

**Rotation index**: number of rotations.
$$
m=\frac{1}{2\pi}\int_a^b\kappa(s)\mathrm{d}s\tag{1.3.5}
$$
As Gauss map tells, when $\mathbf{p}(s)$ moves with a constant speed 1, the normal vector $\mathbf{e}_2(s)$ rotates on a unit circle with the speed $\kappa(s)$. Thus the distance $\mathbf{e}_2(s)$ traveled on the circle, between $\mathbf{e}_2(a)$ and $\mathbf{e}_2(b)$ is given by,
$$
l=\int_a^b\kappa(s)\mathrm{d}s\tag{1.3.6}
$$
Since the curve is smoothly closed, then $\mathbf{p}(a)=\mathbf{p}(b)$ and $\mathbf{e}_2(a)=\mathbf{e}_2(b)$. It may rotate around the unit circle back and forth but will eventually reach its initial point. 

The change of rotation index in deformation: a family of closed curves $\mathbf{p}_r(s)$ parameterized by $r$, $0\le r\le1$, with $\mathbf{p}(s)=\mathbf{p}_0(s)$ and $\bar{\mathbf{p}}(s)=\mathbf{p}_1(s)$. Assume the velocity vector $\mathbf{e}_{1r}(s)$ and the acceleration vector $\mathbf{e}_{2r}(s)$ changes continuously with $r$. Since $\mathbf{e}_{1r}'(s)=\kappa_r(s)\mathbf{e}_{2r}(s)$, then the curvature also changes continuously with $r$. Then the rotation index:
$$
m_r=\frac{1}{2\pi}\int_{a_r}^{b_r}\kappa_r(s)\mathrm{d}s\tag{1.3.7}
$$
should also changes continuously with $r$, but since $m_r$ is a integer, it cannot change gradually. Thus $m_r$ does not change, meaning the rotation index of this family of curves are the same.

**Total curvature**: the integration of the absolute value of the curvature.
$$
\mu=\int_a^b|\kappa(s)|\mathrm{d}s\tag{1.3.8}
$$
**Theorem 1.3.2** The total curvature $\mu$ of a closed plane curve $\mathbf{p}(s)$ satisfies the inequality $\mu\ge2\pi$, and the equality $\mu=2\pi$ holds only when $\mathbf{p}(s)$ is an oval.

First prove $\mu\ge2\pi$:

> First, assume that the range of $\mathbf{e}_1(s)=(x'(s),y'(s))$, $(a\le s\le b)$ is smaller than a semi-circle. Then by rotating the circle, $\mathbf{e}_1(s)$ will belong to the half of a circumference, which means $y'(s)>0$, then:
> $$
> \int_a^by'(s)\mathrm{d}s=y(b)-y(a)>0\tag{1.3.9}
> $$
> But since $\mathbf{p}(s)$ is a closed curve, $y(a)=y(b)$, so this is a contradiction. Thus $\mathbf{e}_1(s)$ covers at least half of the circumference.
>
> Assume $\mathbf{e}_1(s)$ with $(a\le s\le c)$ with $c<b$ covers exactly half of the circumference. Thus we have $\mathbf{e}_1(a)=-\mathbf{e}_1(c)$, also $\mathbf{e}_2(a)=-\mathbf{e}_2(c)$. Since $s$ moves from $a$ to $c$, and as $s$ continue to moves from $c$ to $b$, the distance $\mathbf{e}_2(s)$ travels is at least $\pi$. Thus:
> $$
> \int_a^c|\kappa(s)|\mathrm{d}s\ge\pi\quad\int_c^b|\kappa(s)|\mathrm{d}s\ge\pi\tag{1.3.10}
> $$
> Thus $\mu\ge2\pi$.
>
> Let $\mu=2\pi$, which means $\kappa(s)$ is always $\kappa(s)\ge0$ or $\kappa(s)\le0$ and $\mathbf{e}_2(s)$ rotates along the unit circle once.

Then prove that the curve $\mathbf{p}(s)$ is a simple closed curve:

> If $\mathbf{p}(s)=(x(s),y(s))$ is not a simple closed curve, then assume $\mathbf{p}(a)=\mathbf{p}(s_0)$ for some $a<s_0<b$. Separate the curve into two parts $a\le s\le s_0$ and $s_0\le s\le b$. By rotating the curve, we may assume $y(a)=y(s_0)=y(b)$ is neither minimum nor maximum of $y(s)$ on both interval $[a,s_0]$ and $[s_0,b]$. On each interval, the maximum of $y(s)$ is larger than $y(a)$, the minimum is smaller than $y(a)$. Then there are at least 4 extrema of $y(s)$. At each extrema, the $\mathbf{e}_2(s)$ vector is in the vertical direction. Since $y(s)$ will go from one maximum/minimum to another maximum/minimum, then this contradicts the fact that when $s$ changes from $a$ to $b$, $\mathbf{e}_2(s)$ moves exactly once along the unit circle without going back and forth.

Then prove that the curve $\mathbf{p}(s)$ is an oval, that is convex:

> By proving the curve always lies on one side of its tangent line at each point. By rotating the curve, the tangent line is horizontal at $\mathbf{p}(s_0)$. Then there exists a maximum point $y(s_1)$ and minimum point $y(s_2)$ for $y(s)$. Then the tangent vectors are horizontal and normal vectors are vertical. Again, if there are more than one maximum/minimum,  $y(s)$ will go from one maximum/minimum to another maximum/minimum, then this contradicts the fact that when $s$ changes from $a$ to $b$, $\mathbf{e}_2(s)$ moves exactly once along the unit circle without going back and forth.

The Gauss map for an oval is one-to-one. For $a\le s\le b$, $0\le t \le2\pi$, then define:
$$
\mathbf{e}_1(s(t))=(-\sin t,\cos t)\quad \mathbf{e}_2(s(t))=(\cos t,\sin t)\tag{1.3.11}
$$
Since:
$$
\kappa(s)\mathbf{e}_2=\frac{\mathrm{d}\mathbf{e}_1}{\mathrm{d}s}=\frac{\mathrm{d}\mathbf{e}_1}{\mathrm{d}t}\frac{\mathrm{d}t}{\mathrm{d}s}=\mathbf{e}_2\frac{\mathrm{d}t}{\mathrm{d}s}\tag{1.3.12}
$$
Therefore for an oval: $\kappa(s)=\mathrm{d}t/\mathrm{d}s$.

**Width**: for one fixed $t_0$, the tangents at $p(t_0)$ and $p(t_0+\pi)$ are parallel to each other pointing in the opposite direction. We call this width between the two tangents the width of $p(t)$ at $t_0$ and write it as $W(t_0)$.  

**Problem 1.3.1**

(a) Check that the width $W(t)$ defined above is given by:
$$
W(t)=-\mathbf{p}(t)\cdot\mathbf{e}_2(t)-\mathbf{p}(t+\pi)\cdot\mathbf{e}_2(t+\pi)
$$

> First calculates the distance between two tangents. By projection:
> $$
> \begin{align*}
> W(t)&=(\mathbf{p}(t+\pi)-\mathbf{p}(t))\cdot\mathbf{e}_2(t)\\
> &=\mathbf{p}(t+\pi)\cdot\mathbf{e}_2(t)-\mathbf{p}(t)\cdot\mathbf{e}_2(t)
> \end{align*}
> $$
> Since $\mathbf{e}_2(t)=-\mathbf{e}_2(t+\pi)$:
> $$
> W(t)=-\mathbf{p}(t)\cdot\mathbf{e}_2(t)-\mathbf{p}(t+\pi)\cdot\mathbf{e}_2(t+\pi)
> $$

(b) Prove that the length $L$ of an oval $\mathbf{p}(t)$ is given by:
$$
L=\int_0^{\pi}W(t)\mathrm{d}t
$$

> Since:
> $$
> \begin{align*}
> L&=\int_0^{\pi}W(t)\mathrm{d}t\\
> &=-\int_0^{\pi}\mathbf{p}(t)\cdot\mathbf{e}_2(t)\mathrm{d}t-\int_0^{\pi}\mathbf{p}(t+\pi)\cdot\mathbf{e}_2(t+\pi)\mathrm{d}t\\
> &=-\int_0^{\pi}\mathbf{p}(t)\cdot\mathbf{e}_2(t)\mathrm{d}t-\int_{\pi}^{2\pi}\mathbf{p}(u)\cdot\mathbf{e}_2(u)\mathrm{d}u\\
> &=-\int_0^{2\pi}\mathbf{p}(t)\cdot\mathbf{e}_2(t)\mathrm{d}t
> \end{align*}
> $$
> And since:
> $$
> \frac{\mathrm{d}\mathbf{e}_1}{\mathrm{d}t}=\frac{\mathrm{d}\mathbf{e}_1}{\mathrm{d}s}\frac{\mathrm{d}s}{\mathrm{d}t}=\kappa(s)\mathbf{e}_2\frac{\mathrm{d}s}{\mathrm{d}t}=\mathbf{e}_2
> $$
>
> Then integral by parts:
> $$
> \begin{align*}
> -\int_0^{2\pi}\mathbf{p}(t)\cdot\mathbf{e}_2(t)\mathrm{d}t&=-[\mathbf{p}(t)\cdot\mathbf{e}_1(t)]|_{0}^{2\pi}+\int_0^{2\pi}\mathbf{e}_1(t)\cdot\mathbf{e}_1(t)\frac{\mathrm{d}s}{\mathrm{d}t}\mathrm{d}t\\
> &=\int_0^{2\pi}1\cdot\mathrm{d}s
> \end{align*}
> $$
> Then we done.

**Problem 1.3.2**

> Pass, I haven't learned Complex Analysis.
>

## 1.4 Space Curves

By differentiating:
$$
\mathbf{e}_i\cdot\mathbf{e}_j=\delta_{ij}\tag{1.4.5}
$$
we get
$$
\mathbf{e}'_i\cdot\mathbf{e}_j+\mathbf{e}_i\cdot\mathbf{e}'_j=0
$$

|       |                            $i=1$                             |                            $i=2$                             |                $i=3$                |
| :---: | :----------------------------------------------------------: | :----------------------------------------------------------: | :---------------------------------: |
| $j=1$ |             $2\mathbf{e}'_1\cdot\mathbf{e}_1=0$              |                              /                               |                  /                  |
| $j=2$ | $\mathbf{e}'_1\cdot\mathbf{e}_2+\mathbf{e}'_2\cdot\mathbf{e}_1=0$ |             $2\mathbf{e}'_2\cdot\mathbf{e}_2=0$              |                  /                  |
| $j=3$ | $\mathbf{e}'_1\cdot\mathbf{e}_3+\mathbf{e}'_3\cdot\mathbf{e}_1=0$ | $\mathbf{e}'_2\cdot\mathbf{e}_3+\mathbf{e}'_3\cdot\mathbf{e}_2=0$ | $2\mathbf{e}'_3\cdot\mathbf{e}_3=0$ |

If using length parameter $s$, $\mathbf{e}_1(s)$ will be unit length.

From $i=1,j=1$, $\kappa(s)$ the curvature of space curve $\mathbf{p}(s)$ is:
$$
\begin{align*}
\kappa(s)&=\sqrt{\mathbf{e}'_1(s)\cdot\mathbf{e}'_1(s)}\\
&=\sqrt{x''(s)^2+y''(s)^2+z''(s)^2}\ge0
\end{align*}\tag{1.4.3}
$$
And $\mathbf{e}_2(s)$ is defined as:
$$
\mathbf{e}_2(s)=\frac{1}{\kappa(s)}\mathbf{e}_1'(s)\tag{1.4.4}
$$
And by choosing right-handed system, let $\mathbf{e}_3=\mathbf{e}_1\times\mathbf{e}_2$, and $\mathbf{e}_1$, $\mathbf{e}_2$, $\mathbf{e}_3$ forms the **Frenet frame**. $\mathbf{e}_2$ is the principal normal, $\mathbf{e}_3$ is the binormal.

From $i=1,j=2$, we get $\kappa+\mathbf{e}'_2\cdot\mathbf{e}_1=0$.

From $i=2,j=2$, $\mathbf{e}'_2$ is perpendicular to $\mathbf{e}_2$, then it can be expressed by $\mathbf{e}_1$ and $\mathbf{e}_3$. Merge the condition above, we get $\mathbf{e}'_2=-\kappa\mathbf{e}_1+\tau\mathbf{e}_3$.

From $i=1,2,3,j=3$, we get $\mathbf{e}'_3=-\tau\mathbf{e}_2$.

Therefore:
$$
\begin{bmatrix}\mathbf{e}_1'\\\mathbf{e}_2'\\\mathbf{e}_3'\end{bmatrix}=\begin{bmatrix}0&\kappa&0\\-\kappa&0&\tau\\0&-\tau&0\end{bmatrix}\begin{bmatrix}\mathbf{e}_1\\\mathbf{e}_2\\\mathbf{e}_3\end{bmatrix}\tag{1.4.14}
$$
and called **Frenet-Serret's formula** of a space curve, and $\tau$ the **torsion**.

**Theorem 1.4.1** Assume that the curvature $\kappa$ of a space curve $\mathbf{p}(s)$ is everywhere positive. Then the torsion $\tau$ is zero everywhere if and only if $\mathbf{p}(s)$ lies on the plane .

> Sufficiency:
>
> Since $\tau=0$, then $\mathbf{e}'_3=\mathbf{0}$. Thus $\mathbf{e}_3$ is constant vector. Since $\mathbf{e}_1\cdot\mathbf{e}_3=0$, $(\mathbf{e}_3\cdot\mathbf{p}(s))'=\mathbf{e}'_3\cdot\mathbf{p}(s)+\mathbf{e}_3\cdot\mathbf{e}_1=0$, then $\mathbf{e}_3\cdot\mathbf{p}(s)$ is also a constant. Then $\mathbf{p}(s)$ lies on a plane.
>
> Necessity:
>
> Let $\mathbf{p}(s)$ be a plane curve, $\mathbf{a}\cdot\mathbf{p}\equiv0$ for $\mathbf{a}$ perpendicular to the plane. By differentiating,
> $$
> \mathbf{a}\cdot\mathbf{e}_1=0\quad\mathbf{a}\cdot\kappa\mathbf{e}_2=0\quad\mathbf{a}\cdot(-\kappa\mathbf{e}_1+\tau\mathbf{e}_3)=0\tag{1.4.16}
> $$
> Since $\kappa\ne0$, then $\mathbf{a}$ is perpendicular to $\mathbf{e}_1,\mathbf{e}_2$. Since $\mathbf{a}\cdot\mathbf{e}_3$ should not be 0, then $\tau=0$.

**Theorem 1.4.2** Let the curvature and the torsion of two space curves $\mathbf{p}(s)$ and $\bar{\mathbf{p}}(s)$ be $\kappa(s)$, $\tau(s)$, $\bar\kappa(s)$, and $\bar\tau(s)$, respectively. Then a necessary and sufficient condition for $\kappa=\bar\kappa$ and $\tau=\bar\tau$ is that by suitable rotation and parallel displacement $\bar{\mathbf{p}}(s)$ can be superimposed on $\mathbf{p}(s)$.

> Almost the same as [Theorem 1.2.2](#1.2 **Local Theorems** on Plane Curves).

**Gauss' spherical map**: *TODO*

**Problem 1.4.1/Bouquet's formula** Prove that the Taylor expansion of a space curve $\mathbf{p}(s)$ at $s = 0$ to its third term is given by:
$$
\begin{align*}
\mathbf{p}(s)&=\mathbf{p}(0)+\mathbf{e}_1(0)s+\kappa(0)\mathbf{e}_2(0)\frac{s^2}{2}\\
&+\{-\kappa(0)^2\mathbf{e}_1(0)+\kappa'(0)\mathbf{e}_2(0)+\kappa(0)\tau(0)\mathbf{e}_3(0)\}\frac{s^3}{3!}+\cdots
\end{align*}
$$

> $$
> \begin{align*}
> \frac{\mathrm{d}}{\mathrm{d}s}\mathbf{p}(s)&=\mathbf{e}_1(s)\\
> \frac{\mathrm{d}^2}{\mathrm{d}s^2}\mathbf{p}(s)&=\kappa(s)\mathbf{e}_2(s)\\
> \frac{\mathrm{d}^3}{\mathrm{d}s^3}\mathbf{p}(s)&=\kappa'(s)\mathbf{e}_2(s)+\kappa(s)(-\kappa\mathbf{e}_1(s)+\tau\mathbf{e}_3(s))\\
> \end{align*}
> $$
>
> The it is done.

**Problem 1.4.2** For a space curve $\mathbf{p}(t)$ prove
$$
\kappa=\frac{|\dot{\mathbf{p}}\times\ddot{\mathbf{p}}|}{|\dot{\mathbf{p}}|^3}\quad\tau=\frac{|\dot{\mathbf{p}}\ \ddot{\mathbf{p}}\ \dddot{\mathbf{p}}|}{|\dot{\mathbf{p}}\times\ddot{\mathbf{p}}|^2}
$$

> $$
> \dot{\mathbf{p}}=\frac{\mathrm{d}\mathbf{p}}{\mathrm{d}s}\frac{\mathrm{d}s}{\mathrm{d}t}=\mathbf{e}_1(s)\frac{\mathrm{d}s}{\mathrm{d}t}
> $$
>
> $$
> \ddot{\mathbf{p}}=\frac{\mathrm{d}\dot{\mathbf{p}}}{\mathrm{d}s}\frac{\mathrm{d}s}{\mathrm{d}t}=\kappa\mathbf{e}_2(s)\biggl(\frac{\mathrm{d}s}{\mathrm{d}t}\biggr)^2+\mathbf{e}_1(s)\frac{\mathrm{d}^2s}{\mathrm{d}t^2}
> $$
>
> $$
> \dddot{\mathbf{p}}=\frac{\mathrm{d}\ddot{\mathbf{p}}}{\mathrm{d}s}\frac{\mathrm{d}s}{\mathrm{d}t}=\kappa\tau\mathbf{e}_3(s)\biggl(\frac{\mathrm{d}s}{\mathrm{d}t}\biggr)^3+\cdots
> $$
>
> Then:
> $$
> |\dot{\mathbf{p}}\times\ddot{\mathbf{p}}|=\biggl|\kappa\mathbf{e}_3(s)\biggl(\frac{\mathrm{d}s}{\mathrm{d}t}\biggr)^3\biggr|=\biggl|\kappa\biggl(\frac{\mathrm{d}s}{\mathrm{d}t}\biggr)^3\biggr|
> $$
>
> $$
> |\dot{\mathbf{p}}|=\biggl|\mathbf{e}_1(s)\frac{\mathrm{d}s}{\mathrm{d}t}\biggr|=\biggl|\frac{\mathrm{d}s}{\mathrm{d}t}\biggr|
> $$
>
> $$
> |\dot{\mathbf{p}}\ \ddot{\mathbf{p}}\ \dddot{\mathbf{p}}|=\biggl|\mathbf{e}_1(s)\frac{\mathrm{d}s}{\mathrm{d}t}\ \kappa\mathbf{e}_2(s)\biggl(\frac{\mathrm{d}s}{\mathrm{d}t}\biggr)^2\ \kappa\tau\mathbf{e}_3(s)\biggl(\frac{\mathrm{d}s}{\mathrm{d}t}\biggr)^3\biggr|=\kappa^2\tau\biggl(\frac{\mathrm{d}s}{\mathrm{d}t}\biggr)^6
> $$

**Problem 1.4.3** Let $\mathbf{p}(s)$ be a curve in the $n$-dimensional Euclidean space, which is parameterized in such a way that its speed is 1. Then, prove that the Frenet-Serret's formula is:
$$
\mathbf{e}'_i=-\kappa_{i-1}\mathbf{e}_{i-1}+\kappa_i\mathbf{e}_{i+1}\quad\kappa_0=\kappa_n=0
$$

> 







