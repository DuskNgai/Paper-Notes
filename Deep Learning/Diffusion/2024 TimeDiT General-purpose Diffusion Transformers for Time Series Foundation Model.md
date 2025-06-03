# TimeDiT General-purpose Diffusion Transformers for Time Series Foundation Model

机构：南加州大学

## Claim

1. 提出了一种新的时间序列模型 TimeDiT，不同于自回归模型，TimeDiT 通过扩散过程来建模时间序列数据。
2. 提出了一种无需微调的模型编辑策略，允许在采样过程中无缝集成外部知识，而无需更新任何模型参数。


## Motivation

1.现实世界中的时间序列数据往往含有缺失值，并且来自不同领域的数据通常具有多通道和多分辨率的特性。
2. 时间自动回归解码的单向性限制了领域知识的融入，例如以 PDE 表示的物理定律。

## Method

### Problem Definition

多元时间序列 $\mathbf{X} = \{x_{i,j}\} \in \mathbb{R}^{L \times K}$，其中 $L$ 为时间步长，$K$ 为通道数，$i \in \{1, \ldots, K\}$ 为通道索引，$j \in \{1, \ldots, L\}$ 为时间索引。一个掩码序列 $\mathbf{M}_{\text{obs}} = \{m_{i,j}\} \in \{0, 1\}^{L \times K}$，其中 $m_{i,j} = 1$ 表示 $x_{i,j}$ 已知，$m_{i,j} = 0$ 表示 $x_{i,j}$ 未知。$\mathbf{x}_{0}^{\text{obs}}$ 表示可见的序列，$\mathbf{x}_{0}^{\text{tar}}$ 表示目标序列。$\mathbf{x}_{0}^{\text{con}}$ 表示 $\mathbf{x}_{0}^{\text{obs}}$ 中可见的部分，当作生成 $\mathbf{x}_{0}^{\text{tar}}$ 的条件。最后，目标是用 $p_{\theta}(\mathbf{x}_{0}^{\text{tar}} | \mathbf{x}_{0}^{\text{con}})$ 拟合条件信息 $q_{\mathbf{X}}(\mathbf{x}_{0}^{\text{tar}} | \mathbf{x}_{0}^{\
text{con}})$。
$$
\begin{aligned}
p_{\theta}(\mathbf{x}_{0:T}^{\text{tar}} | \mathbf{x}_{0}^{\text{con}}) &= p(\mathbf{x}_{T}^{\text{tar}}) \prod_{t=1}^{T} p(\mathbf{x}_{t - 1}^{\text{tar}} | \mathbf{x}_{t}^{\text{tar}}, \mathbf{x}_{0}^{\text{con}}), \quad \text{where} \quad \mathbf{x}_{T}^{\text{tar}} \sim \mathcal{N}(\mathbf{0}, \mathbf{I}) \\
p_{\theta}(\mathbf{x}_{t - 1}^{\text{tar}} | \mathbf{x}_{t}^{\text{tar}}, \mathbf{x}_{0}^{\text{con}}) &= \mathcal{N}(\mathbf{x}_{t - 1}^{\text{tar}}; \mu_{\theta}(\mathbf{x}_{t}^{\text{tar}}| \mathbf{x}_{0}^{\text{con}}, t ), \sigma_{\theta}(\mathbf{x}_{t}^{\text{tar}} | \mathbf{x}_{0}^{\text{con}}, t)\mathbf{I})
\end{aligned}
$$

实际上这里就是把时间序列模型和扩散过程结合起来，用可见的序列作为条件，输入到扩散过程中，生成目标序列。

### Physics-Informed TimeDiT

这里改进了采样的过程。

设连续时间函数 $\mathbf{x}(\mathbf{u}, t)$ 满足 PDE：
$$
\frac{\partial \mathbf{x}}{\partial t} = \mathbf{F}(\mathbf{x}, t, \nabla_{\mathbf{u}} \mathbf{x}, \nabla_{\mathbf{u}}^2 \mathbf{x}, \ldots)
$$

