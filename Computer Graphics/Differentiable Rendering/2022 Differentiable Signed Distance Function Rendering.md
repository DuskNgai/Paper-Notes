# 2022 Differentiable Signed Distance Function Rendering
## 0 Abstraction
形状参数的反演受到了特别关注，但也存在着严峻的挑战：形状与可见性交织在一起，除非采取昂贵的预防措施，否则可见性的不连续性会给计算的导数带来严重的偏差。
The inversion of shape parameters is of particular interest but also poses severe challenges: shapes are intertwined with visibility, whose discontinuous nature introduces severe bias in computed derivatives unless costly precautions are taken.

像三角形网格这样的形状表示法还存在其他困难，因为网格参数的连续优化不能引入拓扑变化。
Shape representations like triangle meshes suffer from additional difficulties, since the continuous optimization of mesh parameters cannot introduce topological changes.

解决这些困难的一个常见办法是使用有符号的距离函数（SDF）来表示形状，并在优化过程中逐步调整其零级集。
One common solution to these difficulties entails representing shapes using signed distance functions (SDFs) and gradually adapting their zero level set during optimization.

我们展示了如何扩展常用的球体追踪算法，以便它额外输出一个重新参数化，提供计算精确形状参数导数的方法。
We show how to extend the commonly used sphere tracing algorithm so that it additionally outputs a reparameterization that provides the means to compute accurate shape parameter derivatives.

## 1 Introduction
基于物理的可微渲染（PBDR）方法越来越受到人们的关注，因为它们能够解决以前难以处理的逆问题，包括逼真的材料外观，阴影和相互反射。
Methods for physically-based diferentiable rendering (PBDR) are of increasing interest due to their ability to solve previously intractable inverse problems involving realistic material appearance, shadowing and interrelection.

这些方法的核心挑战是在遮挡物轮廓处出现的不连续。如果不考虑这些突变的话，当使用 PBDR 方法优化场景几何时，这些突变使得导数严重有偏，这极大地破坏了从图像重建等应用。
A central challenge in these methods are discontinuities that arise at the silhouette of occluders. If not accounted for, these instantaneous changes severely bias derivatives when PBDR methods are used to optimize the scene geometry, which effectively breaks applications such as 3D reconstruction from images.

虽然渲染本质上是可微的，但在从方程到算法的转换中很难保留这一属性。例如，直接将自动微分（AD）应用于渲染算法很难能够产生可用的梯度。需要特殊处理的参数包括三角形网格顶点和影响隐式定义曲面的零级集的变量。
While rendering is intrinsically differentiable, it can be difficult to retain this property in the transition from equation to algorithm. For example, the direct application of automatic differentiation (AD) to a rendering algorithm will normally not produce usable gradients. Parameters that require special handling include triangle mesh vertices and variables that influence the zero level set of implicitly defined surfaces.

第二个重要的问题是场景的 3D 表示。存在许多选项，但并非所有选项都同样适合优化。使用 SDF 作为几何表示的一个关键优势是它们能够轻松地表示优化过程中的拓扑变化。
A second important concern is the 3D representation underlying the scene. Many options exist, but not all are equally amenable to optimization. A key advantage of using SDFs as geometric representation is their ability to easily represent topological changes during the optimization process.

虽然有大量的工作使用 SDF 进行逆渲染，但是现有的技术没有可以直接微分 SDF 的渲染关于一阶，二阶（阴影），或更高阶的效果（全局相互反射）。
While there is a large body of work on using SDFs for inverse rendering, no existing technique can directly differentiate renderings of SDFs with respect to primary, secondary (shadows), or higher-order effects (global interreflections)

虽然最初是为三角形网格开发的，但这些方法与几何表示无关，因此也适用于 SDF。它们暴露了偏差与计算的取舍，并且为了获得高质量的梯度，需要追踪许多昂贵的额外光线。
While originally developed for triangle meshes, these methods are independent of the geometry representation and therefore also apply to SDFs. They expose a bias-versus-computation tradeoff, and to achieve high quality gradients, require tracing many costly additional rays.

我们的方法拓展了用于渲染 SDF 的球体追踪技术，以便在空间中步进时收集少量的额外信息。我们使用这些信息以较低的成本初始化一个重参数化，以解决由渲染器产生的不连续积分的问题，因此剩余的计算可以由标准 AD 技术处理。
Our method augments the sphere tracing technique commonly used for SDF rendering so that it collects a small amount of additional information while stepping through space. We use this information to cheaply instantiate a reparameterization that addresses issues with discontinuous integrals performed by the renderer, so that the remaining calculation can be handled by standard AD techniques.

我们的参数化不需要追踪辅助光线以对周围环境进行采样以搜索遮挡边界，因为等效信息在 SDF 值中可以自然获取。
Our parameterization does not need to trace auxiliary rays to sample the surrounding environment in search of occlusion boundaries, since equivalent information is naturally available in the signed distance value of the SDF representation.

最后，我们演示了使用我们的方法重建基于梯度优化的复杂问题的 SDF，无需特定轮廓损失或复杂先验。
Finally, we demonstrate the use of our method to reconstruct SDFs of complex objects with gradient-based optimization, eschewing adhoc silhouette losses or complex priors.

在本文中，我们不追求最先进的现实世界数据重建；我们的重点主要是对 SDF 进行有效的导数评估。
We do not pursue state-of-the art reconstruction from real-world data in this article; our focus is mainly on efficient derivative evaluation for SDFs.

- 我们提出了一种球体追踪的改版，动态地构造重参数化，从而实现精确的可微分 SDF 渲染。
We propose a modification of sphere tracing that dynamically constructs reparameterizations enabling accurate differentiable SDF rendering.
- 我们演示了使用我们的方法进行表面重建，而无需复杂的先验或轮廓损失。
We demonstrate the use of our method for surface reconstruction without the need for complex priors or silhouette loss.
- 我们给我们的重新参数化和导致的扭曲的积分域提供了一个严格的推导。
We provide a rigorous derivation of our reparameterization and the resulting distortion of the integration domain.
- 我们在重新参数化和在单位球面上积分的散度定理之间建立了精确的联系。
We draw a precise connection between using a reparameterization and applying the divergence theorem for integrals over the unit sphere.

## 2 Related Work
### 2.1 Signed Distance Function

依赖于轮廓损失是有问题的。这种方法很难推广到阴影和高阶光反弹。此外，它们不能在轮廓信息不适用的情况下工作（例如，从内部重建房间）。
Relying on a silhouette loss can be problematic. Such approaches are difficult to generalize to shadows and higher order light bounces. Additionally, they cannot work in cases where silhouette information is not applicable (e.g., reconstructing a room from the inside).

我们的算法避免了网格划分，并利用 SDF 的全局结构来获得高质量的梯度。
Our algorithm avoids meshing and leverages the SDF’s global structure to obtain high quality gradients.

### 2.2 Alternative Scene Representations

非表面表示的缺点是它们很难与基于物理的光传输相协调，例如，考虑相互反射。
A disadvantage of many non-surface-based representations is that they are difficult to reconcile with physically-based light transport, e.g., to account for interrelection.

### 2.3 Differentiable Rasterization

光栅化计算效率高，但难以可微地渲染软阴影或间接光照。
Rasterization is computationally efficient, but difficult to use to differentiably render soft shadows or indirect illumination.

### 2.4 Physically-based Differentiable Rendering

微分这些算法的一个核心挑战是可见性不连续性（例如，由于物体遮挡背景或投射阴影）。在微分下，这些不连续引入了一个导数项，必须沿着物体轮廓进行积分。
One central challenge in differentiating such algorithms are visibility discontinuities (e.g., due to an object occluding the background, or casting a shadow). Under differentiation, these discontinuities introduce a derivative term that has to be integrated over object silhouettes.

对这些边缘进行采样具有挑战性，并且要有效地这样做需要使用加速数据结构。当处理连续的隐式曲面如 SDF 时，问题变得更加困难，其中轮廓不是由离散的线段组成的。另一种方法是通过重新参数化被积函数，将边界积分转换为面积分。
Sampling these edges is challenging and doing so efficiently requires using an acceleration data structure. The problem becomes even more difficult when dealing with continuous implicit surfaces such as SDFs, where the silhouette does not consist of a discrete number of line segments. An alternative approach is to convert the boundary integral to an area integral by reparameterizing the integrand.

## 3 Background

| Symbols |Descriptions|
| :--: | :--: |
|   $j$   | a pixel to compute intensity |
| $\mathcal{P}$ | the space of light path |
|  $\mathbf{x}$ | one light path |
|     $\pi$     | a vector containing the scene parameters (e.g., shape parameters, texture values, etc.) |
|    $f_{j}$    | The image contribution function, which measures the contribution of a light path to pixel $j$.  |
| $N$ | the number of samples |
| $x_k$ | sampled light paths |
| $p$ | the probability density function (PDF) of the used sampling strategy |

$$
I_{j}(\pi)=\int_{\mathcal{P}}f_j(\mathbf{x},\pi)\mathrm{d}\mathbf{x}\tag{1}
$$

### 3.1 Differentiable Rendering
在可微渲染中，我们的目标是微分这个积分的值，以在一组大的场景参数上最小化基于图像的目标函数（例如，使用梯度下降）。
In differentiable rendering, our goal is to differentiate the value of this integral to minimize an image-based objective function over a large set of scene parameters (e.g., using gradient descent).
$$
\partial_{\pi}I_j(\pi)=\partial_{\pi}\int_{\mathcal{P}}f_j(\mathbf{x},\pi)\mathrm{d}\mathbf{x}\tag{2}
$$
如果被积函数不含任何关于 $\pi$ 的不连续，我们使用莱布尼茨规则将导数算子移动到积分内部，并应用蒙特卡罗积分来估计导数:
If the integrand does not contain any discontinuities that depend on $\pi$, we use the Leibniz rule to move the derivative operator inside the integral and apply Monte Carlo integration to estimate derivatives:
$$
\partial_{\pi}I_j(\pi)=\int_{\mathcal{P}}\partial_{\pi}f_j(\mathbf{x},\pi)\mathrm{d}\mathbf{x}\approx\frac{1}{N}\sum_{k=1}^{N}\frac{\partial_{\pi}f_j(\mathbf{x}_{k},\pi)}{p(\mathbf{x}_{k},\pi)}\tag{3}
$$

这里的一个重要的设计决定是蒙特卡洛采样步骤本身，以及由此产生的 PDF，是否对场景参数微分。事实证明，对于大多数实际使用情况，最好是将采样策略和 PDF 从微分中分离出来。这大大简化了有效的导数计算。
One important design decision here is whether the Monte Carlo sampling step itself, and consequently the PDF, are differentiated with respect to the scene parameters or not. It turns out that for most practical use cases, it is preferable to detach the sampling strategy and PDF from the differentiation. This greatly simplifies efficient derivative computation.

由于积分的导数和 PDF 之间的不匹配，分离采样策略可能会导致额外的方差，但考虑到由此带来的渲染过程的简化，通常是更可取的。
Detaching the sampling strategy can result in additional variance due to the mismatch between derivative integrand and PDF, but is usually preferable given the resulting simplification of the rendering process.

先前的估计器假定 $f_{j}(\mathbf{x},\pi)$ 的 $\mathbf{x}$ 不含任何取决于 $\pi$ 的不连续点。当 $\pi$ 控制了场景几何的形状或位置时，通常就不再是这种情况了。这种依赖性影响到 $f_{j}$ 中与可见度相关的不连续点的位置，这需要一个由雷诺传输定理描述的额外导数项。简单地将导数算子移入积分中，就无法考虑这个额外的项，且会产生不正确的输出。不过，雷诺传输定理并不是处理参数相关的不连续性的唯一方法。
The previous estimator assumed that $f_{j}(\mathbf{x},\pi)$ did not contain any discontinuities in $\mathbf{x}$ whose position depends on $\pi$. This generally ceases to be the case when $\pi$ controls the shape or position of scene geometry. Such a dependence influences the position of visibilityrelated discontinuities in $f_{j}$, which requires an additional derivative term described by the Reynolds transport theorem. Simply moving the derivative operator into the integral fails to account for this extra term and produces incorrect output. However, the Reynolds transport theorem is not the only way to handle parameter-dependent discontinuities.

### 3.2 Reparameterizing Discontinuities

一种特别适合于单向路径追踪的替代方法是重参数化积分。这个想法是，这个重参数化后的积分应该是没有参数相关的不连续的。在这一步之后，将导数算子移到积分内，并通过使用标准路径追踪对光路进行采样来估计参数导数是合法的。如果我们正确地考虑到由于重新参数化造成的失真，这甚至可以产生一个无偏的梯度估计器。
An alternative approach that is particularly well-suited for unidirectional path tracing is to reparameterize the integral. The idea is that this reparameterized integrand should then be free of parameter-dependent discontinuities. Following this step, it is legal to move the derivative operator inside the integral and estimate parameter derivatives by sampling light paths using standard path tracing. If we correctly account for the distortion due to the reparameterization, this can even result in an unbiased gradient estimator.
$$
\partial_{\pi}I(\pi)=\partial_{\pi}\int_{\mathcal{S}^2}f(\omega,\pi)\mathrm{d}\omega\tag{4}
$$
这个公式目前忽略了由于相互反射而产生的影响，但这对大多数推导来说是足够的。我们将不处理通过完全镜面相互作用观察到的不连续性这一非常具有挑战性的特殊情况。
This formulation for now ignores effects due to interreflections, but that will be sufficient for most derivations. We will not handle the very challenging special case of discontinuities observed through perfectly specular interactions.

给定这个球面积分，我们现在可以用换元 $\mathcal{T}:\mathcal{S}^2\times\mathcal{S}^2$ 来重新表述这个问题，该变换将单位球体映射到自身。重新参数化的 $\mathcal{T}$ 必须使得产生的积分对于场景参数来说没有不连续性，之后就允许将导数算子移到积分中：
Given this spherical integral, we can now reformulate the problem using a change of variables $\mathcal{T}:\mathcal{S}^2\times\mathcal{S}^2$ that maps the unit sphere onto itself. The reparameterization $\mathcal{T}$ has to be chosen so that the resulting integrand is free of discontinuities with respect to the scene parameters, which then allows moving the derivative operator into the integral:
$$
\begin{align*}
\partial_{\pi}I(\pi)&=\partial_{\pi}\int_{\mathcal{S}^2}f(\omega,\pi)\mathrm{d}\omega\\
&=\partial_{\pi}\int_{\mathcal{S}^2}f(\mathcal{T}(\omega,\pi),\pi)\|\mathrm{D}\mathcal{T}_{\omega,\pi}(\mathbf{s})\times\mathrm{D}\mathcal{T}_{\omega,\pi}(\mathbf{t})\|\mathrm{d}\omega\\
&=\int_{\mathcal{S}^2}\partial_{\pi}[f(\mathcal{T}(\omega,\pi),\pi)\|\mathrm{D}\mathcal{T}_{\omega,\pi}(\mathbf{s})\times\mathrm{D}\mathcal{T}_{\omega,\pi}(\mathbf{t})\|]\mathrm{d}\omega\\
\end{align*}\tag{5}
$$

|               Symbols                |                         Descriptions                         |
| :----------------------------------: | :----------------------------------------------------------: |
|       $\mathbf{s},\mathbf{t}$        |  orthonormal tangent vectors of the unit sphere at $\omega$  |
| $\mathrm{D}\mathcal{T}_{\omega,\pi}$ | the differential of $\mathcal{T}$ with respect to vector $\omega$ |

#### Constructing A Reparameterization

我们需要 $\mathcal{T}$ 满足对于所有位于原积分不连续点集合上的方向，都有 $\partial_{\pi}\mathcal{T}(\omega_b,\pi)=\partial_{\pi}\omega_b$。换句话说，对重新参数化进行微分，应该会产生一个与单位球面上不连续点的运动完全匹配的微分运动。注意 $\partial_{\pi}\mathcal{T}(\omega_b,\pi)$ 和 $\partial_{\pi}\omega_b$ 都是单位球面切空间的向量。
We need $\mathcal{T}$ to satisfy $\partial_{\pi}\mathcal{T}(\omega_b,\pi)=\partial_{\pi}\omega_b$ for all directions $\omega_b$ that lie on the set of discontinuities of the original integrand. In other words, differentiating the reparameterization should result in a differential motion that perfectly matches the motion of the discontinuity on the unit sphere. Note that $\partial_{\pi}\mathcal{T}(\omega_b,\pi)$ and $\partial_{\pi}\omega_b$ are vectors that lie in the tangent space of the unit sphere.

除此之外，我们需要 $\partial_{pi}\mathcal{T}(\omega_b,\pi)$ 本身是连续的，这样重参数化才有效。此外，还需要积分域没有边界（例如，单位球体），或者在接近域边界时积分项归零。
Aside from that, we need $\partial_{\pi}\mathcal{T}(\omega_b,\pi)$ itself to be continuous for the reparameterization to be valid. Further, it is necessary that the integration domain either has no boundaries (e.g., the unit sphere), or the integrand goes to zero as it approaches the domain boundary.

对于三角形网格，可以通过首先构建一个辅助的重参数化来获得合适的重参数化，该重参数化在三角形边界获得了正确的运动。然后将这个重参数化与球面域的滤波核进行卷积，以消除重参数化本身的不连续性。由于这种模糊化只影响到重新参数化，它可以用来构建原始积分的无偏梯度估计器。
For triangle meshes, a suitable reparameterization can be obtained by first constructing an auxiliary reparameterization, which attains the correct motion at triangle boundaries. This reparameterization is then convolved with a filter kernel in the spherical domain, that removes discontinuities from the reparameterization itself. Since this blurring only affects the reparameterization, it can be used to construct an unbiased gradient estimator of the original integral.

#### Distortion of the Integration Domain

如果我们要对三维环境空间进行重新参数化，我们将简单地使用映射的雅各布行列式。然而，在这里，这将是不正确的，因为重新参数化是在流形上进行的。相反，我们需要使用切线向量的叉乘的模长，通过重新参数化的微分进行映射。
If we were to reparameterize the 3D ambient space, we would simply use the Jacobian determinant of the mapping. However, here this would be incorrect as the reparameterization works on a manifold. We instead need to use the norm of the cross product of the tangent vectors, mapped through the differential of the reparameterization.

## 4 Method

### 4.1 Preliminaries

|    Symbols    |                         Descriptions                         |
| :-----------: | :----------------------------------------------------------: |
| $\mathcal{M}$ |                          a surface                           |
|    $\phi$     |                      common defined SDF                      |
| $\mathbf{x}$  |                  a point in $\mathbb{R}^3$                   |
|     $\pi$     | the vector of parameters defining the SDF, attached to the AD graph |
|    $\pi_0$    |                   detached to the AD graph                   |

Eikonal equation
$$
\|\partial_{\mathbf{x}}\phi(\mathbf{x},\pi)\|=1\tag{6}
$$

#### SDF Representation

在本文中，我们将有符号的距离函数存储在一个体素网格上。通过这种表示方法，$\pi$ 代表了存储值的列表。在渲染过程中，网格值使用三次 B-spline 基函数进行插值。足够高阶的插值很重要，因为法线与 SDF 的导数有关。简单的三线插值会导致不连续的阴影，产生一个不理想的面状外观。我们使用基函数解析的导数对梯度和 Hessian 进行插值，并利用 SDF 梯度的连续性来构建一个重新参数化。我们的方法依赖于插值平滑了梯度的任何潜在不连续性（包括在 SDF 的骨架上）。
In this paper, we store signed distance functions on a voxel grid. With this representation, $\pi$ represents the list of stored values. During rendering, the grid values are interpolated using cubic B-spline basis functions. Sufficiently high-order interpolation is important, since normals are related to the derivative of the SDF. Simple trilinear interpolation would result in discontinuous shading producing an undesirable faceted appearance. We interpolate positional gradient and Hessian using analytic derivatives of the basis functions and leverage the continuity of the SDF's positional gradient to construct a reparameterization. Our method relies on the interpolation smoothing out any potential discontinuities in the positional gradient (including on the SDF's skeleton).

#### Ray Intersection

在每次迭代中，步长等于 SDF 的绝对值，即到表面的无符号距离，在当前位置 $\mathbf{x}_i$。这意味着算法会在给定到表面的最小距离的情况下迈出一步，这就确保了我们不会意外地跨过它。最终，光线要么逃到虚空中，要么评估的距离低于指定的收敛阈值 $\epsilon$，然后返回光线的交点位置。
In each iteration, the step size is equal to the absolute value of the SDF, i.e., the unsigned distance to the surface, at the current location $\mathbf{x}_i$. This means the algorithm takes a step given the minimum distance to the surface, which ensures that we do not accidentally step over it. Eventually, the ray will either escape into the void, or the evaluated distance will fall below a specified convergence threshold $\epsilon$, and the ray intersection location is returned.

#### Notation

### 4.2 Shading Gradients

除了处理不连续之外，SDF 的可微渲染还要求能够对表面法线的评估进行微分，这在评估形状的反射率模型时是要用到的。归一化是必要的，因为网格插值的 SDF 表示法不能保证完全满足 Eikonal 约束。
Aside from handling discontinuities, differentiable rendering of SDFs also requires the ability to differentiate the evaluation of the surface normal that is later used when evaluating the shape's reflectance model. The normalization is needed, since the grid-interpolated SDF representation cannot guarantee that the eikonal constraint is perfectly satisfied.
$$
\mathbf{n}(\pi)=\frac{\partial_{\mathbf{x}}\phi(\mathbf{x}_{t(\pi)},\pi)}{\|\partial_{\mathbf{x}}\phi(\mathbf{x}_{t(\pi)},\pi)\|}\tag{7}
$$

|        Symbols        |                         Descriptions                         |
| :-------------------: | :----------------------------------------------------------: |
|       $t(\pi)$        |                  the intersection distance                   |
| $\mathbf{x}_{t(\pi)}$ | $\mathbf{x}_0+t(\pi)\omega$, the intersection location on the surface |
| $\mathbf{x}_0,\omega$ |               ray origin and the ray direction               |

要对 $\mathbf{n}(\pi)$ 进行微分，需要特别注意，因为相交距离 $t$ 取决于 $\pi$，是球体追踪的结果，是一种数值求根过程。使用反函数定理：
To differentiate $\mathbf{n}(\pi)$, special care is required, since the intersection distance $t$ depends on $\pi$ and is the result of sphere tracing, a numerical root finding procedure. Using the inverse function theorem:
$$
\partial_{\pi}t(\pi)=-\frac{\partial_{\pi}\phi(\mathbf{x}_{t(\pi_0)},\pi)}{\langle\partial_{\mathbf{x}}\phi(\mathbf{x}_{t(\pi_0)},\pi_0),\omega\rangle}\\
\partial_{\pi}\mathbf{x}_{t(\pi)}=\partial_{\pi}[\mathbf{x}_0+t(\pi)\omega]=-\frac{\partial_{\pi}\phi(\mathbf{x}_{t(\pi_0)},\pi)}{\langle\partial_{\mathbf{x}}\phi(\mathbf{x}_{t(\pi_0)},\pi_0),\omega\rangle}\omega\\\tag{8}
$$
使用这个表达式，我们可以对表面法线进行微分，并正确说明其对交点距离的依赖性，而不需要在球体追踪迭代中跟踪参数导数。
Using this expression, we can differentiate the surface normal and correctly account for its dependency on the intersection distance, without the need to track parameter derivatives across sphere tracing iterations.

### 4.3 Reparameterizing Discontinuities

首先，我们定义一个向量场，其导数跟随 SDF 表面在三维空间的运动。然后，我们展示了如何沿着三维空间的连续位置评估这个向量场，从而在单位球体上构建一个重新参数化，这可以正确处理 SDF 的闭塞和自我封闭。
First, we define a vector field, whose derivative follows the motion of the SDF surface in 3D. We then show how evaluating this vector field along continuous positions in 3D space enables constructing a reparameterization on the unit sphere, which can correctly handle occlusion and self-occlusion by SDFs.

#### Motion of Implicit Surface

我们的最终目标是在单位球体上定义一个重新参数化。
Our eventual goal is to define a reparameterization on the unit sphere.

定义一个辅助的三维向量场 $\mathcal{V}:\mathbb{R}^3\to\mathbb{R}^3$。它的构造使导数 $\partial_{\pi}\mathcal{V}(\mathbf{x},\pi)\in\mathbb{R}^3$ 在零级集上求值时与相对于 $\pi$ 的无穷小表面运动相匹配。
Define an auxiliary 3D vector field $\mathcal{V}:\mathbb{R}^3\to\mathbb{R}^3$. It is constructed so that the derivative $\partial_{\pi}\mathcal{V}(\mathbf{x},\pi)\in\mathbb{R}^3$ matches the infinitesimal surface motion with respect to $\pi$ when evaluated on the zero level set.
$$
\mathcal{V}(\mathbf{x},\pi)=-\frac{\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0)}{\|\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0)\|^2}\phi(\mathbf{x},\pi)\tag{9}
$$

$$
\begin{align*}
\partial_{\pi}\mathcal{V}(\mathbf{x},\pi)&=-\frac{\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0)}{\|\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0)\|^2}\partial_{\pi}\phi(\mathbf{x},\pi)\\
&=-\frac{\frac{\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0)}{\|\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0)\|}}{\left\langle\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0),\frac{\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0)}{\|\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0)\|}\right\rangle}\partial_{\pi}\phi(\mathbf{x},\pi)\\
&=\frac{-\mathbf{n}}{\langle\partial_{\mathbf{x}}\phi(\mathbf{x},\pi_0),\mathbf{n}\rangle}\partial_{\pi}\phi(\mathbf{x},\pi)\\
&=\partial_{\pi}[\mathbf{x}_0+t(\pi)\mathbf{n}]=\partial_{\pi}\mathbf{x}(\pi)
\end{align*}\tag{10}
$$

这里，只有 SDF $\phi(\mathbf{x},\pi)$ 的值取决于可分参数 $\pi$，而位置梯度及其模长平方是静态的，因此用 $\pi_0$ 来写。
Here, only the value of the SDF $\phi(\mathbf{x},\pi)$ depends on the differentiable parameter $\pi$, while the positional gradient and its squared norm are static and hence written using $\pi_0$.

在这种情况下，光线原点 $\mathbf{x}_0$ 只需要是沿着这条光线的任何一点，使光线与 SDF 垂直相交。请注意，这只是一个证明 $\partial_{\pi}\mathcal{V}(\mathbf{x},\pi)$ 具有正确方向和大小的构造，我们不需要实际计算这样一个光线交点。
In this case, the ray origin $\mathbf{x}_0$ just needs to be any point along this ray such that the ray intersects the SDF perpendicularly. Note that this is just a construction to prove that $\partial_{\pi}\mathcal{V}(\mathbf{x},\pi)$ has the right direction and magnitude, we do not need to actually compute such a ray intersection.

到目前为止，这个推导并没有明确假设函数 $\phi$ 是一个SDF。如果 $\phi$ 确实是一个SDF，那么方程 9 中分母的梯度模长就等于 1。然而，我们发现，在处理近似的（如网格内插）SDF 时，包括梯度模长的平方使我们的方法更加稳健。
So far, this derivation does not explicitly assume the function $\phi$ to be an SDF. If $\phi$ is indeed an SDF, the gradient norm in the denominator in Equation 9 equals 1. However, we found that including the normalization by the squared gradient norm makes our method more robust when working with approximate (e.g., grid-interpolated) SDFs.

#### Reparameterization of the Unit Sphere

我们定义了一个距离评估函数 $t(\omega,\pi_0)$，随着边界的接近，收敛到导致不连续的 SDF 的边缘在三维中的距离。
We define an evaluation distance function $t(\omega,\pi_0)$ that, as a boundary is approached, converges to the distance at which the edge of the SDF causing the discontinuity is located in 3D.

我们将这个距离计算为球体追踪过程中遇到的距离的加权组合，选择的权重是为了使加权和在接近边界时具有正确的收敛特性。
We compute this distance as a weighted combination of the distances that are encountered during sphere tracing, with weights chosen so that the weighted sum will have the right convergence characteristics as it approaches a boundary.

这种距离计算的一个重要特性是它不依赖于可微分参数 $\pi$。这意味着我们不需要在球体追踪循环中计算昂贵的参数导数 $\partial_{\pi}\phi(\mathbf{x},\pi)$。
One important property of this distance computation is that it does not depend on the differentiable parameter $\pi$. This means that we do not need to compute expensive parameter derivatives $\partial_{\pi}\phi(\mathbf{x},\pi)$ within the sphere tracing loop.

现在，我们假设这个距离 $t$ 是给定的，以后我们将精确地定义它。考虑到这个距离，我们首先通过定义一个辅助函数在单位球面上构造一个重参数化：
For now, we assume this distance $t$ to be given and we will define it precisely later. Given the distance, we construct a reparameteriza tion on the unit sphere by first defining an auxiliary function:
$$
\begin{align*}
\tilde{\mathcal{T}}(\omega,\pi)&=[\mathbf{x}_t+\mathcal{V}(\mathbf{x}_t,\pi)-\mathcal{V}(\mathbf{x}_t,\pi_0)]-\mathbf{x}\\
&=t\omega+\mathcal{V}(\mathbf{x}_t,\pi)-\mathcal{V}(\mathbf{x}_t,\pi_0)
\end{align*}\tag{11}
$$
where $\mathbf{x}_t=\mathbf{x}+t\omega$.













