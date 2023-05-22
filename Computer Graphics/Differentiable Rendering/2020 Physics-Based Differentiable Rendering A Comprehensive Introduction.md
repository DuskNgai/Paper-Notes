# Physics-Based Differentiable Rendering: A Comprehensive Introduction

用逼真的光传输模拟来渲染场景可以理解为求一个函数 $f(\mathbf{x})$，其输入 $\mathbf{x}$ 编码了物体的形状和材料。由于 $f$ 比较复杂，通常来说不能通过反函数 $f^{-1}$ 从图像 $\mathbf{y}$ 用获得其对应的逆 $\mathbf{x}$。可微渲染器通过对 $f$ 进行微分，并将反演转换为基于梯度的最小化过程来得到这样的逆。
Rendering a scene using a realistic light transport simulation can be interpreted as evaluating a function $f(\mathbf{x})$, whose input $\mathbf{x}$ encodes the shape and materials of objects. Owing to the complexity of $f$, an inverse $\mathbf{x}=f^{-1}(\mathbf{y})$ to retrieve scene parameters from an existing image $\mathbf{y}$ is normally not available. Differentiable renderers pursue such an inverse by differentiating $f$ and casting the inversion into a gradient-based minimization process.

## 1 Introduction

每一个位置都通过光的物理性质耦合到所有其他位置，因此不可能孤立地分析一个物体——我们需要同时了解所有其他物体的属性！
Every position is effectively coupled to all other positions through the physics of light, and it is therefore not possible to analyze one object in isolation--we need to understand the properties of all other objects at the same time!

现有的基于物理的渲染算法通过模拟光在详细的虚拟场景中流动来生成图像。这个过程也可以理解为求一个函数 $f:\mathcal{X}\mapsto\mathcal{Y}$，其高维输入编码了物体的形状和材料，其输出 $\mathbf{y}=f(\mathbf{x})$ 是展示了物体间复杂辐射耦合（阴影、相互反射、焦散等）的渲染图像。
Existing physics-based rendering algorithms generate images by simulating the flow of light through detailed virtual scene descriptions. This process can also be interpreted as evaluating a function $f:\mathcal{X}\mapsto\mathcal{Y}$, whose high-dimensional input encodes the shape and materials of objects, and whose output $\mathbf{y}=f(\mathbf{x})$ is a rendered image that reveals the complex radiative coupling between objects (shadows, inter-reflection, caustics, etc.).

然而，不是直接求得 $f^{-1}$，而是针对一个更普遍的优化问题，需要选择一个目标函数 $z=g(\mathbf{y})$ 来量化一个暂定方案的质量。
However, instead of directly evaluating $f^{-1}$, they target a more general optimization problem that requires the choice of an objective function $z=g(\mathbf{y})$ to quantify the quality of a tentative solution.

通常情况下，由于参数空间的非线性性质及其巨大的维度，确定优化 $g(f(\mathbf{x}))$ 的参数 $\mathbf{x}$ 是不可行的：网格中的每个 texel、voxel 和 vertex 都是一个自由参数，而详细的场景很容易有数以亿计的参数!
Ordinarily, determining parameters $\mathbf{x}$ that optimize $g(f(\mathbf{x}))$ would simply be infeasible due to the nonlinear nature of the parameter space and its enormous dimension: every texel, voxel, and vertex in a mesh is a free parameter, and detailed scenes can easily have hundreds of millions of them!

与“普通”渲染相比，可微渲染的主要区别和优势在于有导数（$\partial{z}/\partial{x}$），它给高维参数空间提供了一个梯度下降方向。使用任何合适的基于梯度的优化技术，可微渲染器就能够根据指定的目标连续地改进参数。
The main distinction and benefit of differentiable rendering compared to "ordinary" rendering is the availability of derivatives ($\partial{z}/\partial{x}$), which provide a direction of steepest descent in the high-dimensional parameter space. Using any suitable gradient-based optimization technique, a differentiable renderer is then able to successively improve the parameters with respect to the specified objective.

可微渲染技术的另一个好处是，它们可以被纳入端到端训练的概率推理和机器学习管道。
Another benefit of differentiable rendering techniques is that they can be incorporated into probabilistic inference and machine learning pipelines that are trained end-to-end.

#### Challenges

**几何体导数涉及一个独特的挑战：在计算阴影和相互反射时，物体的边界会带来麻烦的不连续性，如果不采取预防措施，会导致不正确的梯度。**
**Geometric derivatives involve a unique challenge: boundaries of objects introduce troublesome discontinuities during the computation of shadows and inter-reflections that lead to incorrect gradients if precautions are not taken.**

### 1.1 Scope

真正的逆渲染管道不仅仅涉及梯度计算，还需要仔细的初始化和参数化，以避免收敛到低质量的局部最小值，处理网格的离散拓扑变化，并分配先验参数以解决逆问题的不理想方面。
A real inverse rendering pipeline is more involved than just the gradient computation, requiring careful initialization and parametrization to avoid convergence to low-quality local minima, handling of discrete topology changes of meshes, and assigning priors to resolve ill-posed aspects of inverse problems.

## 2 Motivation and Mathematical Preliminaries

一个简单的可微渲染的例子：两个纯色的，可能会相互遮挡的三角形，一共有 6 个顶点（12 个数）和 2 个颜色（6 个数）。

|                 ymbols                 |    Descriptions     |
| :------------------------------------: | :-----------------: |
|  $\boldsymbol{\pi}\in\mathbb{R}^{18}$  |  scene parameters   |
| $\boldsymbol{\pi}_v\in\mathbb{R}^{12}$ | vertices parameters |
|  $\boldsymbol{\pi}_c\in\mathbb{R}^6$   |  color parameters   |
|         $I(\boldsymbol{\pi})$          |        image        |
|   $\mathcal{L}(I(\boldsymbol{\pi}))$   |        loss         |

我们的目标是计算得到梯度值 $\nabla_{\boldsymbol{\pi}}\mathcal{L}(I(\boldsymbol{\pi}))$ 以便可以用基于梯度的优化方法减小损失。
Our goal to compute the gradient $\nabla_{\boldsymbol{\pi}}\mathcal{L}(I(\boldsymbol{\pi}))$ so that we can minimize the loss using gradient-based optimization.

我们将首先展示渲染通常被建模为一个积分问题。然后我们证明，虽然被积函数是不连续的，但积分实际上是可微的。不幸的是，积分离散化和自动微分的简单组合并不能计算出收敛于极限的正确导数。
We will first show that rendering is usually modeled as an integration problem. Then we show that, while the integrands are discontinuous, the integrals are actually differentiable. Unfortunately, naive combination of integral discretization and automatic differentiation does not compute the correct derivatives that converge in the limit.

### 2.1 Rendering as an Integration Problem

|                         Symbols                         |                   Descriptions                    |
| :-----------------------------------------------------: | :-----------------------------------------------: |
|                         $(x,y)$                         |           **continuous** 2D coordinates           |
| $m(x,y;\boldsymbol{\pi}):\mathbb{R}^2\mapsto\mathbb{R}$ | underlying imaging function maps $(x,y)$ to color |

一种直接的方法是在每个像素的中心评估 $m$。这种方法容易出现混叠，从而导致锯齿状边缘、瞬时闪烁、摩尔纹和破坏精细细节等问题。
A naive approach is to evaluate $m$ at the center of each pixel. This approach is prone to aliasing, which causes issues including jagged edges, temporal flickering, Moire patterns, and breaking up fine details.

从信号处理的角度来看，我们使用离散图像对这个 2D 域进行采样，其中采样率由成像函数确定。由于成像函数 $m$ 是不连续的，它在所有频率上都具有能量并且不受频带限制。因此，只要我们在像素中心进行评估，无论我们选择多大的分辨率，我们都会遇到混叠问题。为了解决混叠问题，我们需要从成像函数 $m$ 中移除高频能量。这是通过将成像函数与低通滤波器进行卷积来完成的。对于每个像素 $I_i$，我们评估以像素中心 $(x_i,y_i)$ 为中心的积分：
From the signal processing perspective, we are sampling this 2D domain with a discrete image, where the sampling rate is determined by the imaging function. Since the imaging function $m$ is discontinuous, it has energy at all frequencies and is not band-limited. Therefore, as long as we are evaluating at the center of the pixels, we will suffer from the aliasing problem no matter how large we select the resolution. To resolve the aliasing issue, we need to remove the high frequencies energy from the imaging function $m$. This is done by convolving the imaging function with a low pass filter. For each pixel $I_i$, we evaluate an integral centered around the pixel center $(x_i,y_i)$:
$$
I_i=\iint k(x,y)m(x_i+x,y_i+y;\boldsymbol{\pi})\mathrm{d}x\mathrm{d}y=\iint f(x,y;\boldsymbol{\pi})\mathrm{d}x\mathrm{d}y\tag{1}
$$
其中 $k$ 是滤波核。
where $k$ is the filtering kernel.

我们可以解析地或者数值地计算这个积分，但是解析的计算一般来说可遇不可求。

大多数渲染器，无论是实时的、离线的、基于物理的、可微的还是不可微的，都需要处理混叠问题。大多数通过在不同的位置评估成像函数，使用数值解法来解决抗混叠积分，这个过程通常称为**离散化**：
Most renderers, whether real-time, offline, physics-based, differentiable or not, need to deal with the aliasing issue. Most of them solve the anti-aliasing integral using numerical solution by evaluating the imaging function at various locations, a process often called **discretization**:
$$
I_i\approx\frac{1}{N}\sum_{j=1}^{N}f(x_j,y_j;\boldsymbol{\pi})\tag{2}
$$
如果离散化收敛于积分，即 $\lim_{N\to\infty}1/N\sum_{j=1}^{N}f(x_j,y_j)=I_i$，我们说离散化是**一致的**。样本 $x_j,y_j$ 的选择不需要是随机的。然而，如果我们使用某种概率分布随机抽样 $x_j,y_j$，如果其期望与积分相同，即 $\mathbb{E}[f(x_j,y_j)]=I_i$，我们说离散化是**无偏的**。
We say a discretization is **consistent** if the discretization converges to the integral, i.e., $\lim_{N\to\infty}1/N\sum_{j=1}^{N}f(x_j,y_j)=I_i$. The choice of the samples $x_j,y_j$ does not need to be stochastic. Nevertheless, if we are randomly sampling $x_j,y_j$ using probabilistic distribution, we say a discretization is **unbiased** if the expectation is the same as the integral, i.e., $\mathbb{E}[f(x_j,y_j)]=I_i$.

大多数渲染问题是关于 1) 我们如何在这些多维积分中对被积函数 $f$ 建模，以及 2) 我们如何评估、近似、预计算或压缩每个像素的积分 $\int f$。相反，可微渲染提出了第三个问题：我们如何去微分这些积分？
Most rendering problems are about 1) How do we model the integrand $f$ inside these multi-dimensional integrals, and 2) How do we evaluate, approximate, precompute, or compress the integral $\int f$ for each pixel. Differentiable rendering instead asks the third question: how do we differentiate these integrals?

### 2.2 Computing Gradients by Differentiating the Integrals

$$
\frac{\partial}{\partial\boldsymbol{\pi}}\mathcal{L}(I(\boldsymbol{\pi}))=\sum_{i}\frac{\partial\mathcal{L}}{\partial{I_i}(\boldsymbol{\pi})}\frac{\partial{I_i}(\boldsymbol{\pi})}{\partial\boldsymbol{\pi}}\tag{3}
$$

$\mathcal{L}$ 可以是任何可微的函数。这里假设
$$
\mathcal{L}(I(\boldsymbol{\pi}))=\|I(\boldsymbol{\pi})-\hat{I}(\boldsymbol{\pi})\|^2\tag{4}
$$
那么
$$
\frac{\partial}{\partial\boldsymbol{\pi}}\mathcal{L}(I(\boldsymbol{\pi}))=\sum_{i}2\left(I_i(\boldsymbol{\pi})-\hat{I}_i(\boldsymbol{\pi})\right)\frac{\partial{I_i}(\boldsymbol{\pi})}{\partial\boldsymbol{\pi}}\tag{3}
$$
**渲染的被积函数 $f$ 是不连续不可微的，但是其积分 $I_i$ 其实是可微的！**
**The integrand of rendering $f$ is discontinuous and not differentiable, but the integral $I_i$ is actually differentiable!**

重要的是，我们没有将渲染作为一个积分问题来使其可微分。相反，渲染首先是一个积分问题。任何实时或离线渲染方法中的所有方法都是渲染积分的不同近似或离散化。
Importantly, we did not make rendering an integration problem to make it differentiable. Instead, rendering is an integration problem in the first place. All the approaches in any rendering methods, real-time or offline, are different approximations or discretizations of the rendering integral.

考虑两个相互遮挡的三角形，考虑 $\pi_v\in\boldsymbol{\pi}_v$ 部分的导数，如果用数值方法求 $\partial{I}_i/\partial\pi_v$，结果永远是 $0$。因为 $f$ 返回的是 $(x,y)$ 所坐落的三角形的颜色。但是，很明显，三角形顶点的位置，对于像素的颜色是存在影响的，因为像素颜色是 $f$ 值在区域内的积分，用数学语言描述就是：
$$
\frac{\partial{I}_i}{\partial\pi_v}=\frac{\partial}{\partial\pi_v}\iint f(x,y;\boldsymbol{\pi})\mathrm{d}x\mathrm{d}y\not\approx\frac{1}{N}\sum_{j=1}^{N}\frac{\partial}{\partial\pi_v}f(x,y;\boldsymbol{\pi})=0\tag{6}
$$
导数测量的是局部变化，均匀离散化检测到不连续点周围局部变化的机会为零。
Derivatives are measuring local changes, and uniform discretization has zero chance of detect the local changes around discontinuities.

考虑 1D 的情况，以下表达式：
$$
\frac{\partial}{\partial{p}}\int_0^1\mathbb{1}[x<p]\mathrm{d}x\tag{7}
$$
解析解是 $1$，数值解是 $0$。

我们对积分求微分的策略是明确地将样本放在不连续处以检测局部变化。
Our strategy to differentiate the integrals is then to explicitly put samples at the discontinuities to detect the local change.

### 2.3 Distributional Derivatives and Dirac Delta

使用分布导数的概念，它定义了甚至不连续函数的导数。在上面的例子中，被积函数关于 $p$ 的分布导数是 Dirac delta $\delta$：
Use the concept of distributional derivatives, which defines the derivative for even discontinuous functions. In the case above, the distributional derivative of the integrand with respect to $p$ is a Dirac delta $\delta$:
$$
\frac{\partial}{\partial{p}}\mathbb{1}[x<p]=\delta(x-p)\tag{8}
$$

$\delta(x-p)$ 不是函数而是泛函。如果 $0\le p\le 1$，我们将 Dirac delta 的积分定义为 $\int_0^1\delta(x-p)\mathrm{d}x=1$，否则为 $0$。
$\delta(x-p)$ is not a function but a distribution. We define the Dirac delta's integral as $\int_0^1\delta(x-p)\mathrm{d}x=1$ if $0\le p\le 1$, otherwise it is $0$.

### 2.4 Leibniz's Rule for Differentiating 1D Integrals

我们的替代策略是在概念上拆分积分，使所有不连续点都位于边界处。
Our alternative strategy is to, conceptually, split the integral such that all the discontinuities are at the boundaries.
$$
\frac{\partial}{\partial{p}}\int_0^1\mathbb{1}[x<p]\mathrm{d}x=\frac{\partial}{\partial{p}}\int_0^p1\mathrm{d}x+\frac{\partial}{\partial{p}}\int_p^10\mathrm{d}x\tag{9}
$$

拆分后，可以通过微积分基本定理计算导数，即
After the splitting, the derivative can then be evaluated through the fundamental theorem of calculus, i.e.,
$$
\frac{\partial}{\partial{p}}\int_0^p1\mathrm{d}x=1,\frac{\partial}{\partial{p}}\int_p^10\mathrm{d}x=0\tag{10}
$$
关于积分边界和被积函数的微分的一般版本是莱布尼茨规则。形式上，令 $\pi\in\mathbb{R}$ 并考虑以下某个区间 $(a(\pi),b(\pi))\subset\mathbb{R}$ 上的黎曼积分：
The general version of differentiation with respect to both the integral boundaries and the integrand is the Leibniz's rule. Formally, let $\pi\in\mathbb{R}$ and consider the following Riemann integral over some interval $(a(\pi),b(\pi))\subset\mathbb{R}$:
$$
\int_{a(\pi)}^{b(\pi)}f(x;\pi)\mathrm{d}x\tag{11}
$$
其中被积函数 $f$ 关于 $x$ 和 $\pi$ 处处可微。然后，莱布尼茨在积分符号下的微分规则表明方程 (11) 的导数关于 $\pi$ 是
where the integrand $f$ is differentiable everywhere with respect to $x$ and $\pi$. Then, Leibniz's rule for differentiation under the integral sign states that the derivative of Eq. (11) with respect to $\pi$ is
$$
\frac{\partial}{\partial{\pi}}\int_{a(\pi)}^{b(\pi)}f(x;\pi)\mathrm{d}x=\underbrace{\int_{a(\pi)}^{b(\pi)}\dot{f}(x;\pi)\mathrm{d}x}_{\text{interior}}+\underbrace{\dot{b}(\pi)f(b(\pi);\pi)-\dot{a}(\pi)f(a(\pi);\pi)}_{\text{boundary}}\tag{12}
$$
其中 $\dot{f}=\partial{f}/\partial{\pi}$，$\dot{a}=\partial{a}/\partial{\pi}$，$\dot{b}=\partial{b}/\partial{\pi}$。

---
设函数 $f(x,u)$ 在 $I=[a,b]\times[\alpha,\beta]$ 上连续且对 $u$ 有*连续的偏微商*，$a(u)$ 和 $b(u)$ 在 $[\alpha,\beta]$ 上*可微*，并且
$$
a\le a(u)\le b, a\le b(u)\le b
$$
则函数
$$
\psi(u)=\int_{a(u)}^{b(u)}f(x,u)\mathrm{d}x
$$
在区间 $[\alpha,\beta]$ 上可微，且有
$$
\frac{\mathrm{d}\psi}{\mathrm{d}u}=\int_{a(u)}^{b(u)}\frac{\partial{f}}{\partial{u}}(x,u)\mathrm{d}x+f(b(u),u)\frac{\mathrm{d}b}{\mathrm{d}u}-f(a(u),u)\frac{\mathrm{d}a}{\mathrm{d}u}
$$
**证明：**

令
$$
F(u,y,z)=\int_{y}^{z}f(x,u)\mathrm{d}x
$$
于是 $\psi(u)$ 是由 $F(u,y,z)$ 与 $y=a(u),z=b(u)$ 复合而成的函数，由链式法则和可微性可得
$$
\begin{align*}
\frac{\mathrm{d}\psi}{\mathrm{d}u}&=\frac{\partial{F}}{\partial{u}}+\frac{\partial{F}}{\partial{y}}\frac{\partial{y}}{\partial{u}}+\frac{\partial{F}}{\partial{z}}\frac{\partial{z}}{\partial{u}}\\
&=\int_{y}^{z}\frac{\partial{f}}{\partial{u}}(x,u)\mathrm{d}x+f(z,u)\frac{\mathrm{d}z}{\mathrm{d}u}-f(y,u)\frac{\mathrm{d}y}{\mathrm{d}u}
\end{align*}
$$
将 $y=a(u),z=b(u)$ 代入上式就得到
$$
\frac{\mathrm{d}\psi}{\mathrm{d}u}=\int_{a(u)}^{b(u)}\frac{\partial{f}}{\partial{u}}(x,u)\mathrm{d}x+f(b(u),u)\frac{\mathrm{d}b}{\mathrm{d}u}-f(a(u),u)\frac{\mathrm{d}a}{\mathrm{d}u}
$$

---

因此，给定一个一维积分 $\int_0^1f(x;\pi)\mathrm{d}x$，和一组代表关于参数 $\pi$ 的不连续点 $p_0,p_1,\dots,p_M$，积分的导数为
Therefore, given an 1D integral $\int_0^1f(x;\pi)\mathrm{d}x$, and a set of points $p_0,p_1,\dots,p_M$ represent the point of discontinuities with respect to the parameter $\pi$, the derivative of the integral is
$$
\begin{align*}
&\frac{\partial}{\partial{\pi}}\int_{0}^{1}f(x;\pi)\mathrm{d}x\\
&=\frac{\partial}{\partial{\pi}}\left[\int_{0}^{p_0}f(x;\pi)\mathrm{d}x+\sum_{k=0}^{M-1}\int_{p_k}^{p_{k+1}}f(x;\pi)\mathrm{d}x+\int_{p_M}^{1}f(x;\pi)\mathrm{d}x\right]\\
&=\frac{\partial}{\partial{\pi}}\int_{0}^{p_0}f(x;\pi)\mathrm{d}x+\sum_{k=0}^{M-1}\frac{\partial}{\partial{\pi}}\int_{p_k}^{p_{k+1}}f(x;\pi)\mathrm{d}x+\frac{\partial}{\partial{\pi}}\int_{p_M}^{1}f(x;\pi)\mathrm{d}x\\
&=\int_{0}^{p_0}\dot{f}(x;\pi)\mathrm{d}x+f^-(p_0;\pi)\\
&+\sum_{k=0}^{M-1}\int_{p_k}^{p_{k+1}}\dot{f}(x;\pi)\mathrm{d}x+\sum_{k=0}^{M-1}\left[f^-(p_{k+1};\pi)-f^+(p_k;\pi)\right]\\
&+\int_{p_M}^{1}\dot{f}(x;\pi)\mathrm{d}x-f^+(p_{M-1};\pi)\\
&=\underbrace{\int_{0}^{1}\dot{f}(x;\pi)\mathrm{d}x}_{\text{interior}}+\underbrace{\sum_{k=0}^{M}\left[f^-(p_k;\pi)-f^+(p_k;\pi)\right]}_{\text{boundary}}
\end{align*}\tag{13}
$$
其中 $f^-(x;\pi)=\lim_{x'\to x^-}f(x';\pi)$ 和 $f^+(x;\pi)=\lim_{x'\to x^+}f(x';\pi)$。

为便于记述，当 $f$ 在 $(x; \pi)$ 处不可微时，我们定义 $\dot{f}(x;\pi)=0$。
For notational convenience, when $f$ is not differentiable at $(x; \pi)$, we define $\dot{f}(x;\pi)=0$.

首先，不连续点的集合 $p_k$ 可以包括连续的点，因为对于连续区域，差值 $f^-(x;\pi)=f^+(x;\pi)$。当我们不知道一个三角形边界是否被遮挡时，这对于以后在二维中处理遮挡问题是很重要的。其次，我们不需要实际分解积分，因为连续区域可以被组装成完整的积分。
First, the set of discontinuity points $p_k$ can include points that turn out to be continuous, since for continuous regions the difference $f^-(x;\pi)=f^+(x;\pi)$. This is important for handling occlusion later in 2D when we do not know if a triangle boundary is occluded or not. Second, we do not need to actually decompose the integral, since the continuous regions can be assembled back to the full integral.

### 2.5 Reynolds Transport Theorem for Differentiating Multi-dimensional Integrals

$f$ 是一个定义在某个 $n$ 维流形 $\Omega(\pi)$ 上的（可能不连续的）标量值函数，参数化为 $\pi\in\mathbb{R}$。此外，$\Gamma(\pi)\subset\Omega(\pi)$ 是由外部边界 $\partial\Omega(\pi)$ 和包含 $f$ 的不连续位置的内部边界联合给出的 $(n - 1)$ 维流形。那么，可以认为
Let $f$ be a (potentially discontinus) scalar-valued function defined on some $n$-dimensional manifold $\Omega(\pi)$ parameterized with some $\pi\in\mathbb{R}$. Additionally, let $\Gamma(\pi)\subset\Omega(\pi)$ be an $(n - 1)$-dimensional manifold given by the union of the external boundary $\partial\Omega(\pi)$ and the internal one containing the discontinuous locations of $f$. Then, it holds that
$$
\frac{\partial}{\partial{\pi}}\left(\int_{\Omega(\pi)}f\mathrm{d}\Omega\right)=\underbrace{\int_{\Omega(\pi)}\dot{f}\mathrm{d}\Omega}_{\text{interior}}+\underbrace{\int_{\Gamma(\pi)}\langle\mathbf{n},\dot{\mathbf{x}}\rangle\Delta{f}\mathrm{d}\Gamma}_{\text{boundary}}\tag{14}
$$
其中 $\dot{\mathbf{x}}=\partial\mathbf{x}/\partial\pi$，是一个 $n$ 维向量，代表相对于参数 $\pi$ 的边界运动，$\mathrm{d}\Omega$ 和 $\mathrm{d}\Gamma$ 是对应的标准测度，$\mathbf{n}$ 是 $\mathbf{x}\in\Gamma(\pi)$ 的法向量，
$$
\Delta{f}(\mathbf{x})=\lim_{\epsilon\to0^-}f(\mathbf{x}+\epsilon\mathbf{n})-\lim_{\epsilon\to0^+}f(\mathbf{x}+\epsilon\mathbf{n})\tag{15}
$$

#### Discretizing the integrals

$$
\begin{gather}
\int_{\Omega(\pi)}\dot{f}\mathrm{d}\Omega\approx\frac{1}{N_i}\sum_{j=1}^{N_i}\dot{f}(x_j)\\
\int_{\Gamma(\pi)}\langle\mathbf{n},\dot{\mathbf{x}}\rangle\Delta{f}\mathrm{d}\Gamma\approx\frac{1}{N_b}\sum_{j=1}^{N_b}\langle\mathbf{n},\dot{\mathbf{x}}\rangle\Delta{f}(x_j)
\end{gather}
$$

对于恒定颜色参数，$\partial{f}/\partial{\pi_c}=1$。对于顶点位置参数，$\partial{f}/\partial{\pi_v}=0$。
For the constant color parameters, $\partial{f}/\partial{\pi_c}=1$. For the vertex positions parameters, $\partial{f}/\partial{\pi_v}=0$.

因此我们定义 $\Gamma$ 为所有三角形边上的点的集合。为了选择边上的点，我们首先随机挑选一条边（2个三角形中有6条边），然后在边上均匀地挑选一个点。为了评估差分 $\Delta{f}$ ，在实践中我们选择一个小的 $\epsilon$，并从两边查询 $f$。此外，我们在同一时间内估计许多像素积分 $I_i$。当我们在边缘上选取一个点时，我们将收集所有滤波支持与我们选取的点相交的像素，并将导数散布到相应的像素积分离散化中。
Therefore we define $\Gamma$ to be the set of points on all the triangle edges. To select a point on the edge, we first randomly pick an edge (there are 6 edges out of 2 triangles), we then uniformly pick a point on the edge. To evaluate the difference $\Delta{f}$ , in practice we select a small $\epsilon$ and query $f$ from the two sides. Furthermore, we are estimating many pixel integrals $I_i$ in the same time. When we pick a point on the edge, we will gather all the pixels whose filter support intersect with the point we pick, and scatter the derivative to the corresponding pixel integral discretization.

### 2.6 Relation to Rasterization-basd Differentiable Renderers

OpenDR 使用像素有限差分对边界项 $\langle\mathbf{n},\dot{\mathbf{x}}\rangle\Delta{f}$ 进行逼近，并根据物体 ID 检测的结果选择中心差分、前向差分和后向差分。
OpenDR approximates the boundary term $\langle\mathbf{n},\dot{\mathbf{x}}\rangle\Delta{f}$ using pixel finite difference, and choose between central difference, forward difference and backward difference depending on the result of the object ID detection.

Kato 等人通过对所有的边缘进行光栅化处理，同时对颜色缓冲区进行插值来计算差分，从而应用边缘光栅化处理。
Kato et al. apply an edge rasterization pass by rasterizing all the edges, while interpolating the color buffer to compute the difference.

- 可见性查询通常不是当前可微渲染系统的最大瓶颈。相反，大部分的渲染时间都花在不连贯的内存访问、不规则散射和导数计算过程中的原子操作上。
  Visibility query is usually not the biggest bottleneck in current differentiable rendering systems. Instead, most of the rendering time is spent on incoherent memory access, irregular scattering, and atomic operations during the derivative computation.
- 内部积分和边界积分不需要使用相同的可见度算法来计算。我们可以使用栅格化来计算内部积分，而使用光线追踪来计算边界积分。
  The interior integral and the boundary integral do not need to be computed using the same visibility algorithm. One can compute the interior integral using rasterization, while computing the boundary integral using ray tracing.
- 许多用于光栅化的加速策略，如用于遮挡剔除的分层 Z 缓冲，也可用于基于光线追踪的边界评估（我们可以使用遮挡剔除技术拒绝边缘）。
  Many acceleration strategies that are used in rasterization, such as the hierarchical Z-buffer for occlusion culling, can also be used for ray tracing based boundary evaluation (we can reject edges using occlusion culling techniques).
- 与像素数相比，边界的集合在图像空间中往往是稀疏的。可以想象，我们可以用比像素数少得多的样本来计算边界积分，同时达到很低的误差。
  The set of the boundary is often sparse in the image space compared to the number of pixels. It is conceivable that we can compute the boundary integral with much less samples compared to the number of pixels while achieving very low error.

## 3 Physics-Based Differentiable Renderering Theory

### 3.1 Differentiable Rendering of Surfaces

$$
L(\mathbf{x},\boldsymbol{\omega}_o)=L_e(\mathbf{x},\boldsymbol{\omega}_o)+\int_{\mathbb{S}^2}L_i(\mathbf{x},\boldsymbol{\omega}_i)f_s(\mathbf{x},\boldsymbol{\omega}_i,\boldsymbol{\omega}_o)\mathrm{d}\sigma(\boldsymbol{\omega}_i)\tag{17}
$$

#### 3.1.1 Direct Illumination

$$
L_r(\mathbf{x},\boldsymbol{\omega}_o)=\int_{\mathbb{S}^2}L_e(\mathbf{y},-\boldsymbol{\omega}_i)f_s(\mathbf{x},\boldsymbol{\omega}_i,\boldsymbol{\omega}_o)\mathrm{d}\sigma(\boldsymbol{\omega}_i)\tag{18}
$$
其中 $\mathbf{y}$ 从 $\mathbf{x}$ 向 $\boldsymbol{\omega}_i$ 发出的光线的最近交点。

考虑计算 $L_r(\mathbf{x},\boldsymbol{\omega}_o)$ 关于某个抽象场景参数 $\pi\in\mathbb{R}$ 的导数问题。
We now consider the problem of calculating the derivative of $L_r(\mathbf{x},\boldsymbol{\omega}_o)$ with respect to some abstract scene parameter $\pi\in\mathbb{R}$.
$$
\frac{\mathrm{d}}{\mathrm{d}\pi}[L_r(\mathbf{x},\boldsymbol{\omega}_o)]=\frac{\mathrm{d}}{\mathrm{d}\pi}\left(\int_{\mathbb{S}^2}L_e(\mathbf{y},-\boldsymbol{\omega}_i)f_s(\mathbf{x},\boldsymbol{\omega}_i,\boldsymbol{\omega}_o)\mathrm{d}\sigma(\boldsymbol{\omega}_i)\right)\tag{19}
$$
定义 $f_{\text{direct}}(\boldsymbol{\omega}_i;\mathbf{x},\boldsymbol{\omega}_o)=L_e(\mathbf{y},-\boldsymbol{\omega}_i)f_s(\mathbf{x},\boldsymbol{\omega}_i,\boldsymbol{\omega}_o)$。由 Reynolds Transport Theorem 可得
$$
\frac{\mathrm{d}}{\mathrm{d}\pi}[L_r(\mathbf{x},\boldsymbol{\omega}_o)]=\underbrace{\int_{\mathbb{S}^2}\frac{\mathrm{d}}{\mathrm{d}\pi}f_{\text{direct}}(\boldsymbol{\omega}_i;\mathbf{x},\boldsymbol{\omega}_o)\mathrm{d}\sigma(\boldsymbol{\omega}_i)}_{\text{interior}}+\underbrace{\int_{\Delta\mathbb{S}^2}\langle\mathbf{n}_\perp,\dot{\boldsymbol{\omega}_i}\rangle\Delta{f}_{\text{direct}}(\boldsymbol{\omega}_i;\mathbf{x},\boldsymbol{\omega}_o)\mathrm{d}\ell(\boldsymbol{\omega}_i)}_{\text{boundary}}\tag{20}
$$
其中 $\mathrm{d}\ell$ 是曲线微元。

内部项是单位球体 $\mathbb{S}^2$ 上的积分，它与场景参数 $\pi$ 无关，使得积分变量 $\boldsymbol{\omega}_i$ 也与 $\pi$ 无关。另一方面，边界项是由于 $f_{\text{direct}}$ 的（跳跃）不连续点（相对于 $\boldsymbol{\omega}_i$ 而言）而出现的，并捕捉到它们如何 "移动"$pi$的变化。这些不连续点形成了一维不连续曲线，我们将其表示为单位球体上的 $\Delta\mathbb{S}^2$。对于任何 $\boldsymbol{\omega}_i\in\mathbb{S}(\mathbf{x},\boldsymbol{\omega}_o)$、$\mathbf{n}_\perp(\boldsymbol{\omega}_i)$ 是 $\mathbb{S}^2$ 的切线空间中垂直于具有单位法线场 $\mathbf{n}_\perp$ 的不连续曲线的一个向量。
The interior term is an integral over the unit sphere $\mathbb{S}^2$, which is independent of the scene parameter $\pi$, making the integral variable $\boldsymbol{\omega}_i$ also be $\pi$-independent. The boundary term, on the other hand, emerges due to the (jump) discontinuity points of $f_{\text{direct}}$ (with respect to $\boldsymbol{\omega}_i$) and captures how they "move" $\pi$ varies. These discontinuity points forms 1D discontinuity curves, which we denote as $\Delta\mathbb{S}^2$ over the unit sphere. For any $\boldsymbol{\omega}_i\in\mathbb{S}(\mathbf{x},\boldsymbol{\omega}_o)$, $\mathbf{n}_\perp(\boldsymbol{\omega}_i)$ is a vector in the tangent space of $\mathbb{S}^2$ at $\boldsymbol{\omega}_i$ perpendicular to the discontinuity curve with unit-normal field $\mathbf{n}_\perp$.
