# A Theory of Fermat Paths for Non-Line-of-Sight Shape Reconstruction

## 0 Abstract

我们提出了一种关于光在已知可见场景与未知非直接视线上的物体之间的费马路径的新理论。这些光路径要么服从镜面反射，要么被物体的边界反射，因此编码了隐藏对象的形状。我们证明了费马路径对应于瞬态测量中的不连续性。然后，我们推导出一种新的约束，将这些不连续点处的路径长度的空间导数与表面法线联系起来。
We present a novel theory of Fermat paths of light between a known visible scene and an unknown object not in the line of sight of a transient camera. These light paths either obey specular reflection or are reflected by the object’s boundary, and hence encode the shape of the hidden object. We prove that Fermat paths correspond to discontinuities in the transient measurements. We then derive a novel constraint that relates the spatial derivatives of the path lengths at these discontinuities to the surface normal.

## 1 Introduction

这些传感器不仅记录入射光子的数量（强度），还记录它们的到达时间，时间分辨率在毫微秒到飞秒范围内。这种测量称为瞬态，而这种方法称为瞬态非视域成像。
These sensors record not only the number of incident photons (intensity) but also their arrival times, at a range of temporal resolutions (millito femto-seconds). Such measurements are called transients and the approach is called transient NLOS imaging.

（1）它们依赖辐射度信息，而现有的单光子雪崩二极管（SPADs）由于诸如堆积效应、后脉冲效应以及对光子噪声和环境光极其敏感等因素，导致强度估计较差；（2）为了简化逆问题，所有现有的重建技术都基于对非直接视线物体的兰伯特反射假设。
(1) they rely on radiometric information and existing SPADs produce poor intensity estimates due to effects such as pile-up and after-pulsing, as well as due to extreme sensitivity to photon noise and ambient lighting; and (2) to simplify the inverse problem, all existing reconstruction techniques rely on an assumption of Lambertian reflectance for the NLOS object.

在本文中，我们仅使用几何约束而非强度约束的技术来克服上述限制，这些约束来自 NLOS 场景的瞬态测量。
In this paper, we overcome the above limitations by developing techniques that use only geometric, rather than intensity, constraints derived from transient measurements of an NLOS scene.

根据费马原理，我们观察到这些路径要么遵循镜面反射定律，要么在物体边界的特定点处反射。然后，我们证明费马路径对应于瞬时测量中的不连续性。不连续性的时间位置仅取决于 NLOS 物体的形状，而不受其反射率（BRDF）的影响。我们还展示了围绕不连续性的瞬时形状与隐藏表面的曲率之间的关系。
Based on Fermat's principle, we observe that these paths follow either the law of specular reflection or reflect at specific points at the object's boundary. We then prove that Fermat paths correspond to discontinuities in the transient measurements. The temporal locations of the discontinuities are a function of only the shape of the NLOS object and not its reflectance (BRDF). We additionally show that the shape of the transient around the discontinuity is related to the curvature of the hidden surface.

我们展示了费马路径长度的空间导数提供了一个简单的约束，可以唯一地确定隐藏场景点的深度和法线。通过将平滑的路径长度函数拟合到更稀疏的测量集上，可以数值估计这个导数。然后，我们应用最后的细化步骤，通过结合深度和法线信息来计算出一个平滑的网格。虽然大多数以前的方法重建了 NLOS 物体的反照率体积，但我们的方法是为数不多的重建了其表面的方法之一。
We show that the spatial derivative of the Fermat pathlength provides a simple constraint that uniquely determines both the depth and normal of a hidden scene point. This derivative is estimated numerically by fitting a smooth pathlength function to a sparser set of measurements. We then apply a final refinement step that computes a smooth mesh by combining both the depth and normal information. While most previous approaches reconstruct an albedo volume of the NLOS object, our approach is one of the few that reconstruct its surface.

我们的理论对所使用的特定瞬态成像技术是无关的。我们通过使用飞秒和皮秒时间尺度的实验验证了我们的理论，并使用脉冲激光和 SPAD（单光子光电二极管）进行了前者的实验，使用干涉测量进行了后者的实验。因此，我们首次能够计算具有从纯粹漫反射到纯粹镜面反射的 BRDF（双向反射分布函数）的曲面对象的毫米和微米尺度的 NLOS 表面。此外，我们的理论适用于反射性 NLOS（向角落看去）和透射性 NLOS（透过扩散器看去）的情况。
Our theory is agnostic to the specific transient imaging technology used. We validate our theory and demonstrate results at both pico-second and femto-second temporal scales, using a pulsed laser and SPAD for the former and interferometry for the latter. Hence, for the first time, we are able to compute millimeter-scale and micrometer-scale NLOS shapes of curved objects with BRDFs ranging from purely diffuse to purely specular. In addition, our theory applies to both reflective NLOS (looking around the corner) and transmissive NLOS (seeing through a diffuser) scenarios.

## 2 Fermat Paths in NLOS Transients

#### Problem Setup

|                    Symbols                    |                    Descriptions                    |
| :-------------------------------------------: | :------------------------------------------------: |
|          $\mathbf{s}\in\mathbb{R}^3$          |                      光源位置                      |
|          $\mathbf{d}\in\mathbb{R}^3$          |                     探测器位置                     |
| $\mathbf{v}\in\mathcal{V}\subset\mathbb{R}^3$ |   可见场景，是光源和探测器的视线内的表面的并集。   |
| $\mathbf{x}\in\mathcal{X}\subset\mathbb{R}^3$ |                     NLOS 场景                      |
|               $I(t;\mathbf{v})$               |      瞬态，相当于在 $t$ 时间内的光子辐照度。       |
|                      $f$                      | 透过率函数吸收了平方反比衰减、阴影、反射和可见性。 |

我们只对那些可以通过单次反射或透过可见场景的单次传输间接地从光源间接接收光线，并以类似的方式间接地向探测器发射光线的表面感兴趣。
We are only interested in such surfaces that can indirectly receive light from the light source by means of a single reflection or transmission through the visible scene, and can indirectly send light to the detector in a likewise manner.

我们假设光源和探测器正在照亮和成像同一个可见点 $\mathbf{v}\in\mathcal{V}$，这个点可以是可见场景中的任意点。这对应于共焦扫描方案。光子路径是：
We assume that the light source and detector are illuminating and imaging the same visible point $\mathbf{v}\in\mathcal{V}$, which can be any point in the visible scene. This corresponds to the confocal scanning scheme. 
$$
\mathbf{s}\to\mathbf{v}\to\mathbf{x}\to\mathbf{v}\to\mathbf{d}
$$
这种三反射假设在 NLOS 成像应用中很常见，原因有两个：首先，NLOS 瞬态成像系统通常具有时间门控机制，可用于去除仅与可见场景交互的直接光子。其次，与 NLOS 场景 $\mathcal{X}$ 有一次以上相互作用的光子具有很低的信噪比，在实际中很难检测到。
This three-bounce assumption is commonplace in NLOS imaging applications, for two reasons: First, NLOS transient imaging systems typically have time-gating mechanisms that can be used to remove direct photons that only interact with the visible scene. Second, photons with more than one interactions with the NLOS scene $\mathcal{X}$ have greatly reduced signal-to-noise ratio, and in practice are difficult to detect.

我们假设已经校准了从光源到可见点的距离和从可见点到探测器的距离 $\tau_{\mathcal V}(\mathbf{v})\triangleq\|\mathbf{s}-\mathbf{v}\|+\|\mathbf{d}-\mathbf{v}\|$。
We assume that we have calibrated the distance $\tau_{\mathcal V}(\mathbf{v})\triangleq\|\mathbf{s}-\mathbf{v}\|+\|\mathbf{d}-\mathbf{v}\|$ from the source to the visible point, and from there to the detector.

然后，我们可以使用在 $\mathcal{X}$ 中传播的路径长度 $\tau\triangleq ct-\tau_{\mathcal{V}}(\mathbf{v})$，其中 $c$ 是光的速度，将瞬态信号重新参数化为 $I(\tau;\mathbf{v})$。
Then, we can use the pathlength traveled in $\mathcal{X}$, $\tau\triangleq ct-\tau_{\mathcal{V}}(\mathbf{v})$ where $c$ is the speed of light, to uniquely reparameterize transients as $I(t;\mathbf{v})$.
$$
I(\tau;\mathbf{v})=\int_{\mathcal{X}}f(\mathbf{x};\mathbf{v})\delta(\tau-\tau(\mathbf{x};\mathbf{v}))\mathrm{d}A(p,q)\tag{1}
$$
其中 $\tau(\mathbf{x};\mathbf{v})=2\|\mathbf{x}-\mathbf{v}\|,(p,q)\in[0,1]^2$ 是参数化的 NLOS 场景表面。

### 2.1 Fermat paths

我们假设 NLOS 场景 $\mathcal{X}$ 为光滑曲面的并集，而 $\partial{\mathcal{X}}\subset\mathcal{X}$ 是在 NLOS 表面上未定义法线的点集。为了简化，我们将 $\partial{\mathcal{X}}$ 称为 $\mathcal{X}$ 的边界，但请注意，除了边界点之外，它还包括构成 $\mathcal{X}$ 的光滑曲面的不连续交点上的点。然后，我们将重点关注以下特定的显著点 $\mathbf{x}\in\mathcal{X}$。
We assume that the NLOS scene $\mathcal{X}$ is formed as the union of smooth surfaces, and $\partial{\mathcal{X}}\subset\mathcal{X}$ is the set of points on the NLOS surface where a surface normal is not defined. We will be referring to $\partial{\mathcal{X}}$ as the boundary of $\mathcal{X}$ for simplicity, but note that, in addition to boundary points, it includes points at discontinuous intersections of the smooth surfaces that make up $\mathcal{X}$. Then, we will be focusing on specific distinguished points $\mathbf{x}\in\mathcal{X}$ as follows.

**定义 1** 对于任何可见点 $v$：

- 镜面反射集合 $\mathcal S(v)\in\mathcal X$ 包含了所有 NLOS 场景的非边界点
- 































