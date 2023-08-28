# 3D Gaussian Splatting for Real-Time Radiance Field Rendering

## 0 Abstract

首先，从摄像机校准过程中产生的稀疏点开始，我们用三维高斯来表示场景，这种三维高斯保留了用于场景优化的连续三维辐射场的理想特性，同时避免了在空白空间进行不必要的计算；其次，我们对三维高斯进行交错优化/密度控制，特别是优化各向异性协方差，以实现对场景的精确表示；第三，我们开发了一种快速可见性感知渲染算法，该算法支持各向异性拼接，既能加速训练，又能实现实时渲染。
First, starting from sparse points produced during camera calibration, we represent the scene with 3D Gaussians that preserve desirable properties of continuous volumetric radiance fields for scene optimization while avoiding unnecessary computation in empty space; Second, we perform interleaved optimization/density control of the 3D Gaussians, notably optimizing anisotropic covariance to achieve an accurate representation of the scene; Third, we develop a fast visibility-aware rendering algorithm that supports anisotropic splatting and both accelerates training and allows real-time rendering. 

## 1 Introduction

- 引入各向异性三维高斯作为辐射场的高质量、非结构化表示。
  The introduction of anisotropic 3D Gaussians as a high-quality, unstructured representation of radiance fields.
- 三维高斯属性的优化方法，与自适应密度控制交织在一起，为捕捉到的场景创建高质量的表现形式。
  An optimization method of 3D Gaussian properties, interleaved with adaptive density control that creates high-quality representations for captured scenes.
- 针对 GPU 的快速可微分渲染方法，该方法具有可视性感知功能，允许各向异性拼接和快速反向传播，以实现高质量的新颖视图合成。
  A fast, differentiable rendering approach for the GPU, which is visibility-aware, allows anisotropic splatting and fast back-propagation to achieve high-quality novel view synthesis.

## 2 Related Work

### 2.1 Traditional Scene Reconstruction and Rendering

### 2.2 Neural Rendering and Radiance Fields

### 2.3 Point-Based Rendering and Radiance Fields

最简单的形式是，点采样渲染将一组固定大小的非结构化点光栅化，为此它可以利用图形应用程序接口本地支持的点类型或 GPU 上的并行软件光栅化。点采样渲染虽然忠实于底层数据，但却存在漏洞，会造成混叠，而且严格来说是不连续的。基于点的高质量渲染的开创性工作通过“拼接”范围大于像素的点基元（如圆形或椭圆形圆盘、椭圆体或曲面）来解决这些问题。
In its simplest form, point sample rendering rasterizes an unstructured set of points with a fixed size, for which it may exploit natively supported point types of graphics APIs or parallel software rasterization on the GPU. While true to the underlying data, point sample rendering suffers from holes, causes aliasing, and is strictly discontinuous. Seminal work on high-quality point-based rendering addresses these issues by "splatting" point primitives with an extent larger than a pixel, e.g., circular or elliptic discs, ellipsoids, or surfels.

使用神经特征对点进行了增强，并使用 CNN 进行了渲染，从而实现了快速甚至实时的视角合成；但是，它们仍然依赖 MVS 来获得初始几何图形，因此继承了 MVS 的缺陷，最明显的是在无特征/有光泽区域或薄结构等困难情况下重建过度或不足。
Points have been augmented with neural features and rendered using a CNN resulting in fast or even real-time view synthesis; however they still depend on MVS for the initial geometry, and as such inherit its artifacts, most notably over- or under-reconstruction in hard cases such as featureless/shiny areas or thin structures.

基于点的 $\alpha$ 混合和 NeRF 风格的体积渲染在本质上共享相同的成像模型。然而，渲染算法却截然不同。NeRF 是一种隐式地表示空/占空间的连续表示法；要找到公式 2 中的样本，需要进行昂贵的随机取样，因此会产生噪声和计算费用。相比之下，点是一种非结构化、离散的表示方法，具有足够的灵活性，可以创建、破坏和移动与 NeRF 类似的几何图形。正如之前的工作所显示的那样，这可以通过优化不透明度和位置来实现，同时避免了全体积表示法的缺点。
Point-based $\alpha$-blending and NeRF-style volumetric rendering share essentially the same image formation model. However, the rendering algorithm is very different. NeRFs are a continuous representation implicitly representing empty/occupied space; expensive random sampling is required to find the samples in Eq. 2 with consequent noise and computational expense. In contrast, points are an unstructured, discrete representation that is flexible enough to allow creation, destruction, and displacement of geometry similar to NeRF. This is achieved by optimizing opacity and positions, as shown by previous work, while avoiding the shortcomings of a full volumetric representation.

我们的光栅化尊重可见性顺序，这与他们不依赖顺序的方法截然不同。此外，我们还在像素中的所有斑块上反向传播梯度，并对各向异性的斑点进行光栅化。
Our rasterization respects visibility order in contrast to their order-independent method. In addition, we back-propagate gradients on all splats in a pixel and rasterize anisotropic splats.

## 3 Overview

我们的方法的输入是一个静态场景的图像集，以及由 SfM 校准的相应相机，这产生了一个稀疏点云作为副作用。从这些点中，我们创建了**一组 3D 高斯函数，由位置（均值）、协方差矩阵和透明度 $\alpha$ 定义**，这允许非常灵活的优化方案。这导致了对 3D 场景的合理紧凑表示，部分原因是可以使用高度各向异性的体积斑点来紧凑地表示细微结构。辐射场的定向外观组件（颜色）通过球谐函数（SH）表示。我们的算法通过对 3D 高斯参数的一系列优化步骤来创建辐射场表示，即位置、协方差、$\alpha$ 和 SH 系数交错进行，并进行自适应控制高斯密度的操作。我们方法效率之秘在于我们基于瓷砖的光栅器，它允许各向异性斑点的 $\alpha$ 混合，得益于快速排序而尊重可见性顺序。我们快速光栅器还包括通过跟踪累积 $\alpha$ 值进行快速反向传播，并且没有限制可以接收梯度的高斯函数数量。
The input to our method is a set of images of a static scene, together with the corresponding cameras calibrated by SfM which produces a sparse point cloud as a side-effect. From these points we create **a set of 3D Gaussians, defined by a position (mean), covariance matrix and opacity $\alpha$**, that allows a very flexible optimization regime. This results in a reasonably compact representation of the 3D scene, in part because highly anisotropic volumetric splats can be used to represent fine structures compactly. The directional appearance component (color) of the radiance field is represented via spherical harmonics (SH). Our algorithm proceeds to create the radiance field representation via a sequence of optimization steps of 3D Gaussian parameters, i.e., position, covariance, $\alpha$ and SH coefficients interleaved with operations for adaptive control of the Gaussian density. The key to the efficiency of our method is our tile-based rasterizer that allows $\alpha$-blending of anisotropic splats, respecting visibility order thanks to fast sorting. Out fast rasterizer also includes a fast backward pass by tracking accumulated $\alpha$ values, without a limit on the number of Gaussians that can receive gradients.

## 4 Differentiable 3D Gaussian Splatting

## 5 Optimization with Adaptive Density Control of 3D Gaussian

### 5.1 Optimization

### 5.2 Adaptive Control of Gaussian

## 6 Fast Differentiable Rasterizer for Gaussian

## 7 Implementation, Result and Evaluation

### 7.1 Implementation

### 7.2 Result and Evaluation

### 7.3 Ablations

### 7.4 Limitations

