# Property of Some Distribution

## Property of $n$-Dimensional Discrete Classification Distribution

### Kullback–Leibler Divergence of Discrete Classification Distribution

Let $X_1\sim P(x)$ and $X_2\sim Q(x)$, where $P,Q$ are $N$ class ($\{0,\dots,N-1\}$) discrete distribution, and $Q(x=k)=\frac{1}{N}$

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


---



$$
V=\begin{bmatrix}a&b&c\\b&d&e\\c&e&f\end{bmatrix}
$$

$$
|V|=adf+2bce-ae^2-b^2f-c^2d
$$

$$
V^{-1}=\frac{1}{|V|}\begin{bmatrix}df-e^2&ce-bf&be-cd\\ce-bf&af-c^2&bc-ae\\be-cd&bc-ae&ad-b^2\end{bmatrix}:=\begin{bmatrix}A&B&C\\B&D&E\\C&E&F\end{bmatrix}=\Lambda
$$

$$
\begin{align*}
G^{3}_{V}&=\frac{1}{(2\pi)^{3/2}|V|^{1/2}}e^{-\frac{1}{2}\mathbf{x}^TV^{-1}\mathbf{x}}\\
&=\frac{|\Lambda|^{1/2}}{(2\pi)^{3/2}}\exp\left(-\frac{1}{2}\mathbf{x}^T\Lambda\mathbf{x}\right)
\end{align*}
$$

$$
\begin{align*}
\mathbf{x}^T\Lambda\mathbf{x}&=Ax^2+Dy^2+Fz^2+2Bxy+2Cxz+2Eyz\\
&=Ax^2+Dy^2+2Bxy+Fz^2+2Cxz+2Eyz
\end{align*}
$$

$$
\begin{align*}
\int_{-\infty}^{+\infty}G^{3}_{V}\ \mathrm{d}z&=\int_{-\infty}^{+\infty}\frac{|\Lambda|^{1/2}}{(2\pi)^{3/2}}\exp\left(-\frac{1}{2}\mathbf{x}^T\Lambda\mathbf{x}\right)\ \mathrm{d}z\\
&=\int_{-\infty}^{+\infty}\frac{|\Lambda|^{1/2}}{(2\pi)^{3/2}}\exp\left(-\frac{1}{2}(Ax^2+Dy^2+2Bxy+Fz^2+2Cxz+2Eyz)\right)\ \mathrm{d}z\\
&=\frac{|\Lambda|^{1/2}}{(2\pi)^{3/2}}\exp\left(-\frac{1}{2}(Ax^2+Dy^2+2Bxy)\right)\int_{-\infty}^{+\infty}\exp\left(-\frac{1}{2}(Fz^2+2Cxz+2Eyz)\right)\ \mathrm{d}z\\
&=\frac{|\Lambda|^{1/2}}{2\pi}\sqrt{\frac{1}{F}}\exp\left(-\frac{1}{2}(Ax^2+Dy^2+2Bxy)+\frac{(Cx+Ey)^2}{2F}\right)\\
&=\frac{1}{2\pi}\sqrt{\frac{|\Lambda|}{F}}\exp\left(-\frac{1}{2}\left(\left(A-\frac{C^2}{F}\right)x^2+\left(D-\frac{E^2}{F}\right)y^2+2\left(B-\frac{CE}{F}\right)xy\right)\right)\\
&=\frac{1}{2\pi}\sqrt{\frac{1}{ad-b^2}}\exp\left(-\frac{1}{2}\left(\frac{d}{ad-b^2}x^2+\frac{a}{ad-b^2}y^2+2\frac{b}{ad-b^2}xy\right)\right)\\
&=\frac{1}{2\pi\begin{vmatrix}a&b\\b&d\end{vmatrix}^{1/2}}\exp\left(-\frac{1}{2}\begin{bmatrix}x\\y\end{bmatrix}^T\begin{vmatrix}a&b\\b&d\end{vmatrix}^{-1}\begin{bmatrix}x\\y\end{bmatrix}\right)\\
\end{align*}
$$

$$
\begin{align*}
&\ \ \int_{-\infty}^{+\infty}\exp\left(-\frac{1}{2}(Fz^2+2Cxz+2Eyz)\right)\ \mathrm{d}z\\
&=\exp\left(\frac{(Cx+Ey)^2}{2F}\right)\int_{-\infty}^{+\infty}\exp\left(-\frac{1}{2}F\left(z+\frac{(Cx+Ey)}{F}\right)^2\right)\ \mathrm{d}z\\
&=\exp\left(\frac{(Cx+Ey)^2}{2F}\right)\sqrt{\frac{2\pi}{F}}
\end{align*}
$$

其中
$$
A-\frac{C^2}{F}=\frac{AF-C^2}{F}=\frac{(df-e^2)(ad-b^2)-(be-cd)^2}{|V|(ad-b^2)}=\frac{d}{ad-b^2}
$$

$$
D-\frac{E^2}{F}=\frac{DF-E^2}{F}=\frac{(af-c^2)(ad-b^2)-(bc-ae)^2}{|V|(ad-b^2)}=\frac{a}{ad-b^2}
$$

$$
B-\frac{CE}{F}=\frac{BF-CE}{F}=\frac{(ce-bf)(ad-b^2)-(bc-ae)(be-cd)}{|V|(ad-b^2)}=\frac{b}{ad-b^2}
$$

