# Chapter 1 Basic

## 1.2 Nabla Operator and Laplace Operator

### 1.2.2 Laplace Operator in Polar Coordinates

Cartesian coordinates to Polar coordinates:
$$
\begin{aligned}
x &= r \cos \theta \\
y &= r \sin \theta \\
\end{aligned}
$$

Polar coordinates to Cartesian coordinates:
$$
\begin{aligned}
r &= \sqrt{x^2 + y^2} \\
\theta &= \arctan \frac{y}{x} \\
\end{aligned}
$$

---

First-order derivative to $r$:
$$
\begin{aligned}
\frac{\partial r}{\partial x} &= \frac{x}{r} \\
\frac{\partial r}{\partial y} &= \frac{y}{r} \\
\end{aligned}
$$

First-order derivative to $\theta$:
$$
\begin{aligned}
\frac{\partial \theta}{\partial x} &= -\frac{y}{r^2} \\
\frac{\partial \theta}{\partial y} &= \frac{x}{r^2} \\
\end{aligned}
$$

---

Second-order derivative to $r$:
$$
\begin{aligned}
\frac{\partial^2 r}{\partial x^2} &= \frac{y^2}{r^3} \\
\frac{\partial^2 r}{\partial y^2} &= \frac{x^2}{r^3} \\
\end{aligned}
$$

Second-order derivative to $\theta$:
$$
\begin{aligned}
\frac{\partial^2 \theta}{\partial x^2} &= \frac{2xy}{r^4} \\
\frac{\partial^2 \theta}{\partial y^2} &= -\frac{2xy}{r^4} \\
\end{aligned}
$$

---

$$
\begin{aligned}
\nabla^2 &= \frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2} \\
&= \frac{\partial}{\partial x} \left(\frac{\partial}{\partial x}\right) + \frac{\partial}{\partial y} \left(\frac{\partial}{\partial y}\right) \\
&= \frac{\partial}{\partial x} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial x} + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial x}\right) + \frac{\partial}{\partial y} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial y} + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial y}\right) \\
&= \frac{\partial}{\partial x} \left(\frac{\partial}{\partial r}\right) \frac{\partial r}{\partial x} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial x^2} + \frac{\partial}{\partial x} \left(\frac{\partial}{\partial \theta}\right) \frac{\partial \theta}{\partial x} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial x^2} \\
&+ \frac{\partial}{\partial y} \left(\frac{\partial}{\partial r}\right) \frac{\partial r}{\partial y} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial y^2} + \frac{\partial}{\partial y} \left(\frac{\partial}{\partial \theta}\right) \frac{\partial \theta}{\partial y} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial y^2} \\
&= \frac{\partial}{\partial r} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial x} + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial x}\right) \frac{\partial r}{\partial x} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial x^2} \\
&+ \frac{\partial}{\partial \theta} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial x} + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial x}\right) \frac{\partial \theta}{\partial x} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial x^2} \\
&+ \frac{\partial}{\partial r} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial y} + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial y}\right) \frac{\partial r}{\partial y} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial y^2} \\
&+ \frac{\partial}{\partial \theta} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial y} + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial y}\right) \frac{\partial \theta}{\partial y} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial y^2} \\
&= \frac{\partial^2}{\partial r^2} \cdot \left(\frac{\partial r}{\partial x}\right)^2 + \frac{\partial^2}{\partial r \partial \theta} \frac{\partial \theta}{\partial x} \frac{\partial r}{\partial x} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial x^2} \\
&+ \frac{\partial^2}{\partial \theta \partial r} \frac{\partial r}{\partial x} \frac{\partial \theta}{\partial x} + \frac{\partial^2}{\partial \theta^2} \cdot \left(\frac{\partial \theta}{\partial x}\right)^2 + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial x^2} \\
&+ \frac{\partial^2}{\partial r^2} \cdot \left(\frac{\partial r}{\partial y}\right)^2 + \frac{\partial^2}{\partial r \partial \theta} \frac{\partial \theta}{\partial y} \frac{\partial r}{\partial y} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial y^2} \\
&+ \frac{\partial^2}{\partial \theta \partial r} \frac{\partial r}{\partial y} \frac{\partial \theta}{\partial y} + \frac{\partial^2}{\partial \theta^2} \cdot \left(\frac{\partial \theta}{\partial y}\right)^2 + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial y^2} \\
&= \frac{\partial^2}{\partial r^2} \cdot \left[\left(\frac{\partial r}{\partial x}\right)^2 + \left(\frac{\partial r}{\partial y}\right)^2\right] + \frac{\partial^2}{\partial \theta^2} \cdot \left[\left(\frac{\partial \theta}{\partial x}\right)^2 + \left(\frac{\partial \theta}{\partial y}\right)^2\right] \\
&+ \frac{\partial}{\partial r} \left[\frac{\partial^2 r}{\partial x^2} + \frac{\partial^2 r}{\partial y^2}\right] + \frac{\partial}{\partial \theta} \left[\frac{\partial^2 \theta}{\partial x^2} + \frac{\partial^2 \theta}{\partial y^2}\right] + 2 \frac{\partial^2}{\partial r \partial \theta} \left[\frac{\partial r}{\partial x} \frac{\partial \theta}{\partial x} + \frac{\partial r}{\partial y} \frac{\partial \theta}{\partial y}\right]
\end{aligned}
$$
We have:
$$
\begin{aligned}
\left(\frac{\partial r}{\partial x}\right)^2 + \left(\frac{\partial r}{\partial y}\right)^2 &= 1 \\
\left(\frac{\partial \theta}{\partial x}\right)^2 + \left(\frac{\partial \theta}{\partial y}\right)^2 &= \frac{1}{r^2} \\
\frac{\partial^2 r}{\partial x^2} + \frac{\partial^2 r}{\partial y^2} &= \frac{1}{r} \\
\frac{\partial^2 \theta}{\partial x^2} + \frac{\partial^2 \theta}{\partial y^2} &= 0 \\
\frac{\partial r}{\partial x} \frac{\partial \theta}{\partial x} + \frac{\partial r}{\partial y} \frac{\partial \theta}{\partial y} &= 0
\end{aligned}
$$
Therefore we get:
$$
\nabla^{2} = \frac{\partial^2}{\partial r^2} + \frac{1}{r^2} \frac{\partial^2}{\partial \theta^2} + \frac{1}{r} \frac{\partial}{\partial r}
$$

### 1.2.3 Laplace Operator in Spherical Coordinates

Cartesian coordinates to spherical coordinates:
$$
\begin{aligned}
x &= r \sin \theta \cos \phi \\
y &= r \sin \theta \sin \phi \\
z &= r \cos \theta
\end{aligned}
$$

Spherical coordinates to Cartesian coordinates:
$$
\begin{aligned}
r &= \sqrt{x^2 + y^2 + z^2} \\
\phi &= \arctan \frac{y}{x} \\
\theta &= \arccos \frac{z}{\sqrt{x^2 + y^2 + z^2}}
\end{aligned}
$$

## First Order Derivative

First order derivative to $r$:
$$
\begin{aligned}
\frac{\partial r}{\partial x} &= \frac{x}{\sqrt{x^2 + y^2 + z^2}} = \sin \theta \cos \phi \\
\frac{\partial r}{\partial y} &= \frac{y}{\sqrt{x^2 + y^2 + z^2}} = \sin \theta \sin \phi \\
\frac{\partial r}{\partial z} &= \frac{z}{\sqrt{x^2 + y^2 + z^2}} = \cos \theta \\
\end{aligned}
$$

First order derivative to $\phi$:
$$
\begin{aligned}
\frac{\partial \phi}{\partial x} &= -\frac{y}{x^2 + y^2} = -\frac{\sin \phi}{r \sin \theta} \\
\frac{\partial \phi}{\partial y} &= \frac{x}{x^2 + y^2} = \frac{\cos \phi}{r \sin \theta} \\
\frac{\partial \phi}{\partial z} &= 0 \\
\end{aligned}
$$

First order derivative to $\theta$:
$$
\begin{aligned}
\frac{\partial \theta}{\partial x} &= \frac{xz}{r^2 \sqrt{x^2 + y^2}} = \frac{\cos \theta \cos \phi}{r} \\
\frac{\partial \theta}{\partial y} &= \frac{yz}{r^2 \sqrt{x^2 + y^2}} = \frac{\cos \theta \sin \phi}{r} \\
\frac{\partial \theta}{\partial z} &= -\frac{x^2 + y^2}{r^2 \sqrt{x^2 + y^2}} = -\frac{\sin \theta}{r} \\
\end{aligned}
$$

## Second Order Derivative

Second order derivative to $r$:
$$
\begin{aligned}
\frac{\partial^2 r}{\partial x^2} &= \frac{r^2 - x^2}{r^3} \\
\frac{\partial^2 r}{\partial y^2} &= \frac{r^2 - y^2}{r^3} \\
\frac{\partial^2 r}{\partial z^2} &= \frac{r^2 - z^2}{r^3} \\
\end{aligned}
$$

Second order derivative to $\phi$:
$$
\begin{aligned}
\frac{\partial^2 \phi}{\partial x^2} &= \frac{2xy}{(x^2 + y^2)^2} \\
\frac{\partial^2 \phi}{\partial y^2} &= -\frac{2xy}{(x^2 + y^2)^2} \\
\frac{\partial^2 \phi}{\partial z^2} &= 0 \\
\end{aligned}
$$

Second order derivative to $\theta$:
$$
\begin{aligned}
\frac{\partial^2 \theta}{\partial x^2} &= \frac{\cos \theta - \cos \theta \cos^2 \phi (1 + \sin^2 \theta)}{r^2 \sin \theta} \\
\frac{\partial^2 \theta}{\partial y^2} &= \frac{\cos \theta - \cos \theta \sin^2 \phi (1 + \sin^2 \theta)}{r^2 \sin \theta} \\
\frac{\partial^2 \theta}{\partial z^2} &= -2\frac{\cos^2 \theta}{r^2} \\
\end{aligned}
$$

## Laplacian Operator

Here are some convensions:
$$
\frac{\partial}{\partial x}\left(\frac{\partial}{\partial x}\right) = \frac{\partial^2}{\partial x^2}
$$
means the first order derivative of the first order derivative to $x$.
$$
\frac{\partial}{\partial x}\frac{\partial}{\partial x} = \left(\frac{\partial}{\partial x}\right)^2
$$
means the multiplications of the first order derivative to $x$ and the first order derivative to $x$.

$$
\begin{aligned}
\nabla^2 &= \frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2} + \frac{\partial^2}{\partial z^2} \\
&= \frac{\partial}{\partial x} \left(\frac{\partial}{\partial x}\right) + \frac{\partial}{\partial y} \left(\frac{\partial}{\partial y}\right) + \frac{\partial}{\partial z} \left(\frac{\partial}{\partial z}\right) \\
&= \frac{\partial}{\partial x} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial x} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial x}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial x}\right) \\
&+ \frac{\partial}{\partial y} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial y} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial y} + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial y}\right) \\
&+ \frac{\partial}{\partial z} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial z} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial z} + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial z}\right) \\
&= \frac{\partial^2}{\partial r \partial x} \frac{\partial r}{\partial x} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial x^2} + \frac{\partial^2}{\partial \phi \partial x} \frac{\partial \phi}{\partial x} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial x^2} + \frac{\partial^2}{\partial \theta \partial x} \frac{\partial \theta}{\partial x} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial x^2} \\
&+ \frac{\partial^2}{\partial r \partial y} \frac{\partial r}{\partial y} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial y^2} + \frac{\partial^2}{\partial \phi \partial y} \frac{\partial \phi}{\partial y} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial y^2} + \frac{\partial^2}{\partial \theta \partial y} \frac{\partial \theta}{\partial y} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial y^2} \\
&+ \frac{\partial^2}{\partial r \partial z} \frac{\partial r}{\partial z} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial z^2} + \frac{\partial^2}{\partial \phi \partial z} \frac{\partial \phi}{\partial z} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial z^2} + \frac{\partial^2}{\partial \theta \partial z} \frac{\partial \theta}{\partial z} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial z^2} \\
&= \frac{\partial}{\partial r} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial x} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial x}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial x}\right) \frac{\partial r}{\partial x} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial x^2} \\
&+ \frac{\partial}{\partial \phi} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial x} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial x}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial x}\right) \frac{\partial \phi}{\partial x} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial x^2} \\
&+ \frac{\partial}{\partial \theta} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial x} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial x}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial x}\right) \frac{\partial \theta}{\partial x} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial x^2} \\
&+ \frac{\partial}{\partial r} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial x} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial y}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial y}\right) \frac{\partial r}{\partial y} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial y^2} \\
&+ \frac{\partial}{\partial \phi} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial y} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial y}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial y}\right) \frac{\partial \phi}{\partial y} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial y^2} \\
&+ \frac{\partial}{\partial \theta} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial y} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial y}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial y}\right) \frac{\partial \theta}{\partial y} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial y^2} \\
&+ \frac{\partial}{\partial r} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial z} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial z}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial z}\right) \frac{\partial r}{\partial z} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial z^2} \\
&+ \frac{\partial}{\partial \phi} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial z} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial z}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial z}\right) \frac{\partial \phi}{\partial z} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial z^2} \\
&+ \frac{\partial}{\partial \theta} \left(\frac{\partial}{\partial r} \frac{\partial r}{\partial z} + \frac{\partial}{\partial \phi} \frac{\partial \phi}{\partial z}\ + \frac{\partial}{\partial \theta} \frac{\partial \theta}{\partial z}\right) \frac{\partial \theta}{\partial z} + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial z^2} \\
&= \frac{\partial^2}{\partial r^2} \cdot \left(\frac{\partial r}{\partial x}\right)^2 + \frac{\partial^2}{\partial r \partial \phi} \frac{\partial \phi}{\partial x} \frac{\partial r}{\partial x} + \frac{\partial^2}{\partial r \partial \theta} \frac{\partial \theta}{\partial x} \frac{\partial r}{\partial x} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial x^2} \\
&+ \frac{\partial^2}{\partial \phi \partial r} \frac{\partial r}{\partial x} \frac{\partial \phi}{\partial x} + \frac{\partial^2}{\partial \phi^2} \cdot \left(\frac{\partial \phi}{\partial x}\right)^2 + \frac{\partial^2}{\partial \phi \partial \theta} \frac{\partial \theta}{\partial x} \frac{\partial \phi}{\partial x} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial x^2} \\
&+ \frac{\partial^2}{\partial \theta \partial r} \frac{\partial r}{\partial x} \frac{\partial \theta}{\partial x} + \frac{\partial^2}{\partial \theta \partial \phi} \frac{\partial \phi}{\partial x} \frac{\partial \theta}{\partial x} + \frac{\partial^2}{\partial \theta^2} \cdot \left(\frac{\partial \theta}{\partial x}\right)^2 + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial x^2} \\
&+ \frac{\partial^2}{\partial r^2} \cdot \left(\frac{\partial r}{\partial y}\right)^2 + \frac{\partial^2}{\partial r \partial \phi} \frac{\partial \phi}{\partial y} \frac{\partial r}{\partial y} + \frac{\partial^2}{\partial r \partial \theta} \frac{\partial \theta}{\partial y} \frac{\partial r}{\partial y} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial y^2} \\
&+ \frac{\partial^2}{\partial \phi \partial r} \frac{\partial r}{\partial y} \frac{\partial \phi}{\partial y} + \frac{\partial^2}{\partial \phi^2} \cdot \left(\frac{\partial \phi}{\partial y}\right)^2 + \frac{\partial^2}{\partial \phi \partial \theta} \frac{\partial \theta}{\partial y} \frac{\partial \phi}{\partial y} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial y^2} \\
&+ \frac{\partial^2}{\partial \theta \partial r} \frac{\partial r}{\partial y} \frac{\partial \theta}{\partial y} + \frac{\partial^2}{\partial \theta \partial \phi} \frac{\partial \phi}{\partial y} \frac{\partial \theta}{\partial y} + \frac{\partial^2}{\partial \theta^2} \cdot \left(\frac{\partial \theta}{\partial y}\right)^2 + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial y^2} \\
&+ \frac{\partial^2}{\partial r^2} \cdot \left(\frac{\partial r}{\partial z}\right)^2 + \frac{\partial^2}{\partial r \partial \phi} \frac{\partial \phi}{\partial z} \frac{\partial r}{\partial z} + \frac{\partial^2}{\partial r \partial \theta} \frac{\partial \theta}{\partial z} \frac{\partial r}{\partial z} + \frac{\partial}{\partial r} \frac{\partial^2 r}{\partial z^2} \\
&+ \frac{\partial^2}{\partial \phi \partial r} \frac{\partial r}{\partial z} \frac{\partial \phi}{\partial z} + \frac{\partial^2}{\partial \phi^2} \cdot \left(\frac{\partial \phi}{\partial z}\right)^2 + \frac{\partial^2}{\partial \phi \partial \theta} \frac{\partial \theta}{\partial z} \frac{\partial \phi}{\partial z} + \frac{\partial}{\partial \phi} \frac{\partial^2 \phi}{\partial z^2} \\
&+ \frac{\partial^2}{\partial \theta \partial r} \frac{\partial r}{\partial z} \frac{\partial \theta}{\partial z} + \frac{\partial^2}{\partial \theta \partial \phi} \frac{\partial \phi}{\partial z} \frac{\partial \theta}{\partial z} + \frac{\partial^2}{\partial \theta^2} \cdot \left(\frac{\partial \theta}{\partial z}\right)^2 + \frac{\partial}{\partial \theta} \frac{\partial^2 \theta}{\partial z^2} \\
&= \frac{\partial^2}{\partial r^2} \cdot \left[\left(\frac{\partial r}{\partial x}\right)^2 + \left(\frac{\partial r}{\partial y}\right)^2 + \left(\frac{\partial r}{\partial z}\right)^2\right] \\
&+ \frac{\partial^2}{\partial \phi^2} \cdot \left[\left(\frac{\partial \phi}{\partial x}\right)^2 + \left(\frac{\partial \phi}{\partial y}\right)^2 + \left(\frac{\partial \phi}{\partial z}\right)^2\right] \\
&+ \frac{\partial^2}{\partial \theta^2} \cdot \left[\left(\frac{\partial \theta}{\partial x}\right)^2 + \left(\frac{\partial \theta}{\partial y}\right)^2 + \left(\frac{\partial \theta}{\partial z}\right)^2\right] \\
&+ \frac{\partial}{\partial r} \left[\frac{\partial^2 r}{\partial x^2} + \frac{\partial^2 r}{\partial y^2} + \frac{\partial^2 r}{\partial z^2}\right] \\
&+ \frac{\partial}{\partial \phi} \left[\frac{\partial^2 \phi}{\partial x^2} + \frac{\partial^2 \phi}{\partial y^2} + \frac{\partial^2 \phi}{\partial z^2}\right] \\
&+ \frac{\partial}{\partial \theta} \left[\frac{\partial^2 \theta}{\partial x^2} + \frac{\partial^2 \theta}{\partial y^2} + \frac{\partial^2 \theta}{\partial z^2}\right] \\
&+ 2 \frac{\partial^2}{\partial r \partial \phi} \left[\frac{\partial \phi}{\partial x} \frac{\partial r}{\partial x} + \frac{\partial \phi}{\partial y} \frac{\partial r}{\partial y} + \frac{\partial \phi}{\partial z} \frac{\partial r}{\partial z}\right] \\
&+ 2 \frac{\partial^2}{\partial r \partial \theta} \left[\frac{\partial \theta}{\partial x} \frac{\partial r}{\partial x} + \frac{\partial \theta}{\partial y} \frac{\partial r}{\partial y} + \frac{\partial \theta}{\partial z} \frac{\partial r}{\partial z}\right] \\
&+ 2 \frac{\partial^2}{\partial \phi \partial \theta} \left[\frac{\partial \theta}{\partial x} \frac{\partial \phi}{\partial x} + \frac{\partial \theta}{\partial y} \frac{\partial \phi}{\partial y} + \frac{\partial \theta}{\partial z} \frac{\partial \phi}{\partial z}\right]
\end{aligned}
$$
We have:
$$
\begin{aligned}
\left(\frac{\partial r}{\partial x}\right)^2 + \left(\frac{\partial r}{\partial y}\right)^2 + \left(\frac{\partial r}{\partial z}\right)^2 &= 1 \\
\left(\frac{\partial \phi}{\partial x}\right)^2 + \left(\frac{\partial \phi}{\partial y}\right)^2 + \left(\frac{\partial \phi}{\partial z}\right)^2 &= \frac{1}{r^2 \sin^2 \theta} \\
\left(\frac{\partial \theta}{\partial x}\right)^2 + \left(\frac{\partial \theta}{\partial y}\right)^2 + \left(\frac{\partial \theta}{\partial z}\right)^2 &= \frac{1}{r^2} \\
\frac{\partial^2 r}{\partial x^2} + \frac{\partial^2 r}{\partial y^2} + \frac{\partial^2 r}{\partial z^2} &= \frac{2}{r} \\
\frac{\partial^2 \phi}{\partial x^2} + \frac{\partial^2 \phi}{\partial y^2} + \frac{\partial^2 \phi}{\partial z^2} &= 0 \\
\frac{\partial^2 \theta}{\partial x^2} + \frac{\partial^2 \theta}{\partial y^2} + \frac{\partial^2 \theta}{\partial z^2} &= \frac{\cos \theta}{r^2 \sin \theta} \\
\frac{\partial \phi}{\partial x} \frac{\partial r}{\partial x} + \frac{\partial \phi}{\partial y} \frac{\partial r}{\partial y} + \frac{\partial \phi}{\partial z} \frac{\partial r}{\partial z} &= 0 \\
\frac{\partial \theta}{\partial x} \frac{\partial r}{\partial x} + \frac{\partial \theta}{\partial y} \frac{\partial r}{\partial y} + \frac{\partial \theta}{\partial z} \frac{\partial r}{\partial z} &= 0 \\
\frac{\partial \theta}{\partial x} \frac{\partial \phi}{\partial x} + \frac{\partial \theta}{\partial y} \frac{\partial \phi}{\partial y} + \frac{\partial \theta}{\partial z} \frac{\partial \phi}{\partial z} &= 0
\end{aligned}
$$
Therefore we get:
$$
\nabla^2 = \frac{\partial^2}{\partial r^2} + \frac{2}{r} \frac{\partial}{\partial r} + \frac{1}{r^2 \sin^2 \theta} \frac{\partial^2}{\partial \phi^2} + \frac{1}{r^2} \frac{\partial^2}{\partial \theta^2} + \frac{\cos \theta}{r^2 \sin \theta} \frac{\partial}{\partial \theta}
$$
