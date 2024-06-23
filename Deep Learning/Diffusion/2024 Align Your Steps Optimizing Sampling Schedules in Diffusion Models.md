# Align Your Steps: Optimizing Sampling Schedules in Diffusion Models

## Claim

为了解决 Diffusion 采样慢的问题，之前的方法是从优化 SDE 数值求解器入手，而作者从找到一个最优的采样调度入手。

论文的主要贡献包括：
1. 分析了最优采样时间表与真实数据分布的依赖性。
2. 提出了 Align Your Steps (AYS)，这是一个针对任何数据集、模型和 SDE 求解器的采样时间表的优化的通用和原则性框架。
3. 1对多种流行的随机和确定性求解器改进了以前的启发式采样时间表，特别是在 NFE 情况下。

## Motivation

当前数值解 SDE 的方法还是用 Ito 积分的方法。作者引入一种原则性方法，以特定数据集的方式优化采样调度，从而在计算预算相同的情况下提升采样效率。

该框架基于以下观察结果：所有 SDE 求解器都可以被重新解释为在短时间间隔上精确求解近似的线性 SDE。这样，我们就可以利用随机微积分的技术，将近似线性 SDE 的求解与真正的生成 SDE 的求解之间的不匹配问题最小化，并将其视为对采样调度的优化问题。

## Method

![](images/align-your-steps.png)

先从一个 Case Study 出发。假设数据分布是 $p_{\text{data}}\sim\mathcal{N}(\mathbf{0},c^2\mathbf{I})$、$s(t)=1,\sigma(t)=t$，则前向 SDE 和 反向 ODE 分别是：
$$
\begin{cases}
\text{Forward SDE:} & \mathrm{d}\mathbf{x} = \sqrt{2t}\mathrm{d}\mathbf{w} \\
\text{Reverse ODE:} & \mathrm{d}\mathbf{x} = -t\nabla_{\mathbf{x}}\log p(\mathbf{x}, t)\mathrm{d}t
\end{cases}
$$

## Result
