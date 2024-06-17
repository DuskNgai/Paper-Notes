# Prove Diffusion

在这里我们证明了 DDPM 的一部分结论。

## Add Noise in Single Step

$$
\begin{align*}
\mathbf{x}_{t}&=\sqrt{\alpha_{t}}\mathbf{x}_{t-1}+\sqrt{1-\alpha_{t}}\boldsymbol{\epsilon}_{t-1}\\
&=\sqrt{\alpha_{t}}\left(\sqrt{\alpha_{t-1}}\mathbf{x}_{t-2}+\sqrt{1-\alpha_{t-1}}\hat{\boldsymbol{\epsilon}}_{t-2}\right)+\sqrt{1-\alpha_{t}}\boldsymbol{\epsilon}_{t-1}\\
&=\sqrt{\alpha_{t}}\sqrt{\alpha_{t-1}}\mathbf{x}_{t-2}+\left(\sqrt{\alpha_{t}}\sqrt{1-\alpha_{t-1}}\hat{\boldsymbol{\epsilon}}_{t-2}+\sqrt{1-\alpha_{t}}\boldsymbol{\epsilon}_{t-1}\right)&&\text{Addition of two Gaussians}\\
&=\sqrt{\alpha_{t}\alpha_{t-1}}\mathbf{x}_{t-2}+\sqrt{\alpha_{t}(1-\alpha_{t-1})+(1-\alpha_{t})}\boldsymbol{\epsilon}_{t-2}\\
&=\sqrt{\alpha_{t}\alpha_{t-1}}\mathbf{x}_{t-2}+\sqrt{1-\alpha_{t}\alpha_{t-1}}\boldsymbol{\epsilon}_{t-2}\\
&=\dots\\
&=\sqrt{\alpha_{t}\alpha_{t-1}\dots\alpha_{1}}\mathbf{x}_{0}+\sqrt{1-\alpha_{t}\alpha_{t-1}\dots\alpha_{1}}\boldsymbol{\epsilon}_{0}\\
&=\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0}+\sqrt{1-\bar{\alpha}_{t}}\boldsymbol{\epsilon}_{0}\\
&\sim\mathcal{N}(\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0},(1-\bar{\alpha}_{t})\mathbf{I})
\end{align*}
$$
其中 $\hat{\boldsymbol{\epsilon}}_{t}$ 是原本在 $t$ 时刻加入的噪声，$\boldsymbol{\epsilon}_t$ 是在 $t$ 时刻等效的噪声。

## Decomposition of ELBO

由于前向过程是一个 Markov 链，所以：
$$
q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})=q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1},\mathbf{x}_{0})=\frac{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})}
$$
后一个等号是由 Bayes 公式得到的。等价的，可以得到：
$$
q(\mathbf{x}_{t-1:t}\mid\mathbf{x}_{0})=q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})=q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})q(\mathbf{x}_{t}\mid\mathbf{x}_{0})
$$
以下开始分解 ELBO：
$$
\begin{align*}
&\quad\ \mathrm{ELBO}(q,\mathbf{x};\theta)\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})\prod_{t=1}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{\prod_{t=1}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)\prod_{t=2}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})\prod_{t=2}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)\prod_{t=2}^{T}p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})\prod_{t=2}^{T}q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}\right]+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\prod_{t=2}^{T}\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}\right]+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\prod_{t=2}^{T}\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)\cancel{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0}})}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\cancel{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{\cancel{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}}\right]+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{\cancel{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}}{q(\mathbf{x}_{T}\mid\mathbf{x}_{0})}\right]+\sum_{t=2}^{T}\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)}{q(\mathbf{x}_{T}\mid\mathbf{x}_{0})}\right]+\sum_{t=2}^{T}\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)+\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})}{q(\mathbf{x}_{T}\mid\mathbf{x}_{0})}\right]+\sum_{t=2}^{T}\mathbb{E}_{q(\mathbf{x}_{1:T}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q({\color{red}\mathbf{x}_{1}}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)+\mathbb{E}_{q({\color{red}\mathbf{x}_{T}}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{T})}{q(\mathbf{x}_{T}\mid\mathbf{x}_{0})}\right]+\sum_{t=2}^{T}\mathbb{E}_{q({\color{red}\mathbf{x}_{t-1:t}}\mid\mathbf{x}_{0})}\log\left[\frac{p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta)}{q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})}\right]\\
&=\mathbb{E}_{q(\mathbf{x}_{1}\mid\mathbf{x}_{0})}\log p(\mathbf{x}_{0}\mid\mathbf{x}_{1};\theta)-\mathrm{KL}(q(\mathbf{x}_{T}\mid\mathbf{x}_{0})\parallel p(\mathbf{x}_{T}))-\sum_{t=2}^{T}\mathbb{E}_{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}[\mathrm{KL}(q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\parallel p(\mathbf{x}_{t-1}\mid\mathbf{x}_{t};\theta))]
\end{align*}
$$

## Denoising Matching Term

$$
\begin{align*}
&\quad\ q(\mathbf{x}_{t-1}\mid\mathbf{x}_{t},\mathbf{x}_{0})\\
&=\frac{q(\mathbf{x}_{t}\mid\mathbf{x}_{t-1})q(\mathbf{x}_{t-1}\mid\mathbf{x}_{0})}{q(\mathbf{x}_{t}\mid\mathbf{x}_{0})}\\
&=\frac{\mathcal{N}(\mathbf{x}_{t};\sqrt{\alpha_{t}}\mathbf{x}_{t-1},(1-\alpha_{t})\mathbf{I})\mathcal{N}(\mathbf{x}_{t-1};\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0},(1-\bar{\alpha}_{t-1})\mathbf{I})}{\mathcal{N}(\mathbf{x}_{t};\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0},(1-\bar{\alpha}_{t})\mathbf{I})}\\
&=C_1\exp\left(-\frac{1}{2}\left(\frac{\left(\mathbf{x}_{t}-\sqrt{\alpha_{t}}\mathbf{x}_{t-1}\right)^{2}}{1-\alpha_{t}}+\frac{\left(\mathbf{x}_{t-1}-\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}\right)^{2}}{1-\bar{\alpha}_{t-1}}-\frac{\left(\mathbf{x}_{t}-\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{0}\right)^{2}}{1-\bar{\alpha}_{t}}\right)\right)\\
&=C_1\exp\left(-\frac{1}{2}\left(\frac{\mathbf{x}_{t}^{2}-2\sqrt{\alpha_{t}}\mathbf{x}_{t}\mathbf{x}_{t-1}+\alpha_{t}\mathbf{x}_{t-1}^{2}}{1-\alpha_{t}}+\frac{\mathbf{x}_{t-1}^{2}-2\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{t-1}\mathbf{x}_{0}+\bar{\alpha}_{t-1}\mathbf{x}_{0}^{2}}{1-\bar{\alpha}_{t-1}}-\frac{\mathbf{x}_{t}^{2}-2\sqrt{\bar{\alpha}_{t}}\mathbf{x}_{t}\mathbf{x}_{0}+\bar{\alpha}_{t}\mathbf{x}_{0}^{2}}{1-\bar{\alpha}_{t}}\right)\right)\\
&=C_2\exp\left(-\frac{1}{2}\left(\frac{-2\sqrt{\alpha_{t}}\mathbf{x}_{t}\mathbf{x}_{t-1}+\alpha_{t}\mathbf{x}_{t-1}^{2}}{1-\alpha_{t}}+\frac{\mathbf{x}_{t-1}^{2}-2\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{t-1}\mathbf{x}_{0}}{1-\bar{\alpha}_{t-1}}\right)\right)\\
&=C_2\exp\left(-\frac{1}{2}\left(\left(\frac{\alpha_{t}}{1-\alpha_{t}}+\frac{1}{1-\bar{\alpha}_{t-1}}\right)\mathbf{x}_{t-1}^{2}-2\left(\frac{\sqrt{\alpha_{t}}\mathbf{x}_{t}}{1-\alpha_{t}}+\frac{\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_{t-1}}\right)\mathbf{x}_{t-1}\right)\right)\\
&=C_2\exp\left(-\frac{1}{2}\left(\frac{1-\bar{\alpha}_t}{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\mathbf{x}_{t-1}^{2}-2\left(\frac{\sqrt{\alpha_{t}}\mathbf{x}_{t}}{1-\alpha_{t}}+\frac{\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_{t-1}}\right)\mathbf{x}_{t-1}\right)\right)\\
&=C_2\exp\left(-\frac{1}{2}\frac{1-\bar{\alpha}_t}{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\left(\mathbf{x}_{t-1}^2-2\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t}\mathbf{x}_{t-1}\right)\right)\\
&=C_1\exp\left(-\frac{1}{2}\frac{1-\bar{\alpha}_t}{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\left(\mathbf{x}_{t-1}-\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t}\mathbf{x}_{t-1}\right)^2\right)\\
&=\mathcal{N}\left(\mathbf{x}_{t-1};\frac{(1-\bar{\alpha}_{t-1})\sqrt{\alpha_{t}}\mathbf{x}_{t}+(1-\alpha_{t})\sqrt{\bar{\alpha}_{t-1}}\mathbf{x}_{0}}{1-\bar{\alpha}_t},\frac{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}{1-\bar{\alpha}_t}I\right)
\end{align*}
$$
其中
$$
\begin{align*}
C_1&=\left(\frac{1-\bar{\alpha}_{t}}{2\pi(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\right)^{\frac{n}{2}}\\
C_2&=\left(\frac{1-\bar{\alpha}_{t}}{2\pi(1-\alpha_{t})(1-\bar{\alpha}_{t-1})}\right)^{\frac{n}{2}}\exp\left(-\frac{\left(\sqrt{\alpha_{t}}(1-\bar{\alpha}_{t-1})\mathbf{x}_{t}+\sqrt{\bar{\alpha}_{t-1}}(1-\alpha_{t})\mathbf{x}_{0}\right)^2}{2(1-\alpha_{t})(1-\bar{\alpha}_{t-1})(1-\bar{\alpha}_t)}\right)
\end{align*}
$$

## Matching Mean

$$
\begin{align*}
& \quad\ \ \mathrm{KL}[q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) \parallel p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)] \\
&= \mathrm{KL}[\mathcal{N}(\mathbf{x}_{t-1}; \boldsymbol{\mu}_{q}(t), \sigma_{q}^2(t) \mathbf{I}) \parallel \mathcal{N}(\mathbf{x}_{t-1}; \boldsymbol{\mu}(\mathbf{x}_t, t; \theta), \sigma_{q}^2(t) \mathbf{I})] \\
&= \frac{1}{2} \left[ \log \frac{|\sigma_{q}^2(t) \mathbf{I}|}{|\sigma_{q}^2(t) \mathbf{I}|} + \mathrm{trace}([\sigma_{q}^2(t) \mathbf{I}]^{-1} [\sigma_{q}^2(t) \mathbf{I}]) + [\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta)]^T [\sigma_{q}^2(t) \mathbf{I}]^{-1} [\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta)] - n \right] \\
&= \frac{1}{2} \left[ \log 1 + n + [\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta)]^T [\sigma_{q}^2(t) \mathbf{I}]^{-1} [\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta)] - n \right] \\
&= \frac{1}{2} [\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta)]^T [\sigma_{q}^2(t) \mathbf{I}]^{-1} [\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta)] \\
&= \frac{ \|\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta) \|^2}{2 \sigma_{q}^2(t)}
\end{align*}
$$

## Matching Clean Data

$$
\begin{align*}
&\quad\ \ \mathrm{KL}[q(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}, \mathbf{x}_{0}) \parallel p(\mathbf{x}_{t-1} \mid \mathbf{x}_{t}; \theta)] \\
&= \frac{ \|\boldsymbol{\mu}_{q}(t) - \boldsymbol{\mu}(\mathbf{x}_t, t; \theta) \|^2}{2 \sigma_{q}^2(t)} \\
&= \frac{1}{2 \sigma_{q}^2(t)} \frac{(1-\alpha_{t})^2 \bar{\alpha}_{t-1}}{(1-\bar{\alpha}_t)^2} \left\| \mathbf{x}_{0} - \mathbf{x}(\mathbf{x}_t, t; \theta) \right\|^2 \\
&= \frac{1}{2} \frac{1-\bar{\alpha}_t}{(1-\alpha_{t})(1-\bar{\alpha}_{t-1})} \frac{(1-\alpha_{t})^2 \bar{\alpha}_{t-1}}{(1-\bar{\alpha}_t)^2} \left\| \mathbf{x}_{0} - \mathbf{x}(\mathbf{x}_t, t; \theta) \right\|^2 \\
&= \frac{1}{2} \frac{(1-\alpha_{t}) \bar{\alpha}_{t-1}}{(1-\bar{\alpha}_{t-1})(1-\bar{\alpha}_t)} \left\| \mathbf{x}_{0} - \mathbf{x}(\mathbf{x}_t, t; \theta) \right\|^2 \\
&= \frac{1}{2} \frac{\bar{\alpha}_{t-1} - \bar{\alpha}_{t}}{(1-\bar{\alpha}_{t-1})(1-\bar{\alpha}_t)} \left\| \mathbf{x}_{0} - \mathbf{x}(\mathbf{x}_t, t; \theta) \right\|^2 \\
&= \frac{1}{2} \left( \frac{\bar{\alpha}_{t-1}}{1-\bar{\alpha}_{t-1}} - \frac{\bar{\alpha}_{t}}{1-\bar{\alpha}_t} \right) \left\| \mathbf{x}_{0} - \mathbf{x}(\mathbf{x}_t, t; \theta) \right\|^2
\end{align*}
$$

## Learning Noise


$$
\begin{align*}
\boldsymbol{\mu}_{q}(t) &= \frac{(1-\bar{\alpha}_{t-1}) \sqrt{\alpha_{t}} \mathbf{x}_{t} + (1-\alpha_{t}) \sqrt{\bar{\alpha}_{t-1}} \mathbf{x}_{0}}{1-\bar{\alpha}_t} \\
&= \frac{(1-\bar{\alpha}_{t-1}) \sqrt{\alpha_{t}} \mathbf{x}_{t} + (1-\alpha_{t}) \sqrt{\bar{\alpha}_{t-1}} \left(\frac{1}{\sqrt{\bar{\alpha}_{t}}} \mathbf{x}_{t} - \frac{\sqrt{1-\bar{\alpha}_{t}}}{\sqrt{\bar{\alpha}_{t}}} \boldsymbol{\epsilon}_{0}\right)}{1-\bar{\alpha}_t} \\
&= \frac{(1-\bar{\alpha}_{t-1}) \sqrt{\alpha_{t}} \mathbf{x}_{t} + (1-\alpha_{t}) \left(\frac{1}{\sqrt{\alpha}_{t}} \mathbf{x}_{t} - \frac{\sqrt{1-\bar{\alpha}_{t}}}{\sqrt{\alpha}_{t}} \boldsymbol{\epsilon}_{0}\right)}{1-\bar{\alpha}_t} \\
&= \frac{(1-\bar{\alpha}_{t-1}) \alpha_{t} \mathbf{x}_{t} + (1-\alpha_{t})(\mathbf{x}_{t} - \sqrt{1-\bar{\alpha}_{t}} \boldsymbol{\epsilon}_{0})}{(1-\bar{\alpha}_t) \sqrt{\alpha}_{t}} \\
&= \frac{(1-\bar{\alpha}_{t-1}) \alpha_{t} \mathbf{x}_{t} + (1-\alpha_{t}) \mathbf{x}_{t} - (1-\alpha_{t}) \sqrt{1-\bar{\alpha}_{t}} \boldsymbol{\epsilon}_{0}}{(1-\bar{\alpha}_t) \sqrt{\alpha}_{t}} \\
&= \frac{(1-\bar{\alpha}_{t}) \mathbf{x}_{t} - (1-\alpha_{t}) \sqrt{1-\bar{\alpha}_{t}} \boldsymbol{\epsilon}_{0}}{(1-\bar{\alpha}_t) \sqrt{\alpha}_{t}} \\
&= \frac{1}{\sqrt{\alpha}_{t}} \mathbf{x}_{t} - \frac{(1-\alpha_{t})}{\sqrt{1-\bar{\alpha}_{t}} \sqrt{\alpha}_{t}} \boldsymbol{\epsilon}_{0}
\end{align*}
$$
