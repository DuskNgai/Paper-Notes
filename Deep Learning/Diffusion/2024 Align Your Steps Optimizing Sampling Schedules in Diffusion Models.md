# Align Your Steps: Optimizing Sampling Schedules in Diffusion Models

## Claim

通过调整 SDE 求解器的采样时间表来提高采样效率。

贡献：
1. 分析了最优采样时间表与真实数据分布的依赖性。
2. 提出了 Align Your Steps (AYS)，这是一个针对任何数据集、模型和 SDE 求解器的采样时间表的优化的通用和原则性框架。
3. 对多种流行的随机和确定性求解器改进了以前的启发式采样时间表，特别是在 NFE 情况下。

## Motivation

- Diffusion 采样太多就会慢。
- 当前数值求解 SDE 的方法还是基于 Ito 积分的方法，之前的方法是从优化 SDE 数值求解器入手。
- 所有 SDE 求解器都可以被重新解释为在短时间间隔上精确求解近似的线性 SDE。

将近似线性 SDE 的求解与真正的生成 SDE 的求解之间的不匹配问题最小化，并将其视为对采样调度的优化问题。

## Method

![](images/align-your-steps.png)


## Result

---

先从一个 Case Study 出发。假设数据分布是 $p_{\text{data}}\sim\mathcal{N}(\mathbf{0},c^2\mathbf{I})$、$s(t)=1,\sigma(t)=t$，则前向 SDE 和 反向 ODE 分别是：
$$
\begin{cases}
\text{Forward SDE:} & \mathrm{d}\mathbf{x} = \sqrt{2t}\mathrm{d}\mathbf{w} \\
\text{Reverse ODE:} & \mathrm{d}\mathbf{x} = -t\nabla_{\mathbf{x}}\log p(\mathbf{x}, t)\mathrm{d}t
\end{cases}
$$


