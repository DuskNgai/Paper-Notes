# Chapter 3 Monte Carlo Methods

## 3.2 Why Are Monte Carlo Techniques Useful?

Monte Carlo 方法的一大优势在于其概念简单；Monte Carlo 技术的另一个优点是它们可以应用于广泛的问题。
One major strength of the Monte Carlo approach lies in its conceptual simplicity; another advantage of Monte Carlo techniques is that they can be applied to a wide range of problems.

Monte Carlo 技术的一个缺点是它们相对较慢的收敛速度 $1/\sqrt{N}$，其中 $N$ 是样本数。
One disadvantage of Monte Carlo techniques is their relatively slow convergence rate of $1/\sqrt{N}$, where $N$ is the number of samples.

但是存在 Monte Carlo 方法是唯一可行的求解技术的问题：高维积分和具有非光滑被积函数的积分等。
But there are problems for which Monte Carlo methods are the only feasible solution technique: higher-dimensional integrals and integrals with non-smooth integrands, among others.

## 3.3 Review of Probability Theory

## 3.4 Monte Carlo Integration

要估计（Estimate）
$$
I=\int_a^bf(x)\mathrm{d}x
$$

### 3.4.1 Weighted Sum of Random Variables

$$
G=\sum_{i=1}^Nw_ig_i
$$

$g_i$ are i.i.d random variables and $\sum_{i=1}^Nw_i=1$. Let $w_i=1/N$, then the expecation of $G$ is:
$$
E[G(x)]=\frac{1}{N}\sum_{i=1}^NE[g_i(x)]=\frac{1}{N}\sum_{i=1}^NE[g(x)]=E[g(x)]
$$
and the variance of $G$ is:
$$
\begin{align*}
\mathrm{Var}[G(x)]&=\mathrm{Var}\left[\frac{1}{N}\sum_{i=1}^Ng_i(x)\right]\\
&=\sum_{i=1}^N\mathrm{Var}\left[\frac{1}{N}g_i(x)\right]\quad(\text{i.i.d})\\
&=\frac{1}{N^2}\sum_{i=1}^N\mathrm{Var}\left[g_i(x)\right]\\
&=\frac{1}{N}\mathrm{Var}\left[g_i(x)\right]
\end{align*}
$$
因此，随着 $N$ 的增加，$G$ 的方差随着 $N$ 而减小，使得 $G$ 越来越好地估计 $E[g(x)]$。标准差 $\sigma$ 减小速度是 $\sqrt{N}$。
Thus, as $N$ increases, the variance of $G$ decreases with $N$, making $G$ an increasingly good estimator of $E[g(x)]$. The standard deviation $\sigma$ decreases as $\sqrt{N}$.

### 3.4.2 Estimator

估计器（Estimator）:
$$
\langle{I}\rangle=\frac{1}{N}\sum_{i=1}^{N}\frac{f(x_i)}{p(x_i)}
$$
这个估计器的方差（样本均值的方差）应该是：
$$
\begin{align*}
\mathrm{Var}_{p}[\langle{I}\rangle]&=\mathrm{Var}_{p}\left[\frac{1}{N}\sum_{i=1}^{N}\frac{f(x_i)}{p(x_i)}\right]\\
&=\frac{1}{N^2}\sum_{i=1}^{N}\mathrm{Var}_{p}\left[\frac{f(x_i)}{p(x_i)}\right]\\
&=\frac{1}{N}\left[\int_a^b\left(\frac{f(x)}{p(x)}\right)^2p(x)\mathrm{d}x-\left(\int_a^bf(x)\mathrm{d}x\right)^2\right]\\
&=\frac{1}{N}\int_a^b\left(\frac{f(x)}{p(x)}-I\right)^2p(x)\mathrm{d}x
\end{align*}
$$

### 3.4.3 Bias

当估计量的期望值恰好是积分 $I$ 的值时（如上述估计量的情况），则称该估计量是无偏的，否则就是有偏的。
When the expected value of the estimator is exactly the value of the integral $I$ (as is the case for the estimator described above), the estimator is said to be unbiased, otherwise it is biased.

bias: $B[\langle{I}\rangle]=E[\langle{I}\rangle]-I$. If $\lim_{N\to\infty}B[\langle{I}\rangle]=0$, then it is consistent.

### 3.4.4 Accuracy

Chebyshev's inquality:
$$
P\left[|\langle{I}\rangle-E[\langle{I}\rangle]|\ge\sqrt{\frac{\sigma_I^2}{\delta}}\right]\le\delta
$$
The Central Limit Theorem.

### 3.4.5 Estimating the Variance

 Monte Carlo 估计器的方差是:
$$
\begin{align*}
\mathrm{Var}_{p}[\langle{I}\rangle]&=\frac{1}{N}\int_a^b\left(\frac{f(x)}{p(x)}-I\right)^2p(x)\mathrm{d}x\\
&=\frac{1}{N}\left[\int_a^b\frac{f^2(x)}{p(x)}\mathrm{d}x-I^2\right]\\
\end{align*}
$$
离散化可得：
$$
\sigma_{\text{est}}^2=\frac{1}{N-1}\left[\frac{1}{N}\sum_{i=1}^N\left(\frac{f(x_i)}{p(x_i)}\right)^2-\left(\frac{1}{N}\sum_{i=1}^N\frac{f(x_i)}{p(x_i)}\right)\right]
$$

### 3.4.6 Deterministic Quadrature versus Monte Carlo

### 3.4.7 Multidimensional Monte Carlo Integration

$$
I=\iint f(x,y)\mathrm{d}x\mathrm{d}y\quad(\text{estimate})\\
\langle{I}\rangle=\frac{1}{N}\sum_{i=1}^N\frac{f(x_i,y_i)}{p(x_i, y_i)}
$$

Unlike deterministic quadrature techniques, which would require $N_d$ samples for a $d$-dimensional integration, Monte Carlo techniques permit an arbitrary choice of $N$.

## 3.5 Sampling Random Variables

### 3.5.1 Inverse Cumulative Distribution Function

#### Discrete Random Variables

Given a set of probabilities $p_i$, we want to pick $x_i$ with probability $p_i$.

首先计算 CDF：
$$
F_i=\sum_{j=1}^ip_j
$$
因此 $F_i$ 是 $[0,1]$ 范围内的数。然后采集一个满足 $[0,1]$ 均匀分布的样本 $u$，然后输出 $x_k$，其中 $k$ 满足：$F_{k-1}\le u<F_k$。

#### Continuous Random Variable

首先计算 CDF：
$$
F(x)=\int_{-\infty}^{x}p(t)\mathrm{d}t
$$
然后采集一个满足 $[0,1]$ 均匀分布的样本 $u$，然后输出 $F^{-1}(u)$。

#### Example

半球上的均匀分布：$P(\omega)=c$。

首先得到 $c$：
$$
\int_{\mathcal{H}^2}P(\omega)\mathrm{d}\omega=\int_{0}^{\pi/2}\int_{0}^{2\pi}c\sin\theta\mathrm{d}\theta\mathrm{d}\phi=2\pi c\\
c=\frac{1}{2\pi}
$$
随后得到关于 $\theta,\phi$ 的概率分布：
$$
P(\theta,\phi)=P(\omega)\sin\theta=\frac{\sin\theta}{2\pi}
$$
因此 CDF 为：
$$
\begin{align*}
F(\theta,\phi)&=\int_0^{\phi}\int_0^{\theta}\frac{\sin\theta'}{2\pi}\mathrm{d}\theta'\mathrm{d}\phi'\\
&=\frac{\phi}{2\pi}(1-\cos\theta)
\end{align*}
$$
由于变量可以分离，因此得到边缘分布为：
$$
F(\theta)=1-\cos\theta,F(\phi)=\frac{\phi}{2\pi}
$$
因此逆采样得到采样点为：
$$
\theta_i=\arccos(u_1),\phi_i=2\pi u_2
$$

### 3.5.2 Rejection Sampling

该方法将被采样的函数的维度提高一倍，然后对包含整个 PDF 的边界框进行均匀采样。
This method raises the dimension of the function being sampled by one and then uniformly samples the bounding box that includes the entire PDF.

首先将处在 $[a,b]$ 区间上的一维函数升维成二维函数，得到一个 $[a,b]\times[0,M]$ 的采样区域，随后均匀采样，得到 $(x_i,y_i)$。如果 $y_i\le p(x_i)$，则保留样本。这个过程可以化为现在 $[a,b]$ 上均匀采样得到 $x_i$，随后在 $[0,1]$ 上均匀采样得到 $u_i$，如果 $Mu_i\le p(x_i)$，则保留样本。

### 3.5.3 Look-Up Table

## 3.6 Variance Reduction

蒙特卡洛积分技术可以大致分为两类：那些没有关于被积函数的信息（有时称为盲蒙特卡洛），以及那些确实有某种信息的（有时称为知情蒙特卡洛）。
Monte Carlo integration techniques can be roughly subdivided into two categories: those that have no information about the function to be integrated (sometimes called blind Monte Carlo), and those that do have some kind of information (sometimes called informed Monte Carlo).

### 3.6.1 Importance Sampling

通过基于被积函数的信息明智地选择概率分布，可以减少计算的方差。
The variance of the computation can be reduced by choosing the probability distribution wisely based on information about the function to be integrated.

重要性采样的难点在于选择一个 $p(x)$ 使得方差最小。事实上，一个完美的估计器会使方差为零。
The difficulty of importance sampling is to choose a $p(x)$ such that the variance is minimized. In fact, a perfect estimator would have the variance be zero.

通过变分法，我们可以得知，当
$$
p(x)=\frac{|f(x)|}{\int_{D}f(x)dx}
$$
此时的方差会达到 0。但是这样的 $p(x)$ 需要我们提前知道积分的值，而这正是我们需要求的东西。

### 3.6.2 Stratified Sampling

分层抽样的基本思想是将集成域拆分为 $N$ 个不相交的子域（也称为层），并分别用一个或多个样本评估每个子域中的积分值。
The basic idea in stratified sampling is to split the integration domain into $N$ disjoint subdomains (also called strata) and evaluate the integral in each of the subdomains separately with one or more samples.
$$
\int_{a}^{b}f(x)dx=\sum_{i=0}^{N-1}\int_{c_i}^{c_{i+1}}f(x)dx
$$
与直接用蒙特卡罗积分方法相比，分层抽样通常会导致较小的方差。
Stratified sampling often leads to a smaller variance as compared to a blind Monte Carlo integration method.

$$
\mathrm{Var}[\langle{I}\rangle]=\sum_{j=0}^{N-1}\frac{c_{j+1}-c_j}{n_j}\int_{c_j}^{c_{j+1}}f^2(x)\mathrm{d}x-\sum_{j=0}^{N-1}\frac{1}{n_j}\left(\int_{c_j}^{c_{j+1}}f^2(x)\mathrm{d}x\right)^2
$$
其中 $n_j$ 是每个区间的采样数。假如采样区间有 $N$ 个，每个区间长度相同，每个区间采样数为 $1$，因此方差为：
$$
\mathrm{Var}[\langle{I}\rangle]=\frac{1}{N}\int_{0}^{1}f^2(x)\mathrm{d}x-\sum_{j=0}^{N-1}\frac{1}{n_j}\left(\int_{\frac{j}{N}}^{\frac{j+1}{N}}f^2(x)\mathrm{d}x\right)^2
$$
该表达式表明使用分层抽样获得的方差总是小于通过纯蒙特卡罗抽样方案获得的方差。因此，在单个层中生成多个样本没有任何优势，因为对层进行简单的相等细分，使得每个样本都归属于单个层总是会产生更好的结果。
This expression indicates that the variance obtained using stratified sampling is always smaller than the variance obtained by a pure Monte Carlo sampling scheme. As a consequence, there is no advantage in generating more than one sample within a single stratum, since a simple equal subdivision of the stratum such that each sample is attributed to a single stratum always yields a better result.

当预先知道所需的样本数量并且问题的维数相对较低（通常小于 20）时，分层抽样效果很好。随着维数的增加，所需的层数不能很好地扩展。
Stratified sampling works well when the number of samples required is known in advance and the dimensionality of the problem is relatively low (typically less than 20). The number of strata required does not scale well with an increase in the number of dimensions.

### 3.6.3 $N$-Rooks or Latin Hypercube Algorithm

$N$-rooks 算法通过将 $N$ 个样本均匀地分布在各层之间来解决这个问题。每个维度仍被细分为 $N$ 个子区间。但是，只需要 $N$ 个样本；这些样本的分布使得每个子区间中都有一个样本。
The $N$-rooks algorithm addresses this by distributing $N$ samples evenly among the strata. Each dimension is still subdivided into N subintervals. However, only $N$ samples are needed; these samples are distributed such that one sample lies in each subinterval.

首先将 $d$ 个 $1\sim N$ 随机重排，得到 $q_{ij},1\le i\le d,1\le j\le N$，随后第 $k$ 个采样的结果为：
$$
\left(\frac{q_{1k}-u_1}{N},\frac{q_{2k}-u_2}{N},\cdots\frac{q_{dk}-u_d}{N}\right)
$$
这样可以保证投影到每一个维度上的 $N$ 个小区间中，每个区间都只会有一个采样。

注意：若是 $1\sim N$ 随机重排，则需要减去 $u$；若是 $0\sim N-1$ 随机重排，则需要加上 $u$。

### 3.6.4 Combining Stratified Sampling and Importance Sampling

Vanilla NeRF 中采用的采样策略。

分层抽样可以很容易地与重要性抽样相结合：根据均匀概率分布计算的样本可以进行分层，然后使用逆 CDF 对这些分层样本进行变换。
Stratified sampling can easily be integrated with importance sampling: the samples computed from a uniform probability distribution can be stratified, and then these stratified samples are transformed using the inverse cumulative distribution function.

#### Example

有 $N$ 个数字，每个数字被采样到的概率是 $p[i]$，求 $n$ 次采样后的总和。

```python
import numpy as np
def stratified_importance_sampling(n: int):
    N_sum, P = 0, 0
    u = np.random.random()
    for i in range(n):
        P += p[i]
        N_i = int(P * n + u) - N_sum
        for j in range(N_i):
            sample(a[i]) # Sample i-th term N_i times.
        N_sum += N_i
```

### 3.6.5 Combining Estimators of Different Distributions

ReSTIR 中采用的采样策略。

#### Using Variance

考虑将 $\langle I_1\rangle$ 和 $\langle I_2\rangle$ 这两个估计器组合起来计算一个积分 $I$。显然，任何权重为 $w_1 + w_2 = 1$ 的线性组合 $w_1\langle I_1\rangle+w_2\langle I_2\rangle$ 也将是 $S$ 的估计器。不过，线性组合的方差取决于权重：
Consider combining two estimators, $\langle I_1\rangle$ and $\langle I_2\rangle$, to compute an integral $I$. Clearly, any linear combination $w_1\langle I_1\rangle+w_2\langle I_2\rangle$ with constant weights $w_1 + w_2 = 1$ will also be an estimator for $S$. The variance of the linear combination however depends on the weights,
$$
\begin{align*}
\mathrm{Var}[w_1\langle I_1\rangle+w_2\langle I_2\rangle]&=w_1^2\mathrm{Var}[\langle I_1\rangle]+w_2^2\mathrm{Var}[\langle I_2\rangle]+2w_1w_2\mathrm{Cov}[\langle I_1\rangle,\langle I_2\rangle]
\end{align*}
$$
如果 $\langle I_1\rangle$ 和 $\langle I_2\rangle$ 独立，则协方差为 0。求导得到，当
$$
\frac{w_1}{w_2}=\frac{\sigma^2[\langle I_2\rangle]-\mathrm{Cov}[\langle I_1\rangle,\langle I_2\rangle]}{\sigma^2[\langle I_1\rangle]-\mathrm{Cov}[\langle I_1\rangle,\langle I_2\rangle]}
$$
的时候，方差最小。

在实践中，权重可以用两种不同的方法计算：
In practice, the weights can be calculated in two different ways:

- 使用相关估计器方差的分析表达式。
    Using analytical expressions for the variance of the involved estimators.
- 根据实验样本本身对方差进行后验估计。这样做会产生轻微偏差。随着样本数量的增加，偏差也会消失：组合在渐近上是无偏或一致的。
    Using a posteriori estimates for the variances based on the samples in an experiment themselves. By doing so, a slight bias is introduced. As the number of samples is increased, the bias vanishes: the combination is asymptotically unbiased or consistent.

#### Multiple Importance Sampling

MIS 结合了不同的估计器，对每个单独的样本使用可能不同的权重，甚至对来自同一估计器的样本也是如此。
MIS combines different estimators using potentially different weights for each individual sample, even for samples from the same estimator.

平衡启发函数用来确定结合这些来自不同 PDF 的样本的权重，且权重之和为 1。
The balance heuristic is used to determine the weights that combine these samples from different PDFs provided the weights sum to 1.

平衡启发函数的结果是一个无偏的估计值，可以证明它的方差与 “最佳估计值” 的方差有一个相加的误差项。对于复杂的问题，这种策略是简单而稳健的。
The balance heuristic results in an unbiased estimator that provably has variance that differs from the variance of the "optimal estimator" by an additive error term. For complex problems, this strategy is simple and robust.

平衡启发函数：
$$
F=\sum_{i=1}^{n}\sum_{j=1}^{n_i}\frac{f(X_{i,j})}{\sum_{k}n_kp_k(X_{i,j})}
$$
从 PDF 为 $p_i$ 的采样中得到 $n_i$ 个值，记为 $X_{i,j},j\in\{1,\dots,n_i\}$。$N=\sum_in_i$。

```python
def balance_heuristic_mis(f: Callable, ni: list[int], pi: list[Callable]) -> float:
    F = 0
    N = sum(ni)
    for i in range(len(ni)):
        for j in range(ni[i]):
            X = sample(pi[i])
            d = sum(map(lambda n, p, X=X: n * p(X), ni, pi))
            F += f(X) / d
    return F
```

### 3.6.6 Control Variates

可以通过计算一个有解析积分的函数 $g$，并从被积的原始函数中减去这个函数，从而减小方差。
Variance could be reduced by computing a function $g$ that can be integrated analytically and subtracted from the original function to be integrated.

### 3.6.7 Quasi-Monte Carlo

准蒙特卡罗技术通过完全消除随机性来减少样本聚集的影响。样本尽可能均匀地确定性分布。准蒙特卡洛技术试图通过一种称为 “差异” 的度量来最小化结块。
Quasi–Monte Carlo techniques decrease the effects of clumping in samples by eliminating randomness completely. Samples are deterministically distributed as uniformly as possible. Quasi–Monte Carlo techniques try to minimize clumping with respect to a measure called the discrepancy.

我们考虑有 $N$ 个点的一组点集 $P$。考虑每个可能的、有一个角位于原点的 AABB 。给定一个大小为 $B_{\text{size}}$ 的方框，理想的点分布将有 $NB_{\text{size}}$ 个点。星形差异度量计算的是点分布 $P$ 与这一理想情况的偏差程度
We consider a set of points $P$ with size $N$. Consider each possible axis-aligned box with one corner at the origin. Given a box of size $B_{\text{size}}$, the ideal distribution of points would have $NB_{\text{size}}$ points. The star discrepancy measure computes how much the point distribution $P$ deviates from this ideal situation,
$$
D^*_N(P)=\sup_B\left|\frac{|P\cap B|}{N}-B_{\text{size}}\right|
$$
即 AABB 的大小比上整个空间的大小应该与 AABB 内的点的数量比上所有点的数量相近。

Koksma-Hlawka 不等式指出，估计器与待计算积分之间的差值满足条件：
The Koksma-Hlawka inequality states that the difference between the estimator and the integral to be computed satisifes the condition:
$$
\left|\langle I\rangle-I\right|\le V_{\text{HK}}(f(x))D^*
$$
其中 $V_{\text{HK}}$ 项是哈代和克劳斯意义上的函数 $f(x)$ 的变化。直观地说，$V_{\text{HK}}$ 衡量函数变化的速度。如果函数具有有界且连续的混合导数，那么它的变化就是有限的。
where the $V_{\text{HK}}$ term is the variation in the function $f(x)$ in the sense of Hardy and Krause. Intuitively, $V_{\text{HK}}$ measures how fast the function can change. If a function has bounded and continuous mixed derivatives, then its variation is finite.

从这个不等式中可以看出，MC 估计的误差与样本集的差异成正比。因此，人们花费了大量精力来设计低差异序列；这些序列被称为低差异序列（LDS）。
The important point to take from this inequality is that the error in the MC estimate is directly proportional to the discrepancy of the sample set. Therefore, much effort has been expended in designing sequences that have low discrepancy; these sequences are called low-discrepancy sequences (LDS).

Halton 序列以基数反函数为基础，计算方法如下。考虑一个用基数 $b$ 表示的数 $i$，其项为 $a_j$：
Halton sequences are based on the radical inverse function and are computed as follows. Consider a number $i$ which is expressed in base $b$ with the terms $a_j$:
$$
i=\sum_{j=0}^{\infty}a_j(i)b^j
$$
基数反函数 $\Phi$ 是通过反转小数点的位数得到的：
The radical inverse function $\Phi$ is obtained by reflecting the digits about the decimal point:
$$
\Phi_b(i)=\sum_{j=0}^{\infty}a_j(i)b^{-j-1}
$$
例如 $11=(1011)_2$，$\Phi_2(11)=(0.1101)_2=0.8125$。

对于任意基数 $b$，基逆序的差异为 $O((\log N)/N)$ 。为了得到一个 $d$ 维的低差异序列，需要在每个维度使用不同的基逆序列。因此，序列中的第 $i$ 个点为
The discrepancy of the radical inverse sequence is $O((\log N)/N)$ for any base $b$. To obtain a $d$-dimensional low-discrepancy sequence, a different radical-inverse sequence is used in each dimension. Therefore, the $i$th point in the sequence is given as
$$
x_i=(\Phi_{b_1}(i),\Phi_{b_2}(i),\dots,\Phi_{b_d}(i))
$$
其中基数 $b_j$ 互素。
where the bases $b_j$ are relatively prime.

Halton 序列的差异为 $O((\log N)^d/N)$ 。直观地说，Halton 序列均匀的原因可以解释如下： 这个序列在产生长度为 $m + 1$ 的字符串之前，先产生长度为 $m$ 的所有二进制字符串。所有大小为 $2^{-m}$ 的区间都会被访问，然后才会在同一区间中放入一个新点。
The Halton sequence has a discrepancy of $O((\log N)^d/N)$. Intuitively, the reason the Halton sequence is uniform can be explained as follows: This sequence produces all binary strings of length $m$ before producing strings of length $m + 1$. All intervals of size $2^{-m}$ will be visited before a new point is put in the same interval.

如果提前知道所需的样本数 $N$，则可以使用 Hammersley 序列来获得稍好的差异。Hammersley 序列的第 $i$ 个点为
When the number of samples required $N$ is known ahead of time, the Hammersley sequence could be used for a slightly better discrepancy. The $i$th point of the Hammersley sequence is
$$
x_i=(i/N,\Phi_{b_1}(i),\Phi_{b_2}(i),\dots,\Phi_{b_{d-1}}(i))
$$

#### Why Quasi–Monte Carlo?

与纯蒙特卡罗的 $1/\sqrt{N}$ 误差界限相比，这一界限可能具有巨大的潜在优势。低差异序列在低维度（约 10-20 维）时效果最佳；在高维度时，其性能与伪随机抽样类似。然而，与伪随机抽样相比，低差分序列具有高度相关性；例如，在 van der Corput 序列（基 2 的 Halton 序列）中，连续采样之间的差值有一半时间为 0.5。
This bound could have a substantial potential benefit compared to the $1/\sqrt{N}$ error bounds for pure Monte Carlo techniques. Low-discrepancy sequences work best for low dimensions (about 10-20); at higher dimensions, their performance is similar to pseudorandom sampling. However, as compared to pseudorandom sampling, low-discrepancy sequences are highly correlated; e.g., the difference between successive samples in the van der Corput sequence (a base-2 Halton sequence) is $0.5$ half of the time.
