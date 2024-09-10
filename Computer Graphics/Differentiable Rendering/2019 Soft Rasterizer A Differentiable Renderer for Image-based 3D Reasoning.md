# Soft Rasterizer: A Differentiable Renderer for Image-based 3D Reasoning

## Claim

Soft Rasterizer 把渲染视作所有 mesh 上的三角形对于像素贡献的叠加，通过软化光栅化过程，使得整个渲染过程可导。

## Motivation

光栅化不可微的原因是在离散化到像素的过程中，选择了距离相机最近的三角形上的点的属性用来渲染。

## Method

### Modeling

Soft Rasterizer 是在 Screen Space 上起作用的，用来可微化原本是离散的光栅化过程。Soft Rasterizer 定义了一个位于 Screen Space 的三角形对于某个像素的贡献函数 $\mathcal{D}$：
$$
\mathcal{D}(t_{i}, p_{j}; \sigma) = \Phi\left(\delta(t_{j}, p_{i}) \cdot \frac{\text{SDF}^{2}(t_{i}, p_{j})}{\sigma}\right)
$$
其中 $t_{i}$ 是第 $i$ 个三角形，$p_{j}$ 是第 $j$ 个像素，$\sigma$ 是一个软化参数，$\Phi$ 是一个软化函数（可以选取 Sigmoid 或者 Erf），$\delta(t_{j}, p_{i})$ 是一个指示函数，当像素 $p_{i}$ 在三角形 $t_{j}$ 上时为 1，否则为 -1，$\text{SDF}(t_{i}, p_{j})$ 是三角形 $t_{i}$ 到像素 $p_{j}$ 的有符号距离。

通过让 $\sigma$ 趋近于 0，Soft Rasterizer 可以退化为普通的光栅化过程。

### Rendering

每一个像素点可以去找到它对应的，有影响的三角形，然后计算它们的贡献，最后加权叠加得到最终的像素值。
