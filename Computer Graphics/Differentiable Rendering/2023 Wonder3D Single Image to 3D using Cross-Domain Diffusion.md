# Wonder3D: Single Image to 3D using Cross-Domain Diffusion

## 0 Abstract

单视角生成要解决 SDS 带来的生成速度慢和几何不一致问题，目标做到又快又好。因此 Wonder3D 就用跨域 Diffusion 生成多个 2D 视角的法向图和颜色图，确保多视角一致性。随后用其他算法生成高质量 3D 表面。

## 1 Introduction

作者认为
- 从 2D 语言或视觉模型利用 SDS 的方法效率和一致性都不太行
- 直接生成 3D 表达的物体还不是时候，泛化不行
- 现有的直接生成多视角一致的图片的方法还原度不够。

因此作者让 Diffusion 模型生成法向图和颜色图

## 3 Problem Formulaion

### 3.1 Diffusion Models

### 3.2 The Distribution of 3D Assets

作者认为，3D 物体的分布 $p_a(\mathbf{z})$ 可以建模为对应 2D 多视角的法向图和颜色图的联合分布。给定相机参数 $\{\pi_1\dots\pi_K\}$ 和输入图片 $y$，就有
$$
p_a(\mathbf{z})=p_{\text{nc}}\left(n^{1:K},x^{1:K}\mid y\right)\tag{1}
$$
其中 $p_{\text{nc}}$ 是法向图 $n^{1:K}$ 和颜色图 $x^{1:K}$ 的分布。因此给定相机参数学习一个 $f$ 合成多视角的法向图和颜色图：
$$
f\left(y,\pi_{1:K}\right)=\left(n^{1:K},x^{1:K}\right)\tag{2}
$$
随后就是 Diffusion
$$
p\left(n_T^{(1:K)},x_T^{(1:K)}\right)\prod_tp\left(n_{t-1}^{(1:K)},x_{t-1}^{(1:K)}\mid n_t^{(1:K)},x_t^{(1:K)}\right)\tag{3}
$$

## 4 Method

1. 先用 Diffusion 出多视角的法向图和颜色图，用多视角注意力约束一致。
2. 为了利用好预训练的 Stable Diffusion，用领域切换开关来控制当前输入的形式。
