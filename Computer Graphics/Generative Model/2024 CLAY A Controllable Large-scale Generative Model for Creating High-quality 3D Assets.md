# CLAY: A Controllable Large-scale Generative Model for Creating High-quality 3D Assets

## 0 Abstract

CLAY 是一个 3D 几何和材质生成器。几何生成支持文字、图片、体像、包围盒、点云、隐式表达等多种条件控制。材质生成支持生成有漫反射、粗糙度和金属模式的 2K 分辨率纹理。

## 1 Introduction

几何：
- 数据：重拓扑，使得把 3D 表面转化为 Occupancy 场的同时能保留尖锐的边和平整的面；用 GPT 标注物体的几何特征。
- 网络：多分辨率 VAE（3DShape2VecSet） + Latent DiT

材质：
- 数据：Objaverse 上的 PBR 材质
- 网络：多视角材质 Diffusion

用 LoRA 和基于交叉注意力的控制来适应下游任务。

## 2 Related Work

- 用 SDS 的纯 2D 的 Diffusion 是先驱，但完全不能理解几何和视角关系。
- 加入视角关系让 2D Diffusion 生成效果稍微好一点了，但几何还是不太行。
- 大重建模型直接学习从图像到 3D 表达（Triplane、Voxel-base NeRF）的映射，但几何很烂。
- 之前的纯 3D 模型存在数据形式不统一，表达不统一的问题。因此需要用 3D VAE 将各种表达都转化为 SDF 或 Occupancy 场，统一了表达。

## 3 Large-Scale 3D Generative Model

CLAY 具体尝试了将几何和材质分离的道路，并且直接生成 3D 几何。

## 3.1 Representation and Model Architecture

首先拓展了 3DShape2VecSet：
1. 



