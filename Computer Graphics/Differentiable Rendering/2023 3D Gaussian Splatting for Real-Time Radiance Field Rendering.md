# 3D Gaussian Splatting for Real-Time Radiance Field Rendering

## 0 Abstract

1. 用摄像机校准过程中产生的稀疏 RGB 点云初始化，用三维高斯来表示场景，因为三维高斯保留了用于场景优化的连续三维辐射场的理想特性，同时避免了在空白空间进行不必要的计算；
2. 对三维高斯进行交错优化/密度控制，特别是优化各向异性协方差，以实现对场景的精确表示；
3. 一种快速可见性感知渲染算法，该算法支持各向异性拼接，既能加速训练，又能实现实时渲染。

## 1 Introduction

三维高斯表达产生最先进的视觉质量和有竞争力的训练时间，基于片区的叠加方法确保在多个数据集上以最先进的质量实现 1080p 分辨率的实时渲染。

- 引入各向异性的三维高斯作为高质量、非结构化的辐射场表达。
- 适用于三维高斯优化方法，与自适应密度控制交织在一起，为拍摄到的场景创建高质量的表达。
- 一种针对 GPU 的快速可微渲染方法，具有可视性感知功能，允许各向异性叠加和快速反向传播，以实现高质量的新视角合成。

## 2 Related Work

### 2.1 Traditional Scene Reconstruction and Rendering

### 2.2 Neural Rendering and Radiance Fields

### 2.3 Point-Based Rendering and Radiance Fields

最简单的基于点的渲染形式是点采样渲染。它将一组固定大小的非结构化点光栅化，为此它可以利用图形 API 原生支持的点类型或 GPU 上的并行软件光栅化。点采样渲染虽然忠实于底层数据，但却存在空洞、混叠问题，而且严格来说是不连续的。基于点的高质量渲染的开创性工作通过“叠加”范围大于像素的点基元（如圆形或椭圆形圆盘、椭圆体或曲面）来解决这些问题。

部分工作使用神经特征对点进行了增强，并使用 CNN 进行了渲染，从而实现了快速甚至实时的视角合成；但是，它们仍然依赖 MVS 来获得初始几何图形，因此继承了 MVS 的缺陷，最明显的是在无特征/有光泽区域或薄结构等困难情况下重建过度或不足。

基于点的 $\alpha$ 混合和 NeRF 的体渲染本质上有着相同的渲染公式。然而，渲染算法却截然不同。NeRF 是一种隐式地表示空/占空间的连续表示法；要找到渲染点，需要进行昂贵的随机采样，因此会产生噪声和计算费用。相比之下，点是一种非结构化、离散的表示方法，具有足够的灵活性，可以创建、破坏和移动与 NeRF 类似的几何图形。可以通过优化不透明度和位置来实现，同时避免了全体积表示法的缺点。

我们的光栅化尊重可见性顺序，这与 Pulsar 不依赖顺序的方法截然不同。此外，我们还在像素中的所有斑块上反向传播梯度，并对各向异性的斑点进行光栅化。

## 3 Overview

输入是一个静态场景的图像集，以及相应由 SfM 校准的相机和稀疏点云。从点云中，创建**一组 3D 高斯函数，由位置（均值）、协方差矩阵和透明度 $\alpha$ 定义**，这种表示允许非常灵活的优化方案。这导致了对 3D 场景的合理紧凑表示，部分原因是可以使用高度各向异性的体积斑点来紧凑地表示细微结构。辐射场的颜色函数通过球谐函数（SH）表示。我们的算法通过对 3D 高斯参数的一系列优化步骤来创建辐射场表达，即位置、协方差、$\alpha$ 和 SH 系数交错进行，并进行自适应控制高斯密度的操作。我们方法效率之秘在于我们基于片区的光栅器，它允许各向异性斑点的 $\alpha$ 混合，得益于快速排序而尊重可见性顺序。我们快速光栅化器还包括通过跟踪累积 $\alpha$ 值进行快速反向传播，并且没有限制可以接收梯度的高斯函数数量。

## 4 Differentiable 3D Gaussian Splatting

| Symbols  |                  Descriptions                   |
| :------: | :---------------------------------------------: |
|  $\mu$   |           3D center point of Gaussian           |
| $\Sigma$ | 3D covariance matrix of Gaussian in world space |
| $\alpha$ |        alpha blending value of Gaussian         |
|   $W$    |                 view transform                  |
|   $J$    |              projection transform               |

我们选择了 3D 高斯函数，它们是可微的，可以轻松投影到 2D 图像，以实现快速的 $\alpha$ 混合渲染。鉴于 SfM 点云的极端稀疏性，很难估计法线。而且优化这种估计得出的非常嘈杂的法向量将非常具有困难，因此不去建模法向量。

$$
G(x)=\alpha\exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)
$$

对于一般的实对称矩阵，当它为半正定矩阵的时候，它才满足作为协方差矩阵的条件。因此对角化这个协方差矩阵得到：
$$
\Sigma=R^T\Lambda R=R^TS^TS R
$$
其中旋转矩阵通常用四元数转换而来，缩放矩阵/对角矩阵就用 3D 向量表示。

---

$\Sigma$ 在 Camera Space 中的表示 $\Sigma'$ 为：
$$
\begin{align*}
x&=W^{-1}J^{-1}x'\\
x^T\Sigma^{-1}x&=x'^TJ^{-T}W^{-T}\Sigma^{-1}W^{-1}J^{-1}x'\\
&=x'^T(JW\Sigma W^{T}J^{T})^{-1}x'\\
&=x'^T\Sigma'^{-1}x'\\
\Sigma'&=JW\Sigma W^{T}J^{T}
\end{align*}
$$

---

## 5 Optimization with Adaptive Density Control of 3D Gaussian

### 5.1 Optimization

Loss:
$$
\mathcal{L}=(1-\lambda)\mathcal{L}_1+\lambda\mathcal{L}_{\text{D-SSIM}}
$$
$\lambda=0.2$

### 5.2 Adaptive Control of Gaussian

- 每 100 次迭代就把 $\alpha$ 低于 $\epsilon_\alpha$ 的 Gaussian 球移除。
- 欠重建（under-reconstruction）：Gaussian 球稀疏区域。
- 过重建（over-reconstruction）：Gaussian 球过大区域。
- 这两种情况，View Space 内关于位置的梯度都很大，优化偏向于将 Gaussian 球移向这个区域。

对于在 View Space 平均梯度值大于阈值的 $\tau_{\text{pos}}=2\times10^{-4}$ 的值 
- 欠重建的解决方法：克隆一份相同的 Gaussian 球，并且移向梯度的方向。
- 过重建的解决方法：将原本的 Gaussian 球复制 2 份，将其 $S$ 值除以 $\phi=1.6$，并且将原来的 Gaussian 球当作 PDF 采样新的球的位置。
但二者都会不断增加 Gaussian 球。因此每 $N=3000$ 将 Gaussian 球的 $\alpha$ 值变成很接近于 0 的值，然后靠优化过程将 $\alpha$ 值变大并且自动去除 $\alpha$ 值小的球。（不重要）

## 6 Fast Differentiable Rasterizer for Gaussian

1. 将屏幕划分为 $16\times16$ 的小片，然后计算这个小片和相机中心产生的视锥体和 Gaussian 的重叠，当置信区间大于 $99\%$ 时候就算重叠。
2. 给 Gaussian 一个键，包含了 View Space 的深度和对应的小片的 ID。
3. 用 GPU Radix Sort 按键来排序 Gaussian。
4. 前向时候加载 Gaussian 到 GPU Shared Memory，从前往后做 $\alpha$ 混合，并且提前停止，记录停止的位置。
5. 反向时候从停止位置开始回传梯度。

## 7 Implementation, Result and Evaluation

### 7.1 Implementation

#### Optimization Details

### 7.2 Result and Evaluation

### 7.3 Ablations

### 7.4 Limitations

# Derivation in Gaussian Splatting

假设 $l=l(\Sigma)$ 是一个标量。

## Original Version

$R$ 是正交矩阵，$S$ 是对角线 ，$M=SR$ 是它们的乘积。$\Sigma=M^TM$ 是协方差矩阵，也是对称矩阵。$l$ 关于 $M$ 的导数为：
$$
\begin{align*}
\frac{\partial{l}}{\partial{m_{ij}}}&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}\frac{\partial{\Sigma_{pq}}}{\partial{m_{ij}}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}\frac{\partial{\sum_km_{kp}m_{kq}}}{\partial{m_{ij}}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}\left[\sum_km_{kq}\delta_{ik}\delta_{jp}+\sum_km_{kp}\delta_{ik}\delta_{jq}\right]\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}[m_{iq}\delta_{jp}+m_{ip}\delta_{jq}]\\
&=\sum_q\frac{\partial{l}}{\partial{\Sigma_{jq}}}m_{iq}+\sum_p\frac{\partial{l}}{\partial{\Sigma_{pj}}}m_{ip}&\text{Symmetry}\\
&=2\sum_p\frac{\partial{l}}{\partial{\Sigma_{pj}}}m_{ip}\\
&=2M_{i\cdot}\left[\frac{\partial{l}}{\partial{\Sigma}}\right]_{j\cdot}^T\\
&=2M_{i\cdot}\left[\frac{\partial{l}}{\partial{\Sigma}}\right]_{\cdot j}&\text{Symmetry}\\
&=2\left[M\frac{\partial{l}}{\partial{\Sigma}}\right]_{ij}
\end{align*}
$$
因此我们可以推测出：
$$
\frac{\partial{l}}{\partial{M}}=2M\frac{\partial{l}}{\partial{\Sigma}}
$$
$l$ 关于 $S$ 的导数为：
$$
\begin{align*}
\frac{\partial{l}}{\partial{s_i}}&=\sum_p\sum_q\frac{\partial{l}}{\partial{m_{pq}}}\frac{\partial{m_{pq}}}{\partial{s_i}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{m_{pq}}}\frac{\partial{s_pr_{pq}}}{\partial{s_i}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{m_{pq}}}r_{pq}\delta_{ip}\\
&=\sum_q\frac{\partial{l}}{\partial{m_{iq}}}r_{iq}\\
&=R_{i\cdot}\frac{\partial{l}}{\partial{M}}_{\cdot i}\\
&=\left[R\frac{\partial{l}}{\partial{M}}\right]_{ii}
\end{align*}
$$
因此我们可以推测出：
$$
\frac{\partial{l}}{\partial{s_i}}=R_{i\cdot}\frac{\partial{l}}{\partial{M}}_{\cdot i}
$$
$l$ 关于 $R$ 的导数为：
$$
\begin{align*}
\frac{\partial{l}}{\partial{r_{ij}}}&=\sum_p\sum_q\frac{\partial{l}}{\partial{m_{pq}}}\frac{\partial{m_{pq}}}{\partial{r_{ij}}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{m_{pq}}}\frac{\partial{s_pr_{pq}}}{\partial{r_{ij}}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{m_{pq}}}s_{p}\delta_{ip}\delta_{jq}\\
&=\frac{\partial{l}}{\partial{m_{ij}}}s_{i}\\
&=\frac{\partial{l}}{\partial{M}}_{ji}s_{i}
\end{align*}
$$
因此我们可以推测出：
$$
\frac{\partial{l}}{\partial{R}}=S\frac{\partial{l}}{\partial{M}}
$$

## Modified Version

$$
\Sigma=R^TSR=\begin{bmatrix}
r_{11}&r_{21}&r_{31}\\
r_{12}&r_{22}&r_{32}\\
r_{13}&r_{23}&r_{33}\\
\end{bmatrix}
\begin{bmatrix}
s_{1}&0&0\\
0&s_{2}&0\\
0&0&s_{3}\\
\end{bmatrix}
\begin{bmatrix}
r_{11}&r_{12}&r_{13}\\
r_{21}&r_{22}&r_{23}\\
r_{31}&r_{32}&r_{33}\\
\end{bmatrix}
$$

$$
\begin{align*}
\frac{\partial{l}}{\partial{s_i}}&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}\frac{\partial{\Sigma_{pq}}}{\partial{s_i}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}\frac{\partial{\sum_kr_{pk}s_kr_{kq}}}{\partial{s_i}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}r_{pi}r_{iq}\\
\end{align*}
$$

$$
\begin{align*}
\frac{\partial{l}}{\partial{r_{ij}}}&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}\frac{\partial{\Sigma_{pq}}}{\partial{r_{ij}}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}\frac{\partial{\sum_kr_{pk}s_kr_{kq}}}{\partial{r_{ij}}}\\
&=\sum_p\sum_q\frac{\partial{l}}{\partial{\Sigma_{pq}}}[r_{pi}s_i\delta_{jq}+s_jr_{jq}\delta_{ip}]\\
&=\sum_p\frac{\partial{l}}{\partial{\Sigma_{pj}}}r_{pi}s_i+\sum_q\frac{\partial{l}}{\partial{\Sigma_{iq}}}s_jr_{jq}\\
&=s_i\sum_p\frac{\partial{l}}{\partial{\Sigma_{pj}}}r_{pi}+s_j\sum_q\frac{\partial{l}}{\partial{\Sigma_{iq}}}r_{jq}\\
&=s_i\frac{\partial{l}}{\partial{\Sigma}}_{j\cdot}R_{\cdot i}+s_jR_{j\cdot}\frac{\partial{l}}{\partial{\Sigma}}_{\cdot i}\\
&=s_i\left[\frac{\partial{l}}{\partial{\Sigma}}R\right]_{ji}+s_j\left[R\frac{\partial{l}}{\partial{\Sigma}}\right]_{ji}
\end{align*}
$$
可以明显发现导数的形式变差了。