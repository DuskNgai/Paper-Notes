# HashSDF: Accurate Implicit Surfaces with Fast Local Features on Permutohedral Lattices

## 0 Abstract

我们提出了对这两个领域的改进，即用包络格代替体素散列编码，在三维和更高维度上更快地进行优化。此外，我们还提出了一个正则化方案，这对恢复高频几何细节至关重要。
We propose improvements to the two areas by replacing the voxel hash encoding with a permutohedral lattice which optimizes faster in three and higher dimensions. We additionally propose a regularization scheme which is crucial for recovering high frequency geometric detail.

## 1 Introduction

我们在 INGP 的体素散列方法的基础上，提出了包络格散列。在这种结构中，每个单纯形的顶点数量随着维度的增加而线性增加，而不是像超立方体体素那样呈指数增长。
We improve upon the voxel-hashing method of INGP by proposing permutohedral lattice hashing. The number of vertices per simplex in this structure scales linearly with dimensionality instead of exponentially as in the hyper-cubical voxel case.

- 一个基于哈希编码的优化神经隐式面的新框架。
    A novel framework for optimizing neural implicit surfaces based on hash-encoding.
- 将哈希编码扩展到包络格中，使其与输入维度呈线性关系，并允许更快的优化。
    An extension of hash encoding to a permutohedral lattice which scales linearly with the input dimensions and allows for faster optimization.

- 一个正则化方案，允许恢复精确的 SDF 几何，其细节程度为毛孔和皱纹的规模。
    A regularization scheme that allows to recover accurate SDF geometry with level of detail at the scale of pores and wrinkles.

## 2 Related Work

### 2.1 Classical Multi-View Reconstruction

### 2.2 NeRF Models

### 2.3 Accelerating NeRF

### 2.4. Implicit Representation

## 3 Overview

|                            Symbol                            |                      Description                      |
| :----------------------------------------------------------: | :---------------------------------------------------: |
|                     $\{\mathcal{I}_k\}$                      |              a set of images with poses               |
|   $\mathcal{S}=\{\mathbf{x}\in\R^3\mid g(\mathbf{x})=0\}$    | surface $\mathcal{S}$ as the zero-level set of an SDF |
|        $g(\mathrm{enc}(\mathbf{x};\theta_g);\Phi_g)$         |                    the SDF network                    |
| $c(\mathrm{enc}(\mathbf{x};\theta_c),\mathbf{v},\mathbf{n},\chi;\Phi_g)$ |                   the color network                   |

## 4 HashSDF

### 4.1 Volume Rendering

### 4.2 Hash Encoding with Permutohedral Lattice

这种网格的主要优点是，给定维度 $d$，每个单纯形的顶点数量是 $d+1$，它是线性扩展的，而不是超立方体的指数增长 $2^d$。
The main advantage of this lattice is that given dimensionality $d$ the number of vertices per simplex is $d+1$, which scales linearly instead of the exponential growth $2^d$ for hyper-cubical voxels.

给定一个位置 $\mathbf{x}$，可以在 $O(d^2)$ 内得到包含的单纯形。在单纯形内，计算重心坐标，并进行类似 INGP 的 $d$ 线性插值。
Given a position $\mathbf{x}$, the containing simplex can be obtained in $O(d^2)$. Within the simplex, barycentric coordinates are calculated and $d$-linear interpolation is performed similar to INGP.

### 4.3 4D Background Estimation

## 5 HashSDF Training

对于镜面或无纹理的区域，Eikonal 正则化不能提供足够的信息来正确恢复表面。
For specular or untextured areas, the Eikonal regularization doesn’t provide enough information to properly recover the surface.
$$
\mathcal{L}_{\text{rgb}}=\sum_{\mathbf{r}}\|\hat{C}(\mathbf{r})-C(\mathbf{r})\|^2\\
\mathcal{L}_{\text{eik}}=\sum_{\mathbf{x}}(\|\nabla{g}(\mathrm{enc}(\mathbf{x}))\|-1)^2\\
$$

### 5.1 SDF Regularization

为了帮助网络在反射或无纹理区域恢复更平滑的表面，我们在 SDF 上增加了曲率损失。计算完整的 $3\times3$ 的 Hessian 矩阵可能很昂贵，所以我们将曲率近似为法向量的局部偏差。
In order to aid the network in recovering smoother surfaces in reflective or untextured areas, we add a curvature loss on the SDF. Calculating the full $3\times3$ Hessian matrix can be expensive, so we approximate curvature as local deviation of the normal vector.

With this normal we define a tangent vector $\eta$ by cross product with a random unit vector $\tau$ such that $\eta=\mathbf{n}\times\tau$. Given this random vector in the tangent plane, we slightly perturb our sample $\mathbf{x}$ to obtain $\mathbf{x}_{\epsilon}=\mathbf{x}+\epsilon$. We obtain the normal at the new perturbed point as $\mathbf{n}_{\epsilon}=\nabla{g}(\mathrm{enc}(\mathbf{x}_{\epsilon}))$ and define a curvature loss based on the dot product between the normals at the original and perturbed point:
$$
\mathcal{L}_{\text{curve}}=\sum_x(\mathbf{n}\cdot\mathbf{n}_{\epsilon}-1)^2
$$

### 5.2 Color Regularization

我们观察到，网络收敛到一个不理想的状态，即几何图形越来越平滑，而色彩网络则学习对所有的高频细节进行建模，以便将 $\mathcal{L}_{\text{rgb}}$ 赶到零。
We observe that the network converges to an undesirable state where the geometry gets increasingly smoother while the color network learns to model all the high-frequency detail in order to drive the $\mathcal{L}_{\text{rgb}}$ to zero.

我们观察到，由颜色网络学到的所有高频细节都必须存在于 MLP 的权重 $\Phi_c$ 或哈希映射表 $\theta_c$ 中，因为所有其他输入都是平滑的。
We observe that all the high-frequency detail learned by the color network has to be present in the weights of the MLP $\Phi_c$ or the hash-map table $\theta_c$ as all the other inputs are smooth.
$$
\|f(a)-f(b)\|\le k\|a-b\|
$$
我们感兴趣的是，颜色网络是一个平滑的函数（小 $k$），这样，高频的颜色也会反映在高细节的几何形状中。
We are interested in the color network being a smooth function (small $k$) such that high-frequency color is also reflected in high-detailed geometry.

给定一个 MLP 层 $y=\sigma(W_ix+b_i)$ 和该层的可训练的 Lipschitz 约束 $k_i$，他们把权重矩阵 $W_i$ 替换为：
Given an MLP layer $y=\sigma(W_ix+b_i)$ and a trainable Lipschitz bound $k_i$ for the layer, they replace the weight matrix $W_i$ with:
$$
y=\sigma(\hat{W_i}x+b_i)\quad\hat{W}_i=m(W_i,\mathrm{softplus}(k_i))
$$
其中 $\mathrm{softplus}(k_i)=\ln(1+e^{k_i})$，函数 $m()$ 通过重新调整 $W_i$ 的每一行，使行和的绝对值小于或等于 $\mathrm{softplus}(k_i)$，使权重矩阵归一。
where $\mathrm{softplus}(k_i)=\ln(1 + e^{k_i})$ and the function $m()$ normalizes the weight matrix by rescaling each row of $W_i$ such that the absolute value of the row-sum is less than or equal to $\mathrm{softplus}(k_i)$.

最后，由于每层 Lipschitz 常数 $k_i$ 的乘积是整个网络的 Lipschitz 约束，我们可以使用正则化：
Finally, since the product of per-layer Lipschitz constants $k_i$​ is the Lipschitz bound for the whole network, we can regularize it using:
$$
\mathcal{L}_{\text{Lip}}=\prod_l^{i=1}\mathrm{softplus}(k_i)
$$
除了对颜色 MLP 进行正则化处理外，我们还对颜色哈希表 $\theta_c$ 应用 0.01 的权重衰减。
In addition to regularizing the color MLP, we also apply weight decay of 0.01 to the color hash map $\theta_c$.

### 5.3 Training Schedule

我们发现，将其作为一个可学习的参数，会导致网络遗漏薄的物体特征，这是因为场景中的大型物体主导了对 $h$ 的梯度。相反，我们使用手动线性衰减 $1/s$ 超过 30 k 次迭代，我们发现这对我们测试的所有物体都是稳健的。
We found out that considering it as a learnable parameter can lead to the network missing thin object features due to the fact that large objects in the scene dominate the gradient towards $h$. Instead, we use a manual linear decay $1/s$ over 30 k iterations which we found to be robust for all the objects we tested.

为了恢复光滑的表面，我们用前 100 k 次迭代来训练：
In order to recover smooth surfaces, we train the first 100 k iterations using:
$$
\mathcal{L}=\mathcal{L}_{\text{rgb}}+\lambda_1\mathcal{L}_{\text{eik}}+\lambda_2\mathcal{L}_{\text{curve}}
$$
对于之后的 100 k 迭代，我们通过去除曲率损失和增加颜色网络 $\lambda_3\mathcal{L}_{\text{Lip}}$ 的正则化来恢复细节。
For further 100 k iterations, we recover detail by removing the curvature loss and adding the regularization of the color network $\lambda_3\mathcal{L}_{\text{Lip}}$.

此外，我们在优化开始时用球体的 SDF 初始化我们的网络，并在最初的 10 k 次迭代过程中以从粗到细的方式退火哈希图的层次 $L$。
In addition, we initialize our network with the SDF of a sphere at the beginning of the optimization and anneal the levels $L$ of the hash-map in a coarse-to-fine manner over the course of the initial 10 k iterations.

## 6 Acceleration

### 6.1 Occupancy Grid

我们维护两个版本的网格，一个是全精度的，存储每个体素的 SDF 值，另一个只包含一个二进制占用位。网格是按 Morton 顺序排列的，以确保快速遍历。请注意，与 INGP 不同的是，我们的网格中存储的是有符号的距离而不是密度。
We maintain two versions of the grid, one with full precision, storing the SDF value at each voxel, and another containing only a binary occupancy bit. The grids are laid out in Morton order to ensure fast traversal. Note that differently from INGP, we store signed distance and not density in our grid.

### 6.2 Sphere Tracing

我们在沿射线的第一个被标记为占用的体素上创建一个射线样本。我们对球体追踪进行预定数量的迭代，并将尚未收敛的样本向表面推进（以其 SDF 高于某个阈值为标志）。一旦所有的样本都收敛了，或者我们达到了球体追踪的最大数量，我们就对颜色网络进行一次采样并渲染。
We create a ray sample at the first voxel along the ray that is marked as occupied. We run sphere tracing for a predefined number of iterations and march not-yet-converged samples towards the surface (indicated by their SDF being above a certain threshold). Once all samples have converged or we reached a maximum number of sphere traces, we sample the color network once and render.

### 6.3 Implementation

Encoding: $\partial\mathbf{y}/\partial\theta$.

Normal: $\partial\mathbf{y}/\partial\mathbf{x}$.

Eikonal loss: $\partial^2\mathcal{L}/\partial\mathbf{x}\partial\theta$ and $\partial(\partial\mathcal{L}/\partial\mathbf{x})/\partial(\partial\mathcal{L}/\partial\mathbf{y})$.

---

### $n$ 维超立方体的体积

$n$ 维超立方体是空间中 $\R^n$ 中这样的点集：
$$
S_n(a)=\{(x_1,\dots,x_n)\mid0\le x_i\le a,i\in\{1,\dots,n\}\}
$$
因此 $n$ 维单位超立方体的体积：
$$
\mu(S_n(1))=1\quad\mu(S_n(a))=a^n
$$

### $n$ 维单形的体积

$n$ 维单形是空间中 $\R^n$ 中这样的点集：
$$
S_n(a)=\{(x_1,\dots,x_n)\mid x_i\ge0,i\in\{1,\dots,n\},\sum_{i}x_i\le a\}
$$
可以作换元：
$$
x_i=at_i,i\in\{1,\dots,n\},\biggl|\frac{\partial(x_1,\dots,x_n)}{\partial(t_1,\dots,t_n)}\biggr|=a^n
$$
即：
$$
\mu(S_n(a))=a^n\mu(S_n(1))
$$
注意到，对于固定的 $t\in[0,1]$，$S_n(1)$ 与 $t_n=t$ 的截面仍然是一个单形，由满足 $t_1+\dots+t_{n-1}\le1-t$ 的点组成，所以是一个 $S_{n-1}(1-t)$ 的单形。

由此可得体积的递推公式：
$$
\begin{align*}
\mu(S_n(1))&=\int_0^1\mu(S_{n-1}(1-t))\mathrm{d}t\\
&=\int_0^1(1-t)^{n-1}\mu(S_{n-1}(1))\mathrm{d}t\\
&=\mu(S_{n-1}(1))\int_0^1(1-t)^{n-1}\mathrm{d}t\\
&=\frac{1}{n}\mu(S_{n-1}(1))
\end{align*}
$$
由于 $\mu(S_1(1))=1$，因此 $n$ 维单位单形的体积是：
$$
\mu(S_n(1))=\frac{1}{n!}\quad\mu(S_n(a))=\frac{a^n}{n!}
$$

### $n$ 维单纯形的体积

$n$ 维单纯形是空间中 $\R^n$​ 中这样的点集：

设 $a=\{a_0,\dots,a_n\},a_i\in\R^n$ 是一组点，且 $\{a_1-a_0,\dots,a_n-a_0\}$ 线性无关，
$$
S_n(a)=\{x\mid x=\sum_{i}a_ix_i,\sum_{i}x_i=1\}
$$
单纯形可以由前述的单形变换而来，满足：
$$
\mu(S_n(a))=\frac{1}{n!}\det(a_1-a_0,\dots,a_n-a_0)
$$
可转换为：
$$
\mu(S_n(a))^2=\frac{(-1)^{n+1}}{2^n(n!)^2}\begin{vmatrix}
0&1&1&1&1&1\\
1&0&d_{01}^2&d_{02}^2&\cdots&d_{0n}^2\\
1&d_{10}^2&0&d_{12}^2&\cdots&d_{1n}^2\\
1&d_{20}^2&d_{21}^2&0&\cdots&d_{2n}^2\\
\vdots&\vdots&\vdots&\vdots&\ddots&\vdots\\
1&d_{n0}^2&d_{n1}^2&d_{n2}^2&\cdots&0
\end{vmatrix}
$$
其中 $d_{ij}=\|a_i-a_j\|$，该行列式为 Cayley-Menger 行列式。

### $n$ 维球体的体积

$n$ 维以原点为球心，以 $a$ 为半径的球体是空间中 $\R^n$ 中这样的点集：
$$
S_n(a)=\{(x_1,\dots,x_n)\mid \sum_{i}x_i^2\le a^2\}
$$
注意到，对于固定的 $u,v,(u^2+v^2\le1)$，$S_n(1)$ 与这个面的截面仍然是一个球体，由满足 $t_1^2+\dots+t_{n-1}^2\le1-u^2-v^2$ 的点组成，所以是一个 $S_{n-2}(\sqrt{1-u^2-v^2})$ 的单形。

由此可得体积的递推公式：
$$
\begin{align*}
\mu(S_n(1))&=\iint_{u^2+v^2\le1}\mu(S_{n-2}(\sqrt{1-u^2-v^2}))\mathrm{d}u\mathrm{d}v\\
&=\iint_{u^2+v^2\le1}(\sqrt{1-u^2-v^2})^{n-2}\mu(S_{n-2}(1))\mathrm{d}u\mathrm{d}v\\
&=\mu(S_{n-2}(1))\iint_{u^2+v^2\le1}(\sqrt{1-u^2-v^2})^{n-2}\mathrm{d}u\mathrm{d}v\\
&=\mu(S_{n-2}(1))\iint_{u^2+v^2\le1}(1-u^2-v^2)^{(n-2)/2}\mathrm{d}u\mathrm{d}v\\
\end{align*}
$$
利用极坐标换元：
$$
\begin{align*}
\iint_{u^2+v^2\le1}(1-u^2-v^2)^{(n-2)/2}\mathrm{d}u\mathrm{d}v&=\int_0^{2\pi}\int_0^1(1-r^2)^{(n-2)/2}r\mathrm{dr\mathrm{d\theta}}\\
&=\frac{2\pi}{n}
\end{align*}
$$
因此：
$$
\mu(S_n(1))=\frac{2\pi}{n}\mu(S_{n-2}(1))
$$
由于 $\mu(S_1(1))=2,\mu(S_2(1))=\pi$，因此 $n$ 维单位单形的体积是：
$$
\mu(S_{2n}(1))=\frac{\pi^n}{n!}\quad\mu(S_{2n-1}(1))=\frac{2^n\pi^{n-1}}{(2n-1)!}\\
\mu(S_{2n}(a))=\frac{\pi^n}{n!}a^{2n}\quad\mu(S_{2n-1}(a))=\frac{2^n\pi^{n-1}}{(2n-1)!}a^{2n-1}
$$
