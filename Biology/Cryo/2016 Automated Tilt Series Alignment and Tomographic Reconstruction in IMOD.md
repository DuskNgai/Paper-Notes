# Automated Tilt Series Alignment and Tomographic Reconstruction in IMOD

## 0 Abstract

在有金颗粒标记对齐时，可以自动选择需要在 tilt series 中跟踪的标记；如果有多余的标记可用，将选择一个最有可能良好跟踪的子集。通过应用增强边缘的 Sobel 算子来优化标记位置，plastic-embedded 样本的对准误差减少了 20%，frozen-hydrated 样本减少了 10%。

采用了 robust fitting，即在计算拟合误差时，赋予离群点较低或无权重，以获得对齐解决方案，因此来自自动跟踪的异常点对对齐的影响很小。在合并两个双轴断层扫描图像时，通过局部 patch 之间的相关性来优化它们之间的对齐；还开发了一种结构度量，以便可以自动排除结构不足无法提供准确相关性的小块。

## 1 Introduction

1. 改进了获取基准标记位置的模型，改进了根据这些位置解决 tilt series 对齐的方法。
2. 应用了确定 tomogram 中结构位置的方法，解决了几个问题，包括精确对齐双轴 tilt series 的两个层析图。
3. 创建了管理自动处理过程的工具。

## 2 Methods and Results

### 2.1 Automatic Generation of A Seed Model

IMOD 构建基准模型的方法：
1. 从接近 0 度倾斜的选定金颗粒的“种子模型”
2. 使用 Beadtrack 程序从一张图像追踪到下一张图像。

优点：可以使用先验知识检测到淡弱或被遮挡的金颗粒，筛去图像上不满足某些检测阈值的金颗粒。

IMOD 已经包含了一个自动构建基准模型的程序，RAPTOR，其缺点是：
- RAPTOR 可能无法检测位于较暗区域上的金颗粒；例如，当细胞被树脂或冰包围时，它可能几乎只选择空区域上方的金颗粒。**实际上，对于 plastic-embedded 材料的重建来说，常常希望得到空区域下方的金颗粒，因为在电子束下，空树脂的形变方式与含有细胞材料的树脂不同。**
- RAPTOR 无法限制选择金颗粒的区域；在高倾斜度时可能会选择远离待重建区域的金颗粒。
- 对于拼接 tilt series ，其显著的非线性畸变必须用局部对齐来拟合，因为它只能将所有点拟合到一个全局对齐上。
- 金颗粒位置的准确度不如 Beadtrack。

因此，我们决定以现有的先选定种子后跟踪的方法为基础。为此，有必要开发一种自动生成种子模型的程序。总体目标是得到一个至少与用户手动选择的效果一样好的模型。金颗粒的选取标准是：能够良好跟踪且分布在平面和切片两侧（当它们被沉积在两侧时）。
金颗粒数量也不应多于为获得符合问题要求的重建质量所需的数量，因为如果要人工填补缺失点和检查偏离位置，过多的金颗粒会带来额外的工作。

Autofidseed 脚本程序如下：
1. Imodfindbeads 程序去检测 0 度角附近 3 - 7 个视角内的所有金颗粒。（图 1A）
	1. 图像与某个特定大小的模板金颗粒做交叉相关。
	2. 分析一幅经过平滑处理的相关峰强度直方图（图 1C/D 中的蓝线），寻找两种模式之中的凹陷（极小值点）：首先在平滑度较高的直方图内寻找该凹陷，然后在平滑度较低的直方图中小范围地寻找凹陷。平滑度较低的直方图中的凹陷点的位置（图 1C/D 中的箭头和红线）被认为是区分金颗粒和非金颗粒之间的最佳*阈值*。这里使用了原始相关性得分而不是归一化交叉相关系数（CCC），是因为交叉相关系数丢失了强度信息，不能很好地区分金颗粒和形状相同但较弱的特征。
	3. 高于阈值的点，或者在直方图分析失效时高于保守的后备阈值的点，将被对齐并求和，以获得一个参考点，用于迭代相关性、峰值搜索和直方图分析。因此，金颗粒的最终选择是基于与平均金颗粒的相关性，而不是基于模型。
	4. 为了减轻由于低密度背景上强相关性的金颗粒所造成的偏差，根据背景密度将相关峰分为 4 组，并分别分析每组的直方图。然后从成功分析中发现的阈值推导出阈值与背景密度之间的关系，前提是至少有两个阈值是成功的。因此，判断任何给定点是否是金颗粒的标准可以基于其背景密度。
	5. 当金颗粒数量较少时，分析的视角数量会增加到 7 个，以使分析更加稳健。
	6. (C, D) 分别用于 (A) 和 (E) 部分的 tilt series 中寻找金颗粒时相关峰强度分布的平滑直方图。直方图用 $(1-(x/h)^2)^3/(0.9143h)$ 的卷积核进行平滑处理，其中 $h$ 是核的半宽度。首先在 $h=0.2$ 的直方图（蓝色曲线）中寻找两个最大值之间的凹陷，然后在 $h=0.05$ 的直方图（红色曲线）中选取其最终位置（箭头）。(C) 中的直方图是非典型的，因为有大量聚集的金颗粒，所以两种模式的面积是可比较的；(D) 中的直方图是典型的，是需要对数缩放才能清楚地显示金颗粒的模式。
2. 
	1. 从第一步的输出中提取出三种不同的种子模型，然后每个种子模型都运行 Beadtrack，通过围绕 0 度倾斜的同一组 11 个图片来跟踪金颗粒。使用这些数字的原因是，它们通常能提供足够的信息，说明跟踪的一致性和质量，以及金颗粒在三维空间中的位置，而无需过多的计算。
	2. （图 1E）显示了此类轨迹的侧视图，在三次运行中只有一次或两次跟踪到金颗粒，分别用绿色或品红色表示。
	3. Beadtrack 可以在一系列局部区域内处理成千上万的金颗粒，即使在金颗粒成团或有一些非金颗粒的情况下，它也能充分完成这项工作。它可以存储每个金颗粒的跟踪信息，包括三维坐标和拟合三维对齐方程时的平均误差。
3. 如果金颗粒出现在两个表面上，则使用 Sortbeadsurfs 程序根据求解的三维坐标将每次 Beadtrack 运行的金颗粒分配到两个表面上。生成的模型文件中的点被染成绿色或洋红色（如图 1F）。
4. Pickbestseed 程序会得到迄今为止收集到的所有信息。
	1. 它将不同跟踪之间的金颗粒进行匹配，然后给每颗金颗粒打分，分数是衡量跟踪一致性和质量的四项指标的等权总和：金颗粒缺失的轨迹比例、轨迹中缺失点的比例、三维对齐的平均误差以及轨迹对中相应点之间的平均距离。
	2. 根据 Beadtrack 计算出的伸长量，每颗金颗粒还被分为聚集型（即在高倾斜度下预计与另一颗金颗粒过于接近）和拉长型。拉长的原因既可能是一颗金颗粒形状不对，也可能是两颗金颗粒在某些角度上紧靠在一起，而在其他角度上投影位置略有不同；无论哪种情况，金颗粒都是不理想的。除非用户选择将金颗粒包含在内，否则聚集或拉长的金颗粒不在考虑之列，在这种情况下，只有在必要时才会添加金颗粒，以达到所需的种子数量。
	3. 程序会经过几个不同的阶段来选择金颗粒，从得分最高的金颗粒开始，然后降低标准，以填补密度较低的区域。如果尚未达到要求的密度，程序会进入后面的阶段。如果金颗粒位于两个表面上，则对每个表面的密度分别进行评估，以优化两面之间的平衡。

两个自动挑选种子的特性：
1. 可以使用模型的轮廓来定义选择金颗粒的区域。在 plastic-embedded 细胞的 ET 中，通常希望排除在空树脂上的金颗粒，如上所述。另一方面，在 Cryo-ET 中，可能希望排除碳膜上的金颗粒，因为其行为可能与冰不同。
2. Etomo 的自动定位 UI 提供了访问不同参数重新运行最终步骤的几个选项。这里的逻辑是，对于需要超过一百个基准的大面积用户，尝试一些选项仍然比手动创建基准模型更有效率。

### 2.2 Robust Fitting

使用 IMOD 中的对齐程序 Tiltalign 时，测量值比待解参数至少多出 4 倍。这种超出量给出的解可以平均掉一些随机误差，但不能有效减少离群点的影响，因为离群点往往会使解偏离正确位置点。

首先进行普通最小二乘拟合，然后各点根据其残差来分配权重。在计算最小二乘时使用这些权重反复拟合，然后分配新的权重，整个过程反复进行，直到权重的变化足够小为止。

|Symbols|Descriptions|
|:-----:|:----------:|
|MADN|normalized median absolute deviation from the median|
|$e_i$|error for the $i$-th point|
|$w_i$|weight for the $i$-th point|
|$m_p$|an estimate of the median error for the projection $p$ where point $i$ is located|
|$m_a$|the median of the values of $e_i/m_p$|
|$k$|a factor that determines how extreme a point must be to receive a weight of 0|

首先计算 MADN：即偏离中位数误差绝对值的中位数除以 0.6745（该系数使 MADN 成为正态分布样本的西格玛估计值），从而从误差集合中分配权重。然后，对于误差为 $e_i$ 的第 $i$ 个点，权重 $w_i$ 由 Tukey 方程表达式求得：
$$
\begin{align*}
d_i&=(e_i/m_p-m_a)/(k*\mathrm{MADN})\\
w_i&=\begin{cases}1&d_i<0\\(1-d_i^2)^2&0\le d_i\le 1\\0&d_i>1\end{cases}
\end{align*}\tag{1}
$$



Translate the given sentences into Chinese with words that are frequently used by structure biologist:
### 2.3 Sobel Filtering to Improve Bead Localization

### 2.4 Automatic Alignment of Dual-axis Tomograms

### 2.5 Fitting the Surface of the Specimen


