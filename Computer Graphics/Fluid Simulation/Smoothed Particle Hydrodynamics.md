# Smoothed Particle Hydrodynamics

## SPH Basics

### Discrete Approximations to a Continuous Field

设 $f: \mathbb{R}^{3} \to \mathbb{R}^{n}$ 是一个连续函数，借助 $\delta$ 函数可以将其表示为积分形式：
$$
f(\mathbf{x}) = \int_{\mathbb{R}^{3}} f(\mathbf{x}') \, \delta(\mathbf{x} - \mathbf{x}')\, \mathrm{d}\mathbf{x}' \tag{1}
$$

$\delta$ 函数可以看作一个权重函数，它只在 $\mathbf{x}' = \mathbf{x}$ 处非零，其余地方均为零。为了便于数值计算，我们用一个光滑的核函数 $W(\mathbf{x} - \mathbf{x}', h)$ 来代替 $\delta$ 函数，其中 $h$ 为核宽度，且满足：
$$
\lim_{h \to 0} W(\mathbf{x}, h) = \delta(\mathbf{x}), \quad \int_{\mathbb{R}^{3}} W(\mathbf{x}, h) \, \mathrm{d}\mathbf{x} = 1 \tag{2}
$$

用 $W(\mathbf{x}, h)$ 替换 $\delta$ 函数后，便可得到连续场的积分近似：
$$
f(\mathbf{x}) \approx \int_{\mathbb{R}^{3}} f(\mathbf{x}') \, W(\mathbf{x} - \mathbf{x}', h) \, \mathrm{d}\mathbf{x}' \tag{4}
$$

进一步，为了将积分离散化为粒子求和，我们先将积分改写成密度加权的形式：
$$
f(\mathbf{x}) \approx \int_{\mathbb{R}^{3}} \frac{f(\mathbf{x}')}{\rho(\mathbf{x}')} \, W(\mathbf{x} - \mathbf{x}', h) \, \rho(\mathbf{x}') \mathrm{d}\mathbf{x}' \tag{5}
$$
其中 $\rho(\mathbf{x}')$ 为空间各点的密度。现在对上述积分进行数值近似：将空间划分为一系列小体积元 $\Delta V_{i}$，每个体积元对应一个粒子，其携带的质量为：
$$
m(\mathbf{x}_{i}) = \rho(\mathbf{x}_{i}) \Delta V_{i},
$$
并用位置 $\mathbf{x}_{i}$ 处的粒子被积函数值代表整个体积元的贡献。这样，整个连续场 $f(\mathbf{x})$ 可以由有限个粒子位置上的值 $f_{i}$ 通过核函数加权重建，空间任意点 $\mathbf{x}$ 的场值都由这些离散粒子贡献。：
$$
f(\mathbf{x}) \approx \sum_{i} \frac{m_{i}}{\rho_{i}} \, f_{i} \, W(\mathbf{x} - \mathbf{x}_{i}, h) \tag{6}
$$
其中 $f_{i} = f(\mathbf{x}_{i})$，$\rho_{i} = \rho(\mathbf{x}_{i})$。

### Spatial Derivatives

这里默认积分与微分可以交换。

**梯度**：
$$
\begin{aligned}
\nabla f(\mathbf{x}) &\approx \nabla \int_{\mathbb{R}^{3}} \frac{f(\mathbf{x}')}{\rho(\mathbf{x}')} \, W(\mathbf{x} - \mathbf{x}', h) \, \rho(\mathbf{x}') \mathrm{d}\mathbf{x}' \\
&= \int_{\mathbb{R}^{3}} \frac{f(\mathbf{x}')}{\rho(\mathbf{x}')} \, \biggl[\nabla W(\mathbf{x} - \mathbf{x}', h)\biggr] \, \rho(\mathbf{x}') \mathrm{d}\mathbf{x}' \\
&\approx \sum_{i} \frac{m_{i}}{\rho_{i}} f_{i} \nabla W(\mathbf{x} - \mathbf{x}_{i}, h)
\end{aligned} \tag{10}
$$

**散度**：
$$
\begin{aligned}
\nabla \cdot \mathbf{f}(\mathbf{x}) &\approx \nabla \cdot \int_{\mathbb{R}^{3}} \frac{\mathbf{f}(\mathbf{x}')}{\rho(\mathbf{x}')} \, W(\mathbf{x} - \mathbf{x}', h) \, \rho(\mathbf{x}') \mathrm{d}\mathbf{x}' \\
&\approx \sum_{i} \frac{m_{i}}{\rho_{i}} \mathbf{f}_{i} \cdot \nabla W(\mathbf{x} - \mathbf{x}_{i}, h)
\end{aligned} \tag{13}
$$
这是由于：$\nabla \cdot (\alpha \mathbf{v}) = \alpha \nabla \cdot \mathbf{v} + \mathbf{v} \cdot \nabla \alpha$，其中 $\alpha$ 是一个标量函数，$\mathbf{v}$ 是一个向量函数。

**旋度**：（不常用）
$$
\begin{aligned}
\nabla \times \mathbf{f}(\mathbf{x}) &\approx \nabla \times \int_{\mathbb{R}^{3}} \frac{\mathbf{f}(\mathbf{x}')}{\rho(\mathbf{x}')} \, W(\mathbf{x} - \mathbf{x}', h) \, \rho(\mathbf{x}') \mathrm{d}\mathbf{x}' \\
&\approx -\sum_{i} \frac{m_{i}}{\rho_{i}} \mathbf{f}_{i} \times \nabla W(\mathbf{x} - \mathbf{x}_{i}, h)
\end{aligned} \tag{14}
$$
由于 $\nabla \times (\alpha \mathbf{v}) = \alpha \nabla \times \mathbf{v} + \nabla \alpha \times \mathbf{v}$，其中 $\alpha$ 是一个标量函数，$\mathbf{v}$ 是一个向量函数。

### Errors

上述近似的误差都是 $O(h^{2})$ 的，即二阶精度。我们可以通过 Taylor 展开来分析误差来源：
$$
f(\mathbf{x}') = f(\mathbf{x} + \mathbf{r}) = f(\mathbf{x}) + \mathbf{r} \cdot \nabla f(\mathbf{x}) + \frac{1}{2} \mathbf{r}^{\top} \nabla^{2} f(\mathbf{x}) \mathbf{r} + O(|\mathbf{r}|^{3})
$$
带入积分近似中：
$$
\begin{aligned}
f(\mathbf{x}) &\approx \int_{\mathbb{R}^{3}} \left[ f(\mathbf{x}) + \mathbf{r} \cdot \nabla f(\mathbf{x}) + \frac{1}{2} \mathbf{r}^{\top} \nabla^{2} f(\mathbf{x}) \mathbf{r} + O(|\mathbf{r}|^{3}) \right] \underbrace{W(-\mathbf{r}, h)}_{= W(\mathbf{r}, h)} \, \mathrm{d}\mathbf{r} \\
&= f(\mathbf{x}) \underbrace{\int_{\mathbb{R}^{3}} W(\mathbf{r}, h) \, \mathrm{d}\mathbf{r}}_{= 1} + \nabla f(\mathbf{x}) \cdot \underbrace{\int_{\mathbb{R}^{3}} \mathbf{r} W(\mathbf{r}, h) \, \mathrm{d}\mathbf{r}}_{= 0} + \frac{1}{2} \int_{\mathbb{R}^{3}} \mathbf{r}^{\top} \nabla^{2} f(\mathbf{x}) \mathbf{r} W(\mathbf{r}, h) \, \mathrm{d}\mathbf{r} + O(h^{3})
\end{aligned}
$$
第二项为零是因为 $W$ 是一个径向对称的函数，$\mathbf{r} W(\mathbf{r}, h)$ 的积分在空间中相互抵消。第三项中，设核函数为：
$$
W(\mathbf{r}, h) = \frac{1}{h^{3}} \phi\left( \frac{\mathbf{r}}{h} \right),
$$
其中 $\phi$ 满足归一化且径向对称。令 $\mathbf{q} = \mathbf{r}/h$，则： 
$$
\mathrm{d}\mathbf{r} = h^{3} \mathrm{d}\mathbf{q},\quad W(\mathbf{r}, h)\,\mathrm{d}\mathbf{r} = \phi(\mathbf{q})\,\mathrm{d}\mathbf{q}.
$$ 
代入得：
$$
\frac{1}{2} \int_{\mathbb{R}^{3}} (h\mathbf{q})^{\top} \nabla^{2} f(\mathbf{x}) (h\mathbf{q}) \; \phi(\mathbf{q}) \,\mathrm{d}\mathbf{q}
= \frac{h^{2}}{2} \int_{\mathbb{R}^{3}} \mathbf{q}^{\top} \nabla^{2} f(\mathbf{x}) \mathbf{q} \; \phi(\mathbf{q}) \,\mathrm{d}\mathbf{q}.
$$
因此整个表达式为 $O(h^{2})$。同样的分析也适用于梯度、散度和旋度的近似，误差同样为 $O(h^{2})$。

### Improved Approximations for Spatial Derivatives

上述的方法拟合常数都无法做到完全精确。为了解决这个问题，我们利用恒等式：
$$
\nabla (1 f) \equiv 1 \nabla f + f \nabla 1 \Longrightarrow \nabla f \equiv \nabla f - f \nabla 1 \tag{17}
$$
可以得到更好的梯度近似：
$$
\begin{aligned}
\nabla f(\mathbf{x}) &\approx \sum_{i} \frac{m_{i}}{\rho_{i}} f_{i} \nabla W(\mathbf{x} - \mathbf{x}_{i}, h) - f(\mathbf{x}) \sum_{i} m_{i} \frac{1}{\rho_{i}} \nabla W(\mathbf{x} - \mathbf{x}_{i}, h) \\
&= \sum_{i} \frac{m_{i}}{\rho_{i}} \biggl[ f_{i} - f(\mathbf{x}) \biggr] \nabla W(\mathbf{x} - \mathbf{x}_{i}, h)
\end{aligned} \tag{20}
$$
这样，常数拟合就能做到完全精确了。同样的，对于散度，我们有：
$$
\nabla \cdot \mathbf{f}(\mathbf{x}) \approx \sum_{i} \frac{m_{i}}{\rho_{i}} \biggl[ \mathbf{f}_{i} - \mathbf{f}(\mathbf{x}) \biggr] \cdot \nabla W(\mathbf{x} - \mathbf{x}_{i}, h)
$$

---

除了用前一个恒等式，还可以用下面的恒等式：
$$
\nabla (\rho^{n} f) \equiv n \rho^{n - 1} f \nabla \rho + \rho^{n} \nabla f \Longrightarrow \nabla f \equiv \frac{1}{\rho^{n}} \left[ \nabla (\rho^{n} f) - n \rho^{n - 1} f \nabla \rho \right] \tag{21}
$$

因此：
$$
\begin{aligned}
\nabla f(\mathbf{x}) &\approx \frac{1}{\rho(\mathbf{x})^{n}} \left[ \sum_{i} \frac{m_{i}}{\rho_{i}} (\rho_{i}^{n} f_{i}) \nabla W(\mathbf{x} - \mathbf{x}_{i}, h) - n \rho(\mathbf{x})^{n - 1} f(\mathbf{x}) \sum_{i} \frac{m_{i}}{\rho_{i}} \rho_{i} \nabla W(\mathbf{x} - \mathbf{x}_{i}, h) \right] \\
&= \frac{1}{\rho(\mathbf{x})^{n}} \sum_{i} m_{i} \biggl[ \rho_{i}^{n - 1} f_{i} - n \rho(\mathbf{x})^{n - 1} f(\mathbf{x}) \biggr] \nabla W(\mathbf{x} - \mathbf{x}_{i}, h)
\end{aligned} \tag{23}
$$
取 $n = 1$，可以得到之前的结果；取 $n = -1$，可以得到：
$$
\nabla f(\mathbf{x}) \approx \rho(\mathbf{x}) \sum_{i} m_{i} \biggl[ \frac{f_{i}}{\rho_{i}^{2}} + \frac{f(\mathbf{x})}{\rho(\mathbf{x})^{2}} \biggr] \nabla W(\mathbf{x} - \mathbf{x}_{i}, h) \tag{25}
$$

### Smoothing Kernels

常用的核函数有以下几种：
- **Gaussian Kernel**：
$$
W(\mathbf{r}, h) = \frac{1}{h^{3} \pi^{3/2}} \exp\left( -\frac{|\mathbf{r}|^{2}}{2 h^{2}} \right), \tag{30}
$$
- **Cubic Spline Kernel**：
$$
W(\mathbf{r}, h) = \frac{1}{\pi h^{3}} \begin{cases}
1 - \dfrac{3}{2} \left( \dfrac{|\mathbf{r}|}{h} \right)^{2} + \dfrac{3}{4} \left( \dfrac{|\mathbf{r}|}{h} \right)^{3}, & 0 \leq |\mathbf{r}| < h, \\
\dfrac{1}{4} \left( 2 - \dfrac{|\mathbf{r}|}{h} \right)^{3}, & h \leq |\mathbf{r}| < 2h, \\
0, & |\mathbf{r}| \geq 2h.
\end{cases}, \tag{31}
$$

## Fluid Equations

连续性方程（质量守恒）：
$$
\frac{\partial \rho}{\partial t} + \nabla \cdot (\rho \mathbf{v}) = 0 \tag{34}
$$
其中 $\rho$ 是密度，$\mathbf{v}$ 是速度场。

欧拉方程（动量守恒）：
$$
\frac{\partial (\rho \mathbf{v})}{\partial t} + \nabla \cdot (\rho \mathbf{v} \otimes \mathbf{v}) + \nabla p = \rho \mathbf{f} \tag{35}
$$
其中 $p$ 是压力，$\mathbf{f}$ 是外力。

能量方程（能量守恒）：
$$
\frac{\partial u}{\partial t} + \nabla \cdot [(u + p) \mathbf{v}] = 0 \tag{36}
$$
其中 $u$ 是单位质量的内能。

一共 6 个未知数，5 个方程（连续性方程、欧拉方程的三个分量、能量方程），因此还需要一个状态方程来闭合系统，例如理想气体状态方程：
$$
p = (\gamma - 1) u\rho \tag{38}
$$
其中 $\gamma$ 是绝热指数。

### Conservation of Mass

对于密度，同样由离散粒子重建：
$$
\rho(\mathbf{x}) \approx \sum_{i} m_{i} \, W(\mathbf{x} - \mathbf{x}_{i}, h)
$$
特别地，在粒子位置 $\mathbf{x}_{j}$ 处，密度为：
$$
\rho_{j} \approx \sum_{i} m_{i} W(\mathbf{x}_{j} - \mathbf{x}_{i}, h) \triangleq \sum_{i} m_{i} W_{ji} \tag{39}
$$
带入连续性方程的左侧：
$$
\begin{aligned}
\frac{\partial \rho_{j}}{\partial t} &\approx \sum_{i} m_{i} \frac{\partial W_{ji}}{\partial t} \\
&= \sum_{i} m_{i} \left[ \frac{\partial W_{ji}}{\partial \mathbf{x}_{j}} \cdot \frac{\mathrm{d} \mathbf{x}_{j}}{\mathrm{d} t} + \frac{\partial W_{ji}}{\partial \mathbf{x}_{i}} \cdot \frac{\mathrm{d} \mathbf{x}_{i}}{\mathrm{d} t} \right] \\
&= \sum_{i} m_{i} \biggl[ \nabla_{\mathbf{x}_{j}} W_{ji} \cdot \mathbf{v}_{j} + \nabla_{\mathbf{x}_{i}} W_{ji} \cdot \mathbf{v}_{i} \biggr] && (\nabla_{\mathbf{x}_{j}} W_{ji} = - \nabla_{\mathbf{x}_{i}} W_{ji}) \\
&= \sum_{i} m_{i} \underbrace{(\mathbf{v}_{j} - \mathbf{v}_{i})}_{\triangleq \mathbf{v}_{ji}} \cdot \nabla_{\mathbf{x}_{j}} W_{ji}
\end{aligned} \tag{41}
$$

连续性方程的右侧是在 $\mathbf{x}_{j}$ 处的散度，因此连续性方程的离散化形式为：
$$
\sum_{i} m_{i} (\mathbf{v}_{j} - \mathbf{v}_{i}) \cdot \nabla_{\mathbf{x}_{j}} W(\mathbf{x}_{j} - \mathbf{x}_{i}, h) + \rho_{j} (\nabla \cdot \mathbf{v})_{\mathbf{x}_{j}} = 0 \tag{43}
$$

### Conservation of Momentum

**拉格朗日量**：对于无粘、无外力的可压缩流体，拉格朗日量 $\mathcal{L}$ 为动能减去内能（势能）：
$$
\begin{aligned}
\mathcal{L}(\mathbf{x}, \mathbf{v}) &= \int_V \left( \frac{1}{2} \rho \mathbf{v} \cdot \mathbf{v} - \rho u \right) \mathrm{d}\mathbf{x} \\
&\approx \sum_{i} m_{i} \left( \frac{1}{2} \mathbf{v}_{i} \cdot \mathbf{v}_{i} - u_{i} \right) && (m_{i} = \rho_{i} \Delta V_{i})
\end{aligned} \tag{46}
$$
注意 $u_{i} = u(\rho_{i}, p_{i})$，而 $\rho_{i}$ 由粒子位置决定（通过 SPH 密度求和），因此 $u_{i}$ 是 $\mathbf{x}$ 的函数。

每个粒子 $j$ 都满足欧拉-拉格朗日方程：
$$
\frac{\mathrm{d}}{\mathrm{d}t}\left( \frac{\partial \mathcal{L}}{\partial \mathbf{v}_{j}} \right) - \frac{\partial \mathcal{L}}{\partial \mathbf{x}_{j}} = \mathbf{0} \tag{47}
$$

速度导数项为：
$$
\frac{\partial \mathcal{L}}{\partial \mathbf{v}_{j}} = \frac{\partial}{\partial \mathbf{v}_{j}} \sum_{i} m_{i} \left( \frac{1}{2} \mathbf{v}_{i} \cdot \mathbf{v}_{i} - u_{i} \right) = m_{j} \mathbf{v}_{j} \tag{48}
$$

位置导数项为：
$$
\frac{\partial \mathcal{L}}{\partial \mathbf{x}_{j}} = - \sum_{i} m_{i} \frac{\partial u_{i}}{\partial \mathbf{x}_{j}} = - \sum_{i} m_{i} \left[\frac{\partial u_{i}}{\partial \rho_{i}} \frac{\partial \rho_{i}}{\partial \mathbf{x}_{j}} + \frac{\partial u_{i}}{\partial p_{i}} \frac{\partial p_{i}}{\partial \mathbf{x}_{j}} \right] \tag{49}
$$

利用热力学关系（等熵流动，$p = p(\rho)$）：
$$
\begin{aligned}
\frac{\partial \mathcal{L}}{\partial \mathbf{x}_{j}} &= - \sum_{i} m_{i} \left[\frac{\partial u_{i}}{\partial \rho_{i}} + \frac{\partial u_{i}}{\partial p_{i}} \frac{\partial p_{i}}{\partial \rho_{i}} \right] \frac{\partial \rho_{i}}{\partial \mathbf{x}_{j}} \\
&= - \sum_{i} m_{i} \frac{p_{i}}{\rho_{i}^{2}} \frac{\partial \rho_{i}}{\partial \mathbf{x}_{j}}
\end{aligned} \tag{52}
$$

用 SPH 近似密度梯度：
$$
\begin{aligned}
\frac{\partial \rho_{i}}{\partial \mathbf{x}_{j}} &\approx \nabla_{\mathbf{x}_{j}} \sum_{k} m_{k} W(\mathbf{x}_{i} - \mathbf{x}_{k}, h) \\
&= \sum_{k} m_{k} (\delta_{ij} - \delta_{jk}) \nabla_{\mathbf{x}_{j}} W_{ik} \\
\end{aligned} \tag{57}
$$

全部代入整理得到：
$$
\begin{aligned}
\frac{\partial \mathcal{L}}{\partial \mathbf{x}_{j}} &= - \sum_{i} m_{i} \frac{p_{i}}{\rho_{i}^{2}} \sum_{k} m_{k} (\delta_{ij} - \delta_{jk}) \nabla_{\mathbf{x}_{j}} W_{ik} \\
&= - m_{j} \frac{p_{j}}{\rho_{j}^{2}} \sum_{k} m_{k} \nabla_{\mathbf{x}_{j}} W_{jk} + \sum_{i} m_{i} \frac{p_{i}}{\rho_{i}^{2}} m_{j} \nabla_{\mathbf{x}_{j}} W_{ij} \\
&= - m_{j} \sum_{i} m_{i} \left( \frac{p_{i}}{\rho_{i}^{2}} + \frac{p_{j}}{\rho_{j}^{2}} \right) \nabla_{\mathbf{x}_{j}} W_{ij} && (\nabla_{\mathbf{x}_{j}} W_{ji} = - \nabla_{\mathbf{x}_{i}} W_{ji})
\end{aligned} \tag{60}
$$

将两项代入欧拉-拉格朗日方程，消去 $m_{j}$：

$$
\boxed{ \frac{\mathrm{d}\mathbf{v}_{j}}{\mathrm{d}t} = - \sum_{i} m_{i} \left( \frac{p_{j}}{\rho_{j}^{2}} + \frac{p_{i}}{\rho_{i}^{2}} \right) \nabla_{\mathbf{x}_{j}} W_{ji} } \tag{61}
$$

这是 SPH 中压力梯度的离散形式，具有**成对对称性**：交换 $i$ 和 $j$ 后，$\nabla_{\mathbf{x}_{j}} W_{ji}$ 变号，而括号内对称，因此粒子间作用力大小相等、方向相反。

**线动量守恒**：系统总动量 $\mathbf{p} = \sum_{j} m_{j} \mathbf{v}_{j}$，其时间导数为：
$$
\frac{\mathrm{d}\mathbf{p}}{\mathrm{d}t} = \sum_{j} m_{j} \frac{\mathrm{d}\mathbf{v}_{j}}{\mathrm{d}t} = -\sum_{j} \sum_{i} m_{j} m_{i} \left( \frac{p_{j}}{\rho_{j}^{2}} + \frac{p_{i}}{\rho_{i}^{2}} \right) \nabla_{j} W_{ji} \tag{62}
$$
交换 $i, j$ 并利用反对称性，得到自身的相反数，故总和为零。因此**线动量严格守恒**。

**角动量守恒**：总角动量 $\mathbf{L} = \sum_{j} m_{j} \mathbf{x}_{j} \times \mathbf{v}_{j}$，求导：
$$
\frac{\mathrm{d}\mathbf{L}}{\mathrm{d}t} = \sum_{j} m_{j} \mathbf{x}_{j} \times \frac{\mathrm{d}\mathbf{v}_{j}}{\mathrm{d}t} = -\sum_{j} \sum_{i} m_{j} m_{i} \left( \frac{p_{j}}{\rho_{j}^{2}} + \frac{p_{i}}{\rho_{i}^{2}} \right) \mathbf{x}_{j} \times \nabla_{j} W_{ji} \tag{66}
$$
同样**角动量严格守恒**。

### Conservation of Energy

系统总能量为：
$$
E = \sum_{j} m_{j} \left( \frac{1}{2} \mathbf{v}_{j} \cdot \mathbf{v}_{j} + u_{j} \right) \tag{67}
$$
类似的方法得到：
$$
\frac{\mathrm{d}E}{\mathrm{d}t} = \sum_{i} \sum_{j} m_{i} m_{j} \left( \frac{p_{j}}{\rho_{j}^{2}} \mathbf{v}_{i} + \frac{p_{i}}{\rho_{i}^{2}} \mathbf{v}_{j} \right) \cdot \nabla_{\mathbf{x}_{j}} W_{ji} \tag{71}
$$
同样**能量严格守恒**。

## Finding the Nearest Neighbor

SPH 中最重要的问题是找到最近邻。可以使用空间哈希或者树结构来加速邻居搜索。

## Integration and Timestepping
