# Fourier Slice Photography

## 0 Abstract

1. 在 Fourier Domain，由全光圈透镜形成的照片是 4D 光场的 2D 切片。
2. 聚焦在不同深度的照片对应于 4D 光场的不同轨迹的切片。

## 1 Introduction

## 3 Background

| Symbols | Description |
|:-------:|:-----------:|
|$\bar{L}(u,v,x,y;F)$|Two-plane light field **inside camera**|
|$(u,v)$|From plane on lens|
|$(x,y)$|To plane on sensor|
|$F$|Distance between lens and sensor|
|$E(x,y;F)$|Irradiance|
|$\phi$|Angle between ray and the normal of $(x,y)$ plane|
|$L(u,v,x,y;F)$|$L(u,v,x,y;F)=\bar{L}(u,v,x,y;F)\cos^{4}\phi$|

### Image Formation of Traditional Camera

$$
E(x,y;F)=\frac{1}{F^{2}}\iint_{\mathbb{R}^{2}}\bar{L}(u,v,x,y;F)\cos^{4}\phi\ \mathrm{d}u\mathrm{d}v\tag{1}
$$

因为 $\phi$ 是 $(u,v,x,y;F)$ 的函数，所以可以被吸收进入 $\bar{L}$ 之中，定义 $L(u,v,x,y;F)=\bar{L}(u,v,x,y;F)\cos^{4}\phi$。

### Image Formation of Plenoptic Camera

全光相机可以采集到从特定方向上穿过传感器的光线辐射度。对于任意的深度 $F'=\alpha F$，用相似三角形可以得到

$$
\begin{align*}
L(u,v,x,y;F')&=L(u,v,x,y;\alpha F)\\
&=L\left(u,v,u+\frac{x-u}{\alpha},v+\frac{y-v}{\alpha};F\right)\\
&=L\left(u,v,u\left(1-\frac{1}{\alpha}\right)+\frac{x}{\alpha},v\left(1-\frac{1}{\alpha}\right)\frac{v}{\alpha};F\right)
\end{align*}\tag{2}
$$

> 这里 $(u,v)$ 空间的度规好像和 $(x,y)$ 相等才可以推导。

因此，在深度为 $F'$ 的辐射度与某个深度为 $F$ 的辐射度相关联起来了。合并上面两个公式得到
$$
\begin{align*}\\
\mathcal{P}[L](x,y;\alpha F)&=E(x,y;\alpha F)\\
&=\frac{1}{\alpha^{2}F^{2}}\iint_{\mathbb{R}^{2}}L\left(u,v,u\left(1-\frac{1}{\alpha}\right)+\frac{x}{\alpha},v\left(1-\frac{1}{\alpha}\right)\frac{v}{\alpha};F\right)\cos^{4}\phi\ \mathrm{d}u\mathrm{d}v
\end{align*}\tag{3}
$$

其中 $\mathcal{P}[L](x,y;\alpha F)$ 这个算子将在深度为 $F$ 采集到的光场转化成了在深度为 $\alpha F$ 的照片。这个算子可以看作是先剪切 4D 空间，然后向下投影到 2D 空间。

## 4 Photographic Imaging in Fourier Space

### 4.1 Generalization of Fourier Slicing Theorem

- $f:\mathbb{R}^{n}\mapsto\mathbb{R}$ 函数表示一个 volume。
- **Projection**: $\mathcal{I}^{n}_{m}:(\mathbb{R}^{n}\mapsto\mathbb{R})\mapsto(\mathbb{R}^{m}\mapsto\mathbb{R})$ 算子表示积分掉输入函数的最后 $n-m$ 维，即
  $$
  \mathcal{I}^{n}_{m}[f](x_{1},\dots,x_{m})=\int_{\mathbb{R}^{n-m}}f(x_{1},\dots,x_{n})\mathrm{d}x_{m+1}\dots\mathrm{d}_{n}
  $$
- **Slicing**: $\mathcal{S}^{n}_{m}:(\mathbb{R}^{n}\mapsto\mathbb{R})\mapsto(\mathbb{R}^{m}\mapsto\mathbb{R})$ 算子表示直接去掉输入函数的最后 $n-m$ 维，即
	$$
	\mathcal{S}^{n}_{m}[f](x_{1},\dots,x_{m})=f(x_{1},\dots,x_{m},0,\dots,0)
	$$
- **Change of Basis**: $\mathcal{B}:\mathbb{R}^{n}\mapsto\mathbb{R}^{n}$ 算子表示改变 $n$ 维空间的基（一般是旋转，也可以是剪切），即
	$$
	\mathcal{B}[f](\mathbf{x})=f(\mathcal{B}^{-1}\mathbf{x})
	$$
- **Fourier Transform**: $\mathcal{F}^{n}: (\mathbb{R}^{n}\mapsto\mathbb{R})\mapsto(\mathbb{R}^{n}\mapsto\mathbb{R})$ 算子表示 $n$ 维函数的 Fourier 变换。

广义 Fourier 切片定理
$$
\mathcal{F}^{m}\circ\mathcal{I}^{n}_{m}\circ\mathcal{B}\equiv\mathcal{S}^{n}_{m}\circ\frac{\mathcal{B}^{-T}}{|\mathcal{B}^{-T}|}\circ\mathcal{F}^{n}\tag{4}
$$
即，先换基、再投影、最后 Fourier 变换等价于先 Fourier 变换、再换基、最后切片。证明在 Appendix A。

### 4.2 Fourier Slice Photography

![classical FST](images/classical-fst.png)

![generalized-FST](images/generalized-fst.png)

![photography-FST](images/photography-fst.png)

在光场成像中，公式 $(3)$ 可以写作
$$
\begin{align*}\\
\mathcal{P}[L](x,y;\alpha F)&=\frac{1}{\alpha^{2}F^{2}}\iint_{\mathbb{R}^{2}}L\left(u,v,u\left(1-\frac{1}{\alpha}\right)+\frac{x}{\alpha},v\left(1-\frac{1}{\alpha}\right)\frac{v}{\alpha};F\right)\cos^{4}\phi\ \mathrm{d}u\mathrm{d}v\\
&=\frac{1}{\alpha^{2}F^{2}}\mathcal{I}^{4}_{2}\circ\mathcal{A}(\alpha)[L](x,y;F)
\end{align*}\tag{5}
$$
其中换基矩阵就是
$$
\begin{align*}
\mathcal{A}(\alpha)&=\begin{bmatrix}1&0&0&0\\0&1&0&0\\1-\alpha&0&\alpha&0\\0&1-\alpha&0&\alpha\end{bmatrix}&\mathcal{A}^{-1}(\alpha)&=\begin{bmatrix}1&0&0&0\\0&1&0&0\\1-\frac{1}{\alpha}&0&\frac{1}{\alpha}&0\\0&1-\frac{1}{\alpha}&0&\frac{1}{\alpha}\end{bmatrix}
\end{align*}
$$
将 Fourier Slice Theorem 作用上去得到
$$
\begin{align*}\\
\mathcal{P}[L](x,y;\alpha F)&=\frac{1}{\alpha^{2}F^{2}}\mathcal{I}^{4}_{2}\circ\mathcal{A}(\alpha)[L](x,y;F)\\
&=\frac{1}{\alpha^{2}F^{2}}\mathcal{F}^{-2}\circ\mathcal{S}^{4}_{2}\circ\frac{\mathcal{A}^{-T}(\alpha)}{|\mathcal{A}^{-T}(\alpha)|}\circ\mathcal{F}^{4}[L](x,y;F)\\
&=\frac{1}{F^{2}}\mathcal{F}^{-2}\circ\mathcal{S}^{4}_{2}\circ\mathcal{A}^{-T}(\alpha)\circ\mathcal{F}^{4}[L](x,y;F)
\end{align*}\tag{6}
$$





---

### Appendix A Generalization of the Fourier Slice Theorem

我们首先注意到
$$
\mathcal{F}^{m}\circ\mathcal{I}^{n}_{m}\equiv\mathcal{S}^{n}_{m}\circ\mathcal{F}^{n}
$$
这是因为
$$
\begin{align*}
&\quad\ \ \mathcal{F}^{m}\circ\mathcal{I}^{n}_{m}[f]\\\\
&=\int_{\mathbb{R}^{m}}\left[\int_{\mathbb{R}^{n-m}}f(x_{1},\dots,x_{n})\mathrm{d}x_{m+1}\dots\mathrm{d}_{n}\right]\exp(-i2\pi(x_{1}u_{1}+\dots+x_{m}u_{m}))\mathrm{d}x_{1}\dots\mathrm{d}_{m}\\
&=\int_{\mathbb{R}^{n}}f(x_{1},\dots,x_{n})\exp(-i2\pi(x_{1}u_{1}+\dots+x_{m}u_{m}))\mathrm{d}x_{1}\dots\mathrm{d}_{n}\\
&=\int_{\mathbb{R}^{n}}f(x_{1},\dots,x_{n})\exp(-i2\pi(x_{1}u_{1}+\dots+x_{m}u_{m}+x_{m+1}0+\dots+x_{n}0))\mathrm{d}x_{1}\dots\mathrm{d}_{n}\\
&=\mathcal{F}^{n}[f](u_1,\dots,u_m,0,\dots,0)\\
&=\mathcal{S}^{n}_{m}\circ\mathcal{F}^{n}[f]
\end{align*}
$$
接下来需要考虑换基的事情。我们有 Fourier 变换下的基变换
$$
\mathcal{F}^{n}\circ\mathcal{B}\equiv\frac{\mathcal{B}^{-T}}{|\mathcal{B}^{-1}|}\circ\mathcal{F}^{n}
$$
这是因为
$$
\begin{align*}
&\quad\ \ \mathcal{F}^n\circ\mathcal{B}[f]\\
&=\int_{\mathbb{R}^{n}}f\left(\mathcal{B}^{-1}\mathbf{y}\right)\exp\left(-i2\pi\mathbf{y}^T\mathbf{u}\right)\mathrm{d}\mathbf{y}&&\mathbf{x}=\mathcal{B}^{-1}\mathbf{y}\\
&=\frac{1}{|\mathcal{B}^{-1}|}\int f(\mathbf{x})\exp\left(-i2\pi(\mathcal{B}\mathbf{x})^T\mathbf{u}\right)\mathrm{d}\mathbf{x}&&\mathrm{d}\mathbf{y}=|\mathcal{B}|\mathrm{d}\mathbf{x}=\frac{1}{|\mathcal{B}^{-1}|}\mathrm{d}\mathbf{x}\\
&=\frac{1}{|\mathcal{B}^{-1}|}\int f(\mathbf{x})\exp\left(-i2\pi\mathbf{x}^T\left(\mathcal{B}^T\mathbf{u}\right)\right)\mathrm{d}\mathbf{x}\\
&=\frac{\mathcal{B}^{-T}}{|\mathcal{B}^{-1}|}\circ\mathcal{F}^{n}
\end{align*}
$$
相关的例子有 1D Fourier 变换的缩放性、2D Fourier 变换的旋转性、缩放性。最后结合这两个式子得到
$$
\begin{align*}
\mathcal{F}^{m}\circ\mathcal{I}^{n}_{m}\circ\mathcal{B}&\equiv\mathcal{S}^{n}_{m}\circ\mathcal{F}^{n}\circ\mathcal{B}\\
&\equiv\mathcal{S}^{n}_{m}\circ\frac{\mathcal{B}^{-T}}{|\mathcal{B}^{-1}|}\circ\mathcal{F}^{n}\\
&\equiv\mathcal{S}^{n}_{m}\circ\frac{\mathcal{B}^{-T}}{|\mathcal{B}^{-T}|}\circ\mathcal{F}^{n}
\end{align*}
$$
得证。
