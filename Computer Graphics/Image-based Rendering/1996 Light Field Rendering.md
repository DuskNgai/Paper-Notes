# 光场渲染 Light Field Rendering

我觉得光场这个名字取的不好，应该叫接收场（Receive Field），对应反射场（Reflectance Field）。

## Abstract

无需深度信息和特征匹配，仅通过组合和重采样已有图像，任意相机位置生成新视角。
Generating new views from arbitrary camera positions without depth information or feature matching, simply by combining and resampling the available images. 

将输入图片看作 4D 函数————光场————的 2D切片。
Interpreting the input images as 2D slices of a 4D function - the light field.

静态场景、无遮挡、固定光照的光流。
The flow of light through unobstructed space in a static scene with fixed illumination.

用渲染好的图像或数字图像来生成光场。
Creates light fields from large arrays of both rendered and digitized images.

一个将光场以 100 : 1 的比例压缩、失真度很小的压缩系统。
A compression system that is able to compress the light fields we have generated by more than a factor of 100 : 1 with very little loss of fidelity.

## Introduction

### 基于图像的渲染 Image-Based Rendering

基于图像的渲染的展示算法需要的计算资源较少。
The display algorithms for image-based rendering require modest computational resources.

交互式地查看场景和场景本身的复杂度无关。
The cost of interactively viewing the scene is independent of scene complexity.

输入图像可以是真实的或虚拟的。
The source of the pre-acquired images can be from a real or virtual environment,

> 除了 Image-Based Rendering，还有 Physically Based Rendering。

### 环境映射 Environment Map

环境映射也可以叫纹理映射。

环境映射记录了各个方向到达一个点的入射光线。
An environment map records the incident light arriving from all directions at a point.

环境映射也可以用于记录从一个点向各个方向的出射光线。
Quickly display any outward looking view of the environment from a fixed location but at a variable orientation.

限制：视角固定。
解决方法 1：插值（需要深度信息，特别处理由于遮挡关系而显现出来的区域）。
Limitations: Viewpoint is fixed.
View interpolation(Require a depth value for each pixel in the environment map/ "fill in the gaps" when previously occluded areas become visible.).

解决方法 2：找到输入图片中对应的两点（等价于获取深度信息，现有算法不够好）。
Find corresponding points in the two input images(equivalent to finding the depth values/ algorithms are fairly fragile).

### 4D 光场 4D Light Field

在无遮挡关系的空间（自由空间）内，光场是 4D 函数。
In regions of space free of occluders (free space), the light field is a 4D function.

**特点**

一、新图像可能由很多不同的输入图像算得，而且不一定和输入图像看起来一样。
First, The new image is generally formed from many different pieces of the original input images, and need not look like any of them.

二、不需要模型信息。
Second, no model information, such as depth values or image correspondences, is needed to extract the image values.

三、生成图像只需要线性采样。
Third, image generation involves only resampling, a simple linear process.

**问题**

一、参数化和采样模式。
First, there is the choice of parameterization and representation of the light field. Related to this is the choice of sampling pattern for the field

二、生成和获得光场。
Second, there is the issue of how to generate or acquire the light field.

三、快速生成新视角。
Third, there is the problem of fast generation of different views.

四、数据量大。
Fourth, the obvious disadvantage of this approach is the large amount of data that may be
required.

## 表示 Representation

**全光函数 Plenoptic Function**

由于光线在不受阻挡的情况下不会变化，因此 4D 光场就可以被解释为有向直线空间上的函数。
The radiance does not change along a line unlessblocked. 4D light fields may be interpreted as functions on the space of oriented lines.

5D 表示过于冗余。
Redundancy of the 5D representation.

一、几何体都是有界的。因此这里的自由空间是物体周围的凸起空间
First, most geometric models are bounded. In this case free space is the region outside the convex hull of the object.

二、移动建筑或者户外模型通常也就是移动自由空间。
Second, if we are moving through an architectural model or an outdoor scene we are usually moving through a region of free space.

**光板 light slab**

用任意两个非共面平面的和直线的交点表示这条直线。
Parameterize lines by their intersections with two planes in arbitrary position.


可以把其中一个平面放在无穷远处。
A nice feature of this representation is that one of the planes may be placed at infinity.

### 对偶空间 line space

类似霍夫（Hough）空间。

一个平面在无穷远处的采样比较好。
Arrangements where one plane is at infinity are better than those with two finite planes.

近处采样率可以比远处稍微低一点。
The observer is likely to stand near the uv plane, then it may be acceptable to sample uv less frequently than st.

## 光场生成 Creation of light fields

### 从渲染图片中生成 From rendered images

正投影可以通过把一个平面放到无穷远处成像做到。
A light slab may be formed from a 2D array of orthographic views. This can be modeled by placing the uv plane at infinity.

先过滤再采样的方式抗混叠。但是因为景深而产生模糊。
The effects of aliasing may be alleviated by filtering before sampling. Introducing blurriness due to depth of field.

### 从数字图像中生成 From digitized images

## 压缩 Compression

## 呈现 Display

从 4D 光场中重采样出 2D 切片，每条光线从中心点发出。
The viewer must resample a 2D slice of lines from the 4D light field; each line represents a ray through the eye point and a pixel center.
