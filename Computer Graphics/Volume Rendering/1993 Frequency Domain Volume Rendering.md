# Frequency Domain Volume Rendering

## 0 Abstract

傅里叶切片定理允许在 $O(n^2\log n)$ 的时间内生成大小为 $n^3$ 的体像数据的投影。不幸的是，投影并没有表现出体渲染特有的遮挡感知。
The Fourier projection-slice theorem allows projections of volume data to be generated in $O(n^2\log n)$ time for a volume of size $n^3$. Unfortunately, these projections do not exhibit the occlusion that is characteristic of conventional volume renderings.

## 1 Introduction

对于一个三维体像，该定理指出，以下两个是傅里叶变换对。
For a 3D volume, the theorem states that the following two are a Fourier transform pair:

- 通过沿垂直于图像平面的射线对体积进行线积分得到的二维图像。
    The 2D image obtained by taking line integrals of the volume along rays perpendicular to the image plane.
- 从体积的傅里叶变换中沿包括原点并与图像平面平行的平面提取一个切片而得到的二维频谱。
    The 2D spectrum obtained by extracting a slice from the Fourier transform of the volume along a plane which includes the origin and is parallel to the image plane.

尽管有理论上的速度优势，但频域体渲染算法却存在几个众所周知的问题。
Despite their theoretical speed advantage, frequency domain volume rendering algorithms suffer from several well-known problems:

**高插值成本**。由于插值是$O(n^2)$，FFT 仍是渐进式的主导。
**High interpolation cost**: As the interpolation is $O(n^2)$, the FFT is still asymptotically dominant.

**内存成本**。假设采用双精度表示，每个体素需要 16 个字节。相比之下，在空域中，每个体素只需要一个字节。
**Memory cost**: Assuming a double precision representation, 16 bytes are required per voxel. By contrast, only 1 byte per voxel is necessary in spatial domain algorithms.

**缺乏深度信息**。观察射线上的体素对图像的贡献是相同的，无论它们与眼睛的距离如何。因此，图像缺乏一个重要的视觉线索，遮挡感知。
**Lack of depth information**: Voxels on a viewing ray contribute equally to the image regardless of their distance from the eye. The image therefore lacks occlusion, an important visual cue.

在早期的一篇论文中，我们表明，对于有限的一类上色模型，对观察方向和照明方向的依赖性可以从投影积分中剔除：
In an earlier paper, we instead showed that for a limited class of shading models, the dependence on viewing direction and lighting direction could be factored out of the projection integral:
$$
I=\sum_{i=0}^nw_i\biggl(\int_{-\infty}^{+\infty}f_i(\mathbf{x}(t))\mathrm{d}t\biggr)
$$
这里，观察和照明方向的影响仅由权重 $w_i$ 表示，而体像 $f_i$ 与它们无关。
Here, effects of viewing and lighting direction are solely expressed by weights $w_i$ while the volumes $f_i$ are independent of them.

## 2 Base Algorithm

|             Symbols              |            Descriptions            |
| :------------------------------: | :--------------------------------: |
| $f(\mathbf{x})$, $F(\mathbf{s})$ | volume in spatial/frequency domain |
|          $\mathcal{F}$           |         Fourier transform          |

对于每个视图，离散频谱 $F(s)$ 使用滤波器 $H(s)$ 沿提取平面（平行于图像平面并通过原点）进行插值。
For each view, the discrete spectrum $F(s)$ is interpolated along the extraction plane (parallel to the image plane and passing through the origin) using a filter $H(s)$.

## 3 Shape Cueing Techniques

### 3.1 Depth Cueing

深度提示是通过根据体素与观察者的距离加权得到的。
Depth cueing is obtained by weighting voxels according to their distance from the observer.

|           Symbols            |                         Descriptions                         |
| :--------------------------: | :----------------------------------------------------------: |
|       $d(\mathbf{x})$        | the weighting function or depth cueing function for a given eye position |
| $f(\mathbf{x})d(\mathbf{x})$ |                     a depth-cued volume                      |
|      $p_m(\mathbf{x})$       |             compensation for the filter response             |
|        $\mathcal{F}$         |                      Fourier transform                       |

空间域深度提示：
Spatial domain depth cueing:
$$
\begin{align*}
\mathcal{F}\{f(\mathbf{x})d(\mathbf{x})p_m(\mathbf{x})\}*H(\mathbf{s})&=(F(\mathbf{s})*D(\mathbf{s})*P_m(\mathbf{s}))*H(\mathbf{s})\\
&=(F(\mathbf{s})*P_m(\mathbf{s}))*(H(\mathbf{s})*D(\mathbf{s}))\\
&=\mathcal{F}\{f(\mathbf{x})p_m(\mathbf{x})\}*H'(\mathbf{s})
\end{align*}
$$
请注意，上述表达式完全是在频域中操作的，而且只在被提取的片断的平面上进行评估。因此，它是一个二维操作。还要注意的是，由于 $\mathcal{F}\{f(\mathbf{x})p_m(\mathbf{x})\}$ 与眼睛的位置无关，三维正向变换只执行一次。
Note that the above expression operates entirely in the frequency domain, and moreover is evaluated only on the plane of the slice being extracted. Hence, it is a 2D operation. Note also that because $\mathcal{F}\{f(\mathbf{x})p_m(\mathbf{x})\}$ is independent of the eye position, the 3D forward transform is performed only once.

尽管 $H'=H*D$ 必须对每个视图进行计算，但重新计算的成本很小，因为滤波器 $H$ 的支持域很小 $(3^3, 5^3)$，而且 $D(\mathbf{s})$ 通常是一个简单的表达式。
Although $H'=H*D$ must be computed for each view, the cost of recomputation is small because the support of filter $H$ is small $(3^3, 5^3)$ and $D(\mathbf{s})$ is usually a simple expression.

这种频域深度提示方法适用于任何深度提示函数 $d(\mathbf{x})$。事实上，该方法可以被设计为突出体积的中间部分，而减弱前面和后面的部分。
This frequency domain depth cueing method applies to any depth cueing function $d(\mathbf{x})$. Indeed, the method can be designed to highlight the middle portion of the volume while attenuating the front and back portions.

让视角矢量为 $\mathbf{V}$。因此，从体积原点测量的有符号深度由 $(\mathbf{V}\cdot\mathbf{x})$ 给出，而 $d_l(\mathbf{x})$ 可以写成：
Let the view vector be $\mathbf{V}$. The signed depth measured from the origin of the volume is thus given by $(\mathbf{V}\cdot\mathbf{x})$, and $d_l(\mathbf{x})$ can be written as:
$$
d_l(\mathbf{x})=C_{\text{cue}}(\mathbf{V}\cdot\mathbf{x})+C_{\text{avg}}
$$
$C_{\text{cue}}$ 是深度提示的强度，$C_{\text{avg}}$ 是常数。
$$
\mathcal{F}\{d_l(\mathbf{x})\}=D_l(\mathbf{s})=-\frac{C_{\text{cue}}}{i2\pi}(\mathbf{V}\cdot\nabla)+C_{\text{avg}}\delta(\mathbf{s})
$$

$$
H'(\mathbf{s})=H(\mathbf{s})*D_l(\mathbf{s})=-\frac{C_{\text{cue}}}{i2\pi}(\mathbf{V}\cdot\nabla H(\mathbf{s}))+C_{\text{avg}}H(\mathbf{s})
$$













