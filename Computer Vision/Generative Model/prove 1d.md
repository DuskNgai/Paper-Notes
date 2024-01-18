# Prove of Property of $1$-Dimensional Gaussian Distribution

## Sum of Gaussian Distributions / Convolution of Gaussian Distribution Functions

$$
\begin{align*}
f_Y(y)&=\int_{-\infty}^{\infty}\frac{1}{\sqrt{2\pi\sigma_1^2}}\exp\left(-\frac{(x-\mu_1)^2}{2\sigma_1^2}\right)\frac{1}{\sqrt{2\pi\sigma_2^2}}\exp\left(-\frac{(y-x-\mu_2)^2}{2\sigma_2^2}\right)\mathrm{d}x\\
&=\int_{-\infty}^{\infty}\frac{1}{\sqrt{(2\pi)^2\sigma_1^2\sigma_2^2}}\exp\left(-\frac{\sigma_2^2(x-\mu_1)^2+\sigma_1^2(y-x-\mu_2)^2}{2\sigma_1^2\sigma_2^2}\right)\mathrm{d}x\\
&=\int_{-\infty}^{\infty}\frac{1}{\sqrt{(2\pi)^2\sigma_1^2\sigma_2^2}}\exp\left(-\frac{(\sigma_1^2+\sigma_2^2)x^2-2(\sigma_1^2y-\sigma_1^2\mu_2+\sigma_2^2\mu_1)x+(\sigma_1^2y^2-2\sigma_1^2\mu_2y+\sigma_1^2\mu_2^2+\sigma_2^2\mu_1^2)}{2\sigma_1^2\sigma_2^2}\right)\mathrm{d}x\\
&=\int_{-\infty}^{\infty}\frac{1}{\sqrt{(2\pi)^2\sigma_1^2\sigma_2^2}}\exp\left(-\frac{\left((\sigma_1^2+\sigma_2^2)x-(\sigma_1^2y-\sigma_1^2\mu_2+\sigma_2^2\mu_1)\right)^2}{2\sigma_1^2\sigma_2^2(\sigma_1^2+\sigma_2^2)}-\frac{(y-(\mu_1+\mu_2))^2}{2(\sigma_1^2+\sigma_2^2)}\right)\mathrm{d}x\\
&=\exp\left(-\frac{(y-(\mu_1+\mu_2))^2}{2(\sigma_1^2+\sigma_2^2)}\right)\int_{-\infty}^{\infty}\frac{1}{\sqrt{(2\pi)^2\sigma_1^2\sigma_2^2}}\exp\left(-\frac{\left((\sigma_1^2+\sigma_2^2)x-(\sigma_1^2y-\sigma_1^2\mu_2+\sigma_2^2\mu_1)\right)^2}{2\sigma_1^2\sigma_2^2(\sigma_1^2+\sigma_2^2)}\right)\mathrm{d}x\\
&=\exp\left(-\frac{(y-(\mu_1+\mu_2))^2}{2(\sigma_1^2+\sigma_2^2)}\right)\int_{-\infty}^{\infty}\frac{1}{\sqrt{(2\pi)^2\sigma_1^2\sigma_2^2}}\exp\left(-\frac{\left((\sigma_1^2+\sigma_2^2)x\right)^2}{2\sigma_1^2\sigma_2^2(\sigma_1^2+\sigma_2^2)}\right)\mathrm{d}x\\
&=\exp\left(-\frac{(y-(\mu_1+\mu_2))^2}{2(\sigma_1^2+\sigma_2^2)}\right)\frac{\sqrt{2\pi\frac{\sigma_1^2\sigma_2^2}{(\sigma_1^2+\sigma_2^2)}}}{\sqrt{(2\pi)^2\sigma_1^2\sigma_2^2}}\\
&=\frac{1}{\sqrt{2\pi\left(\sigma_1^2+\sigma_2^2\right)}}\exp\left(-\frac{(y-(\mu_1+\mu_2))^2}{2(\sigma_1^2+\sigma_2^2)}\right)
\end{align*}
$$
