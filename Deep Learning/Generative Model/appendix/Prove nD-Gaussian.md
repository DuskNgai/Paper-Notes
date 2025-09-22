# Prove of Property of $n$-Dimensional Gaussian Distribution

## Expectation of the Logarithm Gaussian Distribution

$$
\begin{align*}
&\quad\ \mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\log\mathcal{N}(\boldsymbol{\mu}_2,\Sigma_2)\right]\\
&=\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[-\frac{n}{2}\log(2\pi)-\frac{1}{2}\log|\Sigma_2|-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu}_2)^T\Sigma_2^{-1}(\mathbf{x}-\boldsymbol{\mu}_2)\right]\\
&=\frac{1}{2}\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[-n\log(2\pi)-\log|\Sigma_2|-(\mathbf{x}-\boldsymbol{\mu}_2)^T\Sigma_2^{-1}(\mathbf{x}-\boldsymbol{\mu}_2)\right]\\
&=\frac{1}{2}\left(-n\log(2\pi)-\log|\Sigma_2|-\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[(\mathbf{x}-\boldsymbol{\mu}_2)^T\Sigma_2^{-1}(\mathbf{x}-\boldsymbol{\mu}_2)\right]\right)&&\text{prove below}\\
&=\frac{1}{2}\left(-n\log(2\pi)-\log|\Sigma_2|-\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1\right)-\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)\right)\\
&=-\frac{1}{2}\left(n\log(2\pi)+\log|\Sigma_2|+\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1\right)+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)\right)
\end{align*}
$$

$$
\begin{align*}
&\quad\ \mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[(\mathbf{x}-\boldsymbol{\mu}_2)^T\Sigma_2^{-1}(\mathbf{x}-\boldsymbol{\mu}_2)\right]\\
&=\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathrm{trace}((\mathbf{x}-\boldsymbol{\mu}_2)^T\Sigma_2^{-1}(\mathbf{x}-\boldsymbol{\mu}_2))\right]&&a=\mathrm{trace}(a)\\
&=\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathrm{trace}(\Sigma_2^{-1}(\mathbf{x}-\boldsymbol{\mu}_2)^T(\mathbf{x}-\boldsymbol{\mu}_2))\right]&&\mathrm{trace}(AB)=\mathrm{trace}(BA)\\
&=\mathrm{trace}\left(\Sigma_2^{-1}\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[(\mathbf{x}-\boldsymbol{\mu}_2)^T(\mathbf{x}-\boldsymbol{\mu}_2)\right]\right)&&\text{trace is linear}\\
&=\mathrm{trace}\left(\Sigma_2^{-1}\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathbf{x}^T\mathbf{x}-\boldsymbol{\mu}_2^T\mathbf{x}-\mathbf{x}^T\boldsymbol{\mu}_2+\boldsymbol{\mu}_2^T\boldsymbol{\mu}_2\right]\right)\\
&=\mathrm{trace}\left(\Sigma_2^{-1}\left(\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathbf{x}^T\mathbf{x}\right]-\boldsymbol{\mu}_2^T\boldsymbol{\mu}_1-\boldsymbol{\mu}_1^T\boldsymbol{\mu}_2+\boldsymbol{\mu}_2^T\boldsymbol{\mu}_2\right)\right)&&\text{prove below}\\
&=\mathrm{trace}\left(\Sigma_2^{-1}\left(\Sigma_1+\boldsymbol{\mu}_1^T\boldsymbol{\mu}_1-\boldsymbol{\mu}_2^T\boldsymbol{\mu}_1-\boldsymbol{\mu}_1^T\boldsymbol{\mu}_2+\boldsymbol{\mu}_2^T\boldsymbol{\mu}_2\right)\right)\\
&=\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1+\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)\right)\\
&=\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)\right)\\
&=\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1\right)+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)
\end{align*}
$$

$$
\begin{align*}
\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathbf{x}^T\mathbf{x}\right]&=\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[(\mathbf{x}-\boldsymbol{\mu}_1)^T(\mathbf{x}-\boldsymbol{\mu}_1)\right]+\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathbf{x}\right]^T\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathbf{x}\right]\\
&=\Sigma_1+\boldsymbol{\mu}_1^T\boldsymbol{\mu}_1
\end{align*}
$$

## Kullback–Leibler Divergence of Gaussian Distribution

$$
\begin{align*}
&\quad\ \mathrm{KL}\left[\mathcal{N}(\boldsymbol{\mu}_1,\boldsymbol{\Sigma}_1)\parallel\mathcal{N}(\boldsymbol{\mu}_2,\boldsymbol{\Sigma}_2)\right]\\
&=\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\boldsymbol{\Sigma}_1)}\left[\log\mathcal{N}(\boldsymbol{\mu}_1,\boldsymbol{\Sigma}_1)\right]-\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\boldsymbol{\Sigma}_1)}\left[\log\mathcal{N}(\boldsymbol{\mu}_2,\boldsymbol{\Sigma}_2)\right]\\
&=-\frac{1}{2}\left(n\log(2\pi)+\log|\Sigma_1|+\mathrm{trace}\left(\Sigma_1^{-1}\Sigma_1\right)+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_1\right)^T\Sigma_1^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_1\right)\right)\\
&\quad\ +\frac{1}{2}\left(n\log(2\pi)+\log|\Sigma_2|+\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1\right)+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)\right)\\
&=-\frac{1}{2}\left(n\log(2\pi)+\log|\Sigma_1|+\mathrm{trace}\left(I_n\right)\right)\\
&\quad\ +\frac{1}{2}\left(n\log(2\pi)+\log|\Sigma_2|+\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1\right)+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)\right)\\
&=\frac{1}{2}\left(\log\frac{|\Sigma_2|}{|\Sigma_1|}+\mathrm{trace}\left(\Sigma_2^{-1}\Sigma_1\right)+\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)^T\Sigma_2^{-1}\left(\boldsymbol{\mu}_1-\boldsymbol{\mu}_2\right)-n\right)
\end{align*}
$$

## Kullback–Leibler Divergence of Gaussian Distribution with Standard Gaussian Distribution

$$
\mathrm{KL}\left[\mathcal{N}(\boldsymbol{\mu},\Sigma)\parallel\mathcal{N}(\mathbf{0},I_n)\right]=\frac{1}{2}\left(-\log|\Sigma|+\mathrm{trace}(\Sigma)+\boldsymbol{\mu}^T\boldsymbol{\mu}-n\right)
$$

## Moment-generating Function of the Gaussian Distribution

$$
\begin{align*}
M(\mathbf{t};\mathbf{x})&= \mathbb{E}[\exp(\mathbf{t}^T\mathbf{x})]\\
&= \int_{\mathbb{R}^n}\frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\exp(\mathbf{t}^T\mathbf{x})\exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T\Sigma^{-1}(\mathbf{x}-\boldsymbol{\mu})\right)\mathrm{d}\mathbf{x}\\
&= \int_{\mathbb{R}^n}\frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\exp\left(-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^T\Sigma^{-1}(\mathbf{x}-\boldsymbol{\mu})+\mathbf{t}^T\mathbf{x}\right)\mathrm{d}\mathbf{x}\\
&= \int_{\mathbb{R}^n}\frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\exp\left(-\frac{1}{2}\left[\mathbf{x}^T\Sigma^{-1}\mathbf{x}-2\boldsymbol{\mu}^T\Sigma^{-1}\mathbf{x}-\mathbf{x}^T\Sigma^{-1}\boldsymbol{\mu}+\boldsymbol{\mu}^T\Sigma^{-1}\boldsymbol{\mu}-2\mathbf{t}^T\mathbf{x}\right]\right)\mathrm{d}\mathbf{x}\\
&= \int_{\mathbb{R}^n}\frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\exp\left(-\frac{1}{2}\left[\mathbf{x}^T\Sigma^{-1}\mathbf{x}-2(\boldsymbol{\mu}+\Sigma\mathbf{t})^T\Sigma^{-1}\mathbf{x}+\boldsymbol{\mu}^T\Sigma^{-1}\boldsymbol{\mu}\right]\right)\mathrm{d}\mathbf{x}\\
&= \int_{\mathbb{R}^n}\frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\exp\left(-\frac{1}{2}\left[\mathbf{x}^T\Sigma^{-1}\mathbf{x}-2(\boldsymbol{\mu}+\Sigma\mathbf{t})^T\Sigma^{-1}\mathbf{x}+(\boldsymbol{\mu}+\Sigma\mathbf{t})^T\Sigma^{-1}(\boldsymbol{\mu}+\Sigma\mathbf{t})-2\mathbf{t}^T\boldsymbol{\mu}-\mathbf{t}^T\Sigma\mathbf{t}\right]\right)\mathrm{d}\mathbf{x}\\
&= \int_{\mathbb{R}^n}\frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\exp\left(-\frac{1}{2}[\mathbf{x}-(\boldsymbol{\mu}+\Sigma\mathbf{t})]^T\Sigma^{-1}[\mathbf{x}-(\boldsymbol{\mu}+\Sigma\mathbf{t})]+\mathbf{t}^T\boldsymbol{\mu}+\frac{1}{2}\mathbf{t}^T\Sigma\mathbf{t}\right)\mathrm{d}\mathbf{x}\\
&= \exp\left(\mathbf{t}^T\boldsymbol{\mu}+\frac{1}{2}\mathbf{t}^T\Sigma\mathbf{t}\right)\int_{\mathbb{R}^n}\frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\exp\left(-\frac{1}{2}[\mathbf{x}-(\boldsymbol{\mu}+\Sigma\mathbf{t})]^T\Sigma^{-1}[\mathbf{x}-(\boldsymbol{\mu}+\Sigma\mathbf{t})]\right)\mathrm{d}\mathbf{x}\\
&= \exp\left[\mathbf{t}^T\boldsymbol{\mu}+\frac{1}{2}\mathbf{t}^T\Sigma\mathbf{t}\right]
\end{align*}
$$

## Linear Transform of Gaussian Distribution

We prove by moment-generating function:
$$
\begin{align*}
M(\mathbf{t};\mathbf{y})&= \mathbb{E}[\exp(\mathbf{t}^T\mathbf{y})]\\
&= \mathbb{E}[\exp(\mathbf{t}^T(\mathcal{A}\mathbf{x}+\mathbf{b}))]\\
&= \mathbb{E}[\exp(\mathbf{t}^T\mathcal{A}\mathbf{x}+\mathbf{t}^T\mathbf{b})]\\
&= \mathbb{E}[\exp(\mathbf{t}^T\mathcal{A}\mathbf{x})\exp(\mathbf{t}^T\mathbf{b})]\\
&= M(\mathcal{A}^T\mathbf{t};\mathbf{x})\exp(\mathbf{t}^T\mathbf{b})\\
&= \exp\left[\left(\mathcal{A}^T\mathbf{t}\right)^T\boldsymbol{\mu}+\frac{1}{2}\left(\mathcal{A}^T\mathbf{t}\right)^T\Sigma\mathcal{A}^T\mathbf{t}\right]\exp(\mathbf{t}^T\mathbf{b})\\
&= \exp\left[\mathbf{t}^T\left(\mathcal{A}\boldsymbol{\mu}+\mathbf{b}\right)+\frac{1}{2}\mathbf{t}^T\left(\mathcal{A}\Sigma\mathcal{A}^T\right)\mathbf{t}\right]
\end{align*}
$$
Then $Y\sim\mathcal{N}(\mathcal{A}\boldsymbol{\mu}+\mathbf{b},\mathcal{A}\Sigma\mathcal{A}^T)$.
