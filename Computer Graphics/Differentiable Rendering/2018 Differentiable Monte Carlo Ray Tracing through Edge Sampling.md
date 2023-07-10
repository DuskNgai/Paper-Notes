# Differentiable Monte Carlo Ray Tracing through Edge Sampling

## 0 Abstract

在渲染中，需要对摄像机参数、光源、场景几何或材料外观等变量进行梯度处理。然而，计算渲染的梯度是具有挑战性的，因为渲染积分包括不可微的可见度项。
In rendering, the gradient is required with respect to variables such as camera parameters, light sources, scene geometry, or material appearance. However, computing the gradient of rendering is challenging because the rendering integral includes visibility terms that are not differentiable.

我们方法的关键是一种新颖的边缘采样算法，它直接对不连续积分的导数所引入的狄拉克 $\delta$ 函数进行采样。我们还开发了基于空间层次结构的高效重要性采样方法。
The key to our method is a novel edge sampling algorithm that directly samples the Dirac $\delta$ functions introduced by the derivatives of the discontinuous integrand. We also develop efficient importance sampling methods based on spatial hierarchies.

## 1 Introduction

以前在可微分渲染方面的工作主要集中在快速的、近似的解决方案上，使用较简单的渲染模型，只处理一阶的可见性，而忽略了诸如阴影和间接光等二阶效果。
Previous work in differentiable rendering has focused on fast, approximate solutions using simpler rendering models that only handle primary visibility, and ignore secondary effects such as shadows and indirect light.

我们的解决方案是随机的，建立在蒙特卡洛光线追踪的基础上。为此，我们引入了新的技术，除了传统方法中常见的立体角采样外，还对三角形的边缘进行了明确的采样。这需要新的空间加速策略和重要性采样来有效地对边缘进行采样。我们的方法是通用的，可以对任意反弹的光传输的导数进行采样。
Our solution is stochastic and builds on Monte Carlo ray tracing. For this, we introduce new techniques to explicitly sample edges of triangles in addition to the usual solid angle sampling of traditional approaches. This requires new spatial acceleration strategies and importance sampling to efficiently sample edges. Our method is general and can sample derivatives for arbitrary bounces of light transport.

## 2 Related Work

### 2.1 Inverse Graphics

逆向图形技术寻求找到给定的观察图像的场景参数。
Inverse graphics techniques seek to find the scene parameters given observed images.

### 2.2 Derivatives in Rendering

我们使用一种混合方法来计算梯度，这种方法混合了自动微分和手动推导不连续的积分。
We compute the gradients using a hybrid approach that mixes automatic differentiation and manually derived derivatives focusing on the discontinuous integrand.

## 3 Method

我们的任务如下：给定一个连续参数集合 $\Phi$（包括相机姿势、场景几何、材料和照明参数）的三维场景，我们使用路径追踪算法生成一个图像。给定一个从图像中计算出来的标量函数（例如我们想要优化的损失函数），我们的目标是反向传播标量的梯度，相对于所有的场景参数 $\Phi$。
Our task is the following: given a 3D scene with a continuous parameter set $\Phi$ (including camera pose, scene geometry, material and lighting parameters), we generate an image using the path tracing algorithm. Given a scalar function computed from the image (e.g. a loss function we want to optimize), our goal is to backpropagate the gradient of the scalar with respect to all scene parameters $\Phi$.

我们计算梯度积分的策略是将其分成平滑和不连续的区域。对于积分的平滑部分（如 Phong 着色或双线性纹理重建），我们采用传统的区域采样，并进行自动微分。对于不连续的部分，我们使用一种新的边缘采样方法来捕捉边界的变化。
Our strategy for computing the gradient integral is to split it into smooth and discontinuous regions. For the smooth part of the integrand (e.g. Phong shading or bilinear texture reconstruction) we employ traditional area sampling with automatic differentiation. For the discontinuous part we use a novel edge sampling method to capture the changes at boundaries.

我们专注于三角形网格，我们假设网格已经被预处理过，因此没有相互渗透。我们还假设没有点光源，也没有完美的镜面。我们用面光源和粗糙度很低的 BRDFs 来近似这些。我们还专注于静态场景，并将运动模糊的时间维度上的整合作为未来的工作。
We focus on triangle meshes and we assume the meshes have been preprocessed such that there is no interpenetration. We also assume no point light sources and no perfectly specular surfaces. We approximate these with area light sources and BRDFs with very low roughness. We also focus on static scenes and leave integration over the time dimension for motion blur as future work.

### 3.1 Primary Visibility

|    Symbol     |                Description                |
| :-----------: | :---------------------------------------: |
|      $k$      |               pixel filter                |
|      $L$      | radiance, integration of scene parameter  |
|     $\pi$     |              scene parameter              |
|      $f$      |         $f(x,y;\pi)=k(x,y)L(x,y)$         |
|      $I$      |                   color                   |
| $\mathcal{H}$ |          Heaviside step function          |
|     $f_u$     |             upper half-space              |
|     $f_l$     |             lower half-space              |
|   $\alpha$    | the edge equation formed by the triangles |

$$
I=\iint k(x,y)L(x,y)\mathrm{d}x\mathrm{d}y\tag{1}
$$

$$
\partial_{\pi}I=\partial_{\pi}\iint f(x,y;\pi)\mathrm{d}x\mathrm{d}y\tag{2}
$$

我们依靠蒙特卡洛积分来估计像素值$I$。然而，我们不能采取简单的方法，应用同样的蒙特卡洛采样器来估计梯度 $\nabla I$，因为场景函数 $f$ 对于场景参数来说不一定可微。
We rely on Monte Carlo integration to estimate the pixel value $I$. However, we cannot take the naive approach of applying the same Monte Carlo sampler to estimate the gradient $\nabla I$, since the scene function $f$ is not necessarily differentiable with respect to the scene parameters.
$$
\mathcal{H}(\alpha(x,y))f_u(x,y)+\mathcal{H}(-\alpha(x,y))f_l(x,y\tag{3})
$$
对于一条边的两个端点 $(a_x,a_y),(b_x,b_y)$，其方程为
$$
\alpha(x,y)=(a_x-b_x)x+(a_y-b_y)y+(a_xb_y-b_xa_y)\tag{4}
$$
对于一点 $(x,y)$，如果 $\alpha(x,y)>0$，则该点在上半空间中，反之在下半空间中。

### 3.2 Secondary Visibility

## 4 Importance Sampling the Edges

### 4.1 Hierarchical Edge Sampling

### 4.2 Importance Sampling A Single Edge

## 5 Results