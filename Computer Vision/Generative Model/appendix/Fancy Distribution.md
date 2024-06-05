# Property of Some Distribution

## Property of $n$-Dimensional Discrete Classification Distribution

### Kullback–Leibler Divergence of Discrete Classification Distribution

Let $X_1\sim P(x)$ and $X_2\sim Q(x)$, where $P,Q$ are $N$ class ($\{0,\dots,N-1\}$) discrete distribution, and $Q(x=k)=\frac{1}{N}$, then

$$
\begin{align*}
\mathrm{KL}[P\parallel Q]&=\mathbb{E}_{x\sim P(x)}[\log P(x)]-\mathbb{E}_{x\sim P(x)}[\log Q(x)]\\
&=\sum_{k=0}^{N-1}P(X=k)[\log P(X=k)-\log Q(X=k)]\\
&=\sum_{k=0}^{N-1}P(X=k)[\log P(X=k)+\log N]\\
&=\sum_{k=0}^{N-1}P(X=k)\log P(X=k)+N\log N
\end{align*}
$$

## Property of $1$-Dimensional Gaussian Distribution

$$
G\left(x;\mu,\sigma^2\right)=\frac{1}{\sqrt{2\pi\sigma^2}}\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$

Two Gaussian distribution random variables are independent if and only if they are uncorrelated.

See proof of some properties in [here](Prove%201D-Gaussian.md).

### Sum of Gaussian Distributions / Convolution of Gaussian Distribution Functions

Let $X_1\sim\mathcal{N}(\mu_1,\sigma_1^2)$ and $X_2\sim\mathcal{N}(\mu_2,\sigma_2^2)$, then
$$
Y=X_1+X_2\sim\mathcal{N}(\mu_1+\mu_2,\sigma_1^2+\sigma_2^2)
$$

### Linear Combination of Gaussian Distributions

Let $X_1\sim\mathcal{N}(\mu_1,\sigma_1^2)$ and $X_2\sim\mathcal{N}(\mu_2,\sigma_2^2)$, then
$$
Y=aX_1+bX_2\sim\mathcal{N}(a\mu_1+b\mu_2,a^2\sigma_1^2+b^2\sigma_2^2)
$$

### Central Limit Theorem

Let $X_1,X_2,\cdots,X_n$ be $n$ independent random variables with the same distribution, $\mathbb{E}[X_i]=\mu$ and $\mathrm{Var}[X_i]=\sigma^2$, then the distribution of $Y=\sum_{i=1}^nX_i$ tends to be a Gaussian distribution when $n$ tends to infinity, where
$$
Y\sim\mathcal{N}(n\mu,n\sigma^2)
$$
or
$$
\frac{Y-n\mu}{\sqrt{n}\sigma}\sim\mathcal{N}(0,1)
$$

## Property of $n$-Dimensional Gaussian Distribution

$$
G(\mathbf{x};\boldsymbol{\mu},\Sigma)=\frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T\Sigma^{-1}(\mathbf{x}-\boldsymbol{\mu})\right)
$$

A $n$-dimensional Gaussian distribution random variable $(X_1,X_2,\cdots,X_n)$ can be decomposed into $n$ independent $1$-dimensional Gaussian distribution random variables $X_i$.

See proof of some properties in [here](Prove%20nD-Gaussian.md).

### Expectation of the Logarithm Gaussian Distribution

Let $X_1\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)$ and $X_2\sim\mathcal{N}(\boldsymbol{\mu}_2,\Sigma_2)$, then
$$
\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\log\mathcal{N}(\boldsymbol{\mu}_2,\Sigma_2)\right]=-\frac{1}{2}\left(n\log(2\pi)+\log|\Sigma_2|+\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1\right)+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)\right)
$$

### Kullback–Leibler Divergence of Gaussian Distribution

Let $X_1\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)$ and $X_2\sim\mathcal{N}(\boldsymbol{\mu}_2,\Sigma_2)$, then
$$
\mathrm{KL}\left[\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)\parallel\mathcal{N}(\boldsymbol{\mu}_2,\Sigma_2)\right]=\frac{1}{2}\left(\log\frac{|\Sigma_2|}{|\Sigma_1|}+\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1\right)+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)-n\right)
$$

### Moment-generating Function of the Gaussian Distribution

The moment-generation function of $\mathcal{N}(\boldsymbol{\mu},\Sigma)$ is
$$
M(\mathbf{t};\mathbf{x})=\exp\left[\mathbf{t}^T\boldsymbol{\mu}+\frac{1}{2}\mathbf{t}^T\Sigma\mathbf{t}\right]
$$

### Affine Transform of Gaussian Distribution

Let $X\sim\mathcal{N}(\boldsymbol{\mu},\Sigma)$, $\mathcal{A}\in\mathbb{R}^{n}\mapsto\mathbb{R}^{m}$ is any linear map, and $\mathbf{b}$ is the translation vector, then $Y=\mathcal{A}X+\mathbf{b}$ is also multi-dimensional Gaussian distribution with mean $A\boldsymbol{\mu}+\mathbf{b}$ and covariance $\mathcal{A}\Sigma\mathcal{A}^T$.
