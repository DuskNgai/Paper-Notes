# 第 13 章 反常积分和含参变量的积分

## 13.1 反常积分

### 13.1.1 无穷区间上积分的收敛性

如果函数 $f(x)$ 在 $[a,\infty)$ 的**任意有限区间上可积**，并且
$$
\int_{a}^{+\infty}f(x)\mathrm{d}x=\lim_{A\to+\infty}\int_{a}^{A}f(x)\mathrm{d}x
$$
存在极限，则积分 $\int_{a}^{+\infty}f(x)\mathrm{d}x$ 收敛。对于任意的 $A>a$，定义函数 $F(A)=\int_{a}^{A}f(x)\mathrm{d}x$。因此，$\int_{a}^{+\infty}f(x)\mathrm{d}x$ 收敛等价于 $\lim_{A\to+\infty}F(A)$ 有极限。

**定理 13.1（Cauchy 收敛准则）** $\int_{a}^{+\infty}f(x)\mathrm{d}x$ 收敛的充要条件是
$$
\forall\epsilon>0,\exists A_0>a,\exists A_1,A_2>A_0\Rightarrow|F(A_2)-F(A_1)|=\left|\int_{A_1}^{A_2}f(x)\mathrm{d}x\right|<\epsilon
$$
而后者成立的条件不需要由 $\lim_{x\to+\infty}f(x)=0$ 来保证。

**定理 13.2** 如果 $\int_{a}^{+\infty}|f(x)|\mathrm{d}x$ 收敛，那么 $\int_{a}^{+\infty}f(x)\mathrm{d}x$ 收敛。
