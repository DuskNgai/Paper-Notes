# Chapter 2 Local Theory of Surfaces in the Space

## 2.1 Concept of Surfaces in the Space

$$
x=x(u,v),y=y(u,v),z=z(u,v)
$$

The graph of $z=f(x,y)$ is a smooth surface if $f$ is diﬀerentiable.

The tangent plane at point $(x_0,y_0,z_0)$ on the surface is:
$$
F'_x(x_0,y_0,z_0)(x-x_0)+F'_y(x_0,y_0,z_0)(y-y_0)+F'_z(x_0,y_0,z_0)(z-z_0)=0
$$
At least one of the derivative must be nonzero. The tangent plane also can be:
$$
\begin{vmatrix}
x-x_0&y-y_0&z-z_0\\
x'_u(u_0,v_0)&y'_u(u_0,v_0)&z'_u(u_0,v_0)\\
x'_v(u_0,v_0)&y'_v(u_0,v_0)&z'_v(u_0,v_0)
\end{vmatrix}=0
$$
 where $x_0=x(u_0,v_0)$, $y_0=y(u_0,v_0)$, $z_0=z(u_0,v_0)$.

When th Jacobian matrix
$$
\begin{bmatrix}
x'_u&x'_v\\
y'_u&y'_v\\
z'_u&z'_v\\
\end{bmatrix}
$$
has rank 2 everywhere on $D$, then parametric representation defines a surface in space.

|          Shapes           |           Implicit           |                         Explicit                          |
| :-----------------------: | :--------------------------: | :-------------------------------------------------------: |
|          Sphere           |      $x^2+y^2+z^2=a^2$       |     $x=a\cos u\cos v$, $y=a\cos u\sin v$, $z=a\sin u$     |
|         Ellipsoid         | $x^2/a^2+y^2/b^2+z^2/c^2=1$  |     $x=a\cos u\cos v$, $y=b\cos u\sin v$, $z=c\sin u$     |
| Hyperboloid of one sheet  | $x^2/a^2+y^2/b^2-z^2/c^2=1$  |   $x=a\cosh u\cos v$, $y=b\cosh u\sin v$, $z=c\sinh u$    |
| Hyperboloid of two sheets | $x^2/a^2+y^2/b^2-z^2/c^2=-1$ |   $x=a\sinh u\cos v$, $y=b\sinh u\sin v$, $z=c\cosh u$    |
|    Elliptic paraboloid    |     $x^2/a^2+y^2/b^2=z$      |             $x=u$, $y=v$, $z=u^2/a^2+v^2/b^2$             |
|   Hyperbolic paraboloid   |     $-x^2/a^2+y^2/b^2=z$     |            $x=u$, $y=v$, $z=-u^2/a^2+v^2/b^2$             |
|           Torus           |              -               | $x=(R+r\cos u)\cos v$, $y=(R+r\cos u)\sin v$, $z=r\sin u$ |

**Problem 2.1.1** For each of the following surfaces, determine to which of the above examples it is equal.

> (i) $x=au\cos v,y=bu\sin v, z=u^2$

$$
\frac{x^2}{a^2}+\frac{y^2}{b^2}=z
$$

An elliptic paraboloid

> (ii) $x=au\cosh v,y=bu\sinh v, z=u^2$

$$
\frac{x^2}{a^2}-\frac{y^2}{b^2}=z
$$

A hyperbolic paraboloid.

> (iii) $x=a(u+v),y=b(u-v),z=4uv$

$$
\frac{x^2}{a^2}-\frac{y^2}{b^2}=z
$$

A hyperbolic paraboloid.

> (iv) $x=a(u-v)/(u+v),y=b(uv+1)/(u+v),z=c(uv-1)/(u+v)$

$$
\frac{x^2}{a^2}+\frac{y^2}{b^2}-\frac{z^2}{c^2}=1
$$

A hyperboloid of one sheet.

## 2.2 Fundamental Forms and Curvatures

The tangent space of the surface $\mathbf{p}(u,v)$ at point $\mathbf{p}(a,b)$ will be spanned by $\mathbf{p}'_u(a,b)$ and $\mathbf{p}'_v(a,b)$.

Let:
$$
\color{red}E=\mathbf{p}'_u\cdot\mathbf{p}'_u\\
\color{red}F=\mathbf{p}'_u\cdot\mathbf{p}'_v=\mathbf{p}'_v\cdot\mathbf{p}'_u\\
\color{red}G=\mathbf{p}'_v\cdot\mathbf{p}'_v
$$
each tangent vector can be written as a linear combination of $\mathbf{p}'_u$ and $\mathbf{p}'_v$: $\xi\mathbf{p}'_u+\eta\mathbf{p}'_v$. And the square of the length of this vector is:
$$
(\xi\mathbf{p}'_u+\eta\mathbf{p}'_v)\cdot(\xi\mathbf{p}'_u+\eta\mathbf{p}'_v)=E\xi^2+2F\xi\eta+G\eta^2
$$
Let $u=u(t)$, $v=v(t)$ be a curve in a region on the $(u,v)$-plane, $\mathbf{p}(t)=\mathbf{p}(u(t),v(t))$ be the corresponding curve on the surface. Then the tangent vector at each point on this curve $\mathbf{p}(t)$ is:
$$
\frac{\mathrm{d}\mathbf{p}}{\mathrm{d}t}=\frac{\partial\mathbf{p}}{\partial u}\frac{\mathrm{d}u}{\mathrm{d}t}+\frac{\partial\mathbf{p}}{\partial v}\frac{\mathrm{d}v}{\mathrm{d}t}=\mathbf{p}'_u\frac{\mathrm{d}u}{\mathrm{d}t}+\mathbf{p}'_v\frac{\mathrm{d}v}{\mathrm{d}t}
$$
The length of the curve from $\alpha$ to $\beta$ is:
$$
\int_{\alpha}^{\beta}\sqrt{\frac{\mathrm{d}\mathbf{p}}{\mathrm{d}t}\cdot\frac{\mathrm{d}\mathbf{p}}{\mathrm{d}t}}\mathrm{d}t=\int_{\alpha}^{\beta}\sqrt{E\left(\frac{\mathrm{d}u}{\mathrm{d}t}\right)^2+2F\frac{\mathrm{d}u}{\mathrm{d}t}\frac{\mathrm{d}v}{\mathrm{d}t}+G\left(\frac{\mathrm{d}v}{\mathrm{d}t}\right)^2}\mathrm{d}t
$$
The **ﬁrst fundamental form** (area differential) of the surface $p(u, v)$ is:
$$
\color{red}\mathbf{I}=E\mathrm{d}u\mathrm{d}u+2F\mathrm{d}u\mathrm{d}v+G\mathrm{d}v\mathrm{d}v
$$
Since:
$$
\mathrm{d}\mathbf{p}=\mathbf{p}'_u\mathrm{d}u+\mathbf{p}'_v\mathrm{d}v
$$
Then:
$$
\mathbf{I}=\mathrm{d}\mathbf{p}\cdot\mathrm{d}\mathbf{p}
$$
And it is a *positive definite form*.

For the normal vector $\mathbf{e}$ of the tangent plane:
$$
\mathbf{e}=\frac{\mathbf{p}'_u\times\mathbf{p}'_v}{|\mathbf{p}'_u\times\mathbf{p}'_v|}
$$
Then we have:
$$
\mathbf{p}'_u\cdot\mathbf{e}=0,\mathbf{p}'_v\cdot\mathbf{e}=0
$$
Partially diﬀerentiating this equation by $u$ and $v$ we have:
$$
\begin{align*}
\mathbf{p}''_{uu}\cdot\mathbf{e}+\mathbf{p}'_u\cdot\mathbf{e}_u&=0&\mathbf{p}''_{uv}\cdot\mathbf{e}+\mathbf{p}'_u\cdot\mathbf{e}_v&=0\\
\mathbf{p}''_{vu}\cdot\mathbf{e}+\mathbf{p}'_v\cdot\mathbf{e}_u&=0&\mathbf{p}''_{vv}\cdot\mathbf{e}+\mathbf{p}'_v\cdot\mathbf{e}_v&=0\\
\end{align*}
$$
Then define:
$$
\begin{align*}
L&=\mathbf{p}''_{uu}\cdot\mathbf{e}=-\mathbf{p}'_u\cdot\mathbf{e}'_u\\
M&=\mathbf{p}''_{uv}\cdot\mathbf{e}=-\mathbf{p}'_u\cdot\mathbf{e}'_v\\
M&=\mathbf{p}''_{vu}\cdot\mathbf{e}=-\mathbf{p}'_v\cdot\mathbf{e}'_u\\
N&=\mathbf{p}''_{vv}\cdot\mathbf{e}=-\mathbf{p}'_v\cdot\mathbf{e}'_v\\
\end{align*}
$$
Then define the **second fundamental form** of the surface by:
$$
\color{red}\begin{align*}
\mathbf{II}&=L\mathrm{d}u\mathrm{d}u+2M\mathrm{d}u\mathrm{d}v+N\mathrm{d}v\mathrm{d}v\\
&=-(\mathbf{p}'_u\mathrm{d}u+\mathbf{p}'_v\mathrm{d}v)\cdot(\mathbf{e}_u\mathrm{d}u+\mathbf{e}_v\mathrm{d}v)\\
&=-\mathrm{d}\mathbf{p}\cdot\mathrm{d}\mathbf{e}
\end{align*}
$$
At each point on the surface, $\mathbf{p}'_u$ , $\mathbf{p}'_v$ and $\mathbf{e}$ are linearly independent, so any vectors in the space can be represented as a linear combination of these three vectors.

**Gauss' equations**: $\Gamma$ are Christoffel's symbols
$$
\mathbf{p}''_{uu}=\Gamma_{uu}^u\mathbf{p}'_u+\Gamma_{uu}^v\mathbf{p}'_v+L\mathbf{e}\\
\mathbf{p}''_{uv}=\Gamma_{uv}^u\mathbf{p}'_u+\Gamma_{uv}^v\mathbf{p}'_v+M\mathbf{e}\\
\mathbf{p}''_{vu}=\Gamma_{vu}^u\mathbf{p}'_u+\Gamma_{vu}^v\mathbf{p}'_v+M\mathbf{e}\\
\mathbf{p}''_{vv}=\Gamma_{vv}^u\mathbf{p}'_u+\Gamma_{vv}^v\mathbf{p}'_v+N\mathbf{e}\\
$$
**Weingarten's equations**:
$$
\mathbf{e}'_u=\frac{FM-GL}{EG-F^2}\mathbf{p}_u+\frac{FL-EM}{EG-F^2}\mathbf{p}_v\\
\mathbf{e}'_v=\frac{FN-GM}{EG-F^2}\mathbf{p}_u+\frac{FM-EN}{EG-F^2}\mathbf{p}_v\\
$$

> Since $\mathbf{e}\cdot\mathbf{e}=1$, then $\mathbf{e}_u\cdot\mathbf{e}=0$, $\mathbf{e}_u$ is orthogonal to $\mathbf{e}$:
> $$
> \mathbf{e}'_u=A\mathbf{p}'_u+B\mathbf{p}'_v
> $$
> From the definition of $L,M,N$:
> $$
> -L=\mathbf{p}'_u\cdot\mathbf{e}'_u=\mathbf{p}'_u\cdot(A\mathbf{p}'_u+B\mathbf{p}'_v)=EA+FB\\
> -M=\mathbf{p}'_v\cdot\mathbf{e}'_u=\mathbf{p}'_v\cdot(A\mathbf{p}'_u+B\mathbf{p}'_v)=FA+GB\\
> $$
> Solve the linear system:
> $$
> \begin{cases}
> A=\dfrac{FM-GL}{EG-F^2}\\
> B=\dfrac{FL-EM}{EG-F^2}
> \end{cases}
> $$

**Theorem 2.2.1** At a point where the second fundamental form
$$
\mathbf{II}=L\mathrm{d}u\mathrm{d}u+2M\mathrm{d}u\mathrm{d}v+N\mathrm{d}v\mathrm{d}v
$$
is deﬁnite (either positive or negative), the surface is convex (concave if it is looked at from the opposite direction), and at a point where $\mathbf{II}$ is indeﬁnite, it is saddle shaped.

> In order to understand the geometric meaning of the second fundamental form, we ﬁx a unit vector $\mathbf{a}$ and deﬁne a function $f$ on the surface by:
> $$
> f(u,v)=\mathbf{a}\cdot\mathbf{p}(u,v)
> $$
> Now, ﬁx a point $\mathbf{p}_0=\mathbf{p}(u_0,v_0)$ on a surface, and let $\mathbf{a}$ be the normal vector $\mathbf{a}=\mathbf{e}(u_0,v_0)$ at that point. Since $\mathbf{a}\cdot\mathbf{p}'_u(u_0,v_0)=\mathbf{a}\cdot\mathbf{p}'_v(u_0,v_0)=0$, then
> $$
> \mathrm{d}f=\mathrm{d}(\mathbf{a}\cdot\mathbf{p})=\mathbf{a}\cdot\mathbf{p}'_u\mathrm{d}u+\mathbf{a}\cdot\mathbf{p}'_v\mathrm{d}v
> $$
> becomes $0$ at $\mathbf{p}_0=\mathbf{p}(u_0,v_0)$.
>
> The **Hessian** matrix:
> $$
> H_f=\begin{bmatrix}f''_{uu}&f''_{uv}\\f''_{vu}&f''_{vv}\end{bmatrix}
> $$
> Since:
> $$
> \begin{align*}
> f''_{uu}(u_0,v_0)&=\mathbf{a}(u_0,v_0)\cdot\mathbf{p}''_{uu}(u_0,v_0)=\mathbf{e}(u_0,v_0)\cdot\mathbf{p}''_{uu}(u_0,v_0)=L(u_0,v_0)\\
> f''_{uv}(u_0,v_0)&=\mathbf{a}(u_0,v_0)\cdot\mathbf{p}''_{uv}(u_0,v_0)=\mathbf{e}(u_0,v_0)\cdot\mathbf{p}''_{uv}(u_0,v_0)=M(u_0,v_0)\\
> f''_{vu}(u_0,v_0)&=\mathbf{a}(u_0,v_0)\cdot\mathbf{p}''_{vu}(u_0,v_0)=\mathbf{e}(u_0,v_0)\cdot\mathbf{p}''_{vu}(u_0,v_0)=M(u_0,v_0)\\
> f''_{vv}(u_0,v_0)&=\mathbf{a}(u_0,v_0)\cdot\mathbf{p}''_{vv}(u_0,v_0)=\mathbf{e}(u_0,v_0)\cdot\mathbf{p}''_{vv}(u_0,v_0)=N(u_0,v_0)\\
> \end{align*}
> $$
> If the second fundamental form $\mathbf{II}$ is positive-definite at $\mathbf{p}_0$, then the surface is concave in the direction of the normal, convex when negative-definite. When $\mathbf{II}$ is indeﬁnite, it is saddle shaped.













## Page 47 Fig. 2.2.4











