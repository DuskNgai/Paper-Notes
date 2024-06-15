# 3 Brownian Motion and "White Noise"

随机过程的基本概念。

## A Motivation and Definition

布朗运动 $W$，也叫维纳过程，满足以下性质：
1. $W(0) = 0$.
2. 对于 $0 \leq s \leq t$，$(W(t) - W(s)) \sim \mathcal{N}(0, t - s)$.
3. 独立增量：对于 $0 \leq s_{1} < t_{1} \leq s_{2} < t_{2} \leq \cdots < s_{n} < t_{n}$，$(W(t_{1}) - W(s_{1})), (W(t_{2}) - W(s_{2})), \cdots, (W(t_{n}) - W(s_{n}))$ 两两独立。
