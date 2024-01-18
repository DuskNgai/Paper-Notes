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
&=\mathrm{trace}\left(\Sigma_2^{-1}\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathbf{x}^T\mathbf{x}-\boldsymbol{\mu}_2^T\mathbf{x}-\mathbf{x}^T\boldsymbol{\mu}_2+\boldsymbol{\mu}_2^T\boldsymbol{\mu}_2\right]\right)&&\text{prove below}\\
&=\mathrm{trace}\left(\Sigma_2^{-1}\left(\mathbb{E}_{\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu}_1,\Sigma_1)}\left[\mathbf{x}^T\mathbf{x}\right]-\boldsymbol{\mu}_2^T\boldsymbol{\mu}_1-\boldsymbol{\mu}_1^T\boldsymbol{\mu}_2+\boldsymbol{\mu}_2^T\boldsymbol{\mu}_2\right)\right)\\
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

