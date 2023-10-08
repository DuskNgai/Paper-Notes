# Improvements on Marker-free Images Alignment for Electron Tomograph

## 0 Abstract

Patch Tracking 的改进。

## 1 Introduction

最流行且最简单的数据采集几何结构是单轴 tilt series，尽管还有其他可能的几何结构，如双轴、多轴或锥形倾斜。

金颗粒标记非常有效，但由于标记物并不总是可见或可跟踪的，或者因为它们不可用，因此并不总是适用。此外，使用标记物需要用户在图像处理过程中干预，手动提示图像上标记物的位置，因此无法自动处理整个数据集。

单颗粒的 Cryo-EM 研究表明，基于图像信息的算法，如交叉相关，在预先存在参考体积的情况下提供了最佳结果。然而，由于 Cryo-ET 中要重建的对象是独一无二的，无法生成参考体积。因此，需要基于仅有的 tilt series 图像中的信息进行校准。

使用交叉相关、无金颗粒标记来对齐 tilt series 有 3 种方法：

1. 将第一幅图像与第二幅图像对齐，第二幅与第三幅对齐，依此类推。它将 align 问题简化为多个图像对的对齐问题，而不是将整个 tilt series 视为一个整体。有着严重的跟踪漂移问题，每幅图像的倾斜轴和平移参数的估计不准确。
2. 计算出倾斜图的粗略重建，然后将 tilt series 与根据重投影进行迭代优化的倾斜图重新对齐。但在 Cryo-ET 中受限，因为用于计算第一个体积的投影数量很少，而且非常耗时。
3. 使用交叉相关来跟踪 tilt series 内的特征。特征是可以在相邻图像中跟踪图像中的特征点（由于对比度变化、边界、角等引起的）。特征跟踪允许定义在 tilt series 的某个子序列中可见的标记。

[A Complete Data Processing Workflow for Cryo-ET and Subtomogram Averaging](https://www.nature.com/articles/s41592-019-0591-8) 用第二种方法。具体步骤是：

1. 首先基于交叉相关和 Fourier 共线法进行初步粗略对齐后，重建了一个体像。
2. 在这个体像中，找出最暗的体素作为后续优化的 3D 标志。
3. 通过迭代对齐过程优化 3D 标志的坐标和图像变换。

[Automated Batch Fiducial-less Tilt-series Alignment in Appion Using Protomo](https://www.sciencedirect.com/science/article/pii/S1047847715300812) 也是第二种方法。具体步骤是：

1. tilt series 的预对齐，随后重建了初始体像。
2. 从这个初始体像开始，对几何模型和图像变换进行迭代优化。

[Automated Tilt Series Alignment and Tomographic Reconstruction in IMOD](2016 Automated Tilt Series Alignment and Tomographic Reconstruction in IMOD.md) 是第三种方法。具体步骤是：

1. 在初步粗略对齐之后，在中央切片上检测参考标记，以后在整个 tilt series 中跟踪其位置。
2. 从这个轨迹中，可以解决和调整倾斜轴的角度以及图像中的旋转和尺寸变化。
3. 这个实现也允许对系列进行无参考对齐，尽管作者声称它的效果略逊色一些。

作者之前的方法是第三种方法。具体步骤是：

1. 预对齐：使用交叉相关粗略地校正偏移、使用仿射变换估计了旋转和变形。
2. 在每张图像上划分了网格，以在格点放置种子。由于网格划在未对齐的图像上，因此种子不会位于相同的特征上。
3. 取一个 patch，并且使用预对齐参数，通过交叉相关找到前一张和后一张图像中对应的 patch。只要 patch 之间的相关得分足够高，就会生成一个标记链。
4. 长度小于用户定义的阈值的链条被丢弃。
5. 沿着标记的检测倾斜轴方向，逐步创建了标记的 3D 模型，同时检测了平面内的偏移和旋转（错误检测的标记链会被自动识别并移除）。

## 2 Methods





