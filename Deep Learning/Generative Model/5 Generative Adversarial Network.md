# Generative Adversarial Network

记号同 [Variational Auto-Encoder](variational%20auto-encoder.md)。

---

相比于 VAE 和 Diffusion，GAN 并没有建模数据的概率分布，而是直接生成了符合数据分布的样本。GAN 的基本思想是利用容易采样的先验分布 $p(\mathbf{z})$，通过一个可微的映射，将先验分布映射到数据分布 $p(\mathbf{x})$ 上。GAN 生成数据的方法完全抛弃了极大似然估计的框架，这带来了一个关键的问题，即如何保证网络产生的样本一定服从真实的数据分布。

因此，GAN 的关键在于找到一个有效的方法来确保生成的样本符合真实的数据分布。这就引入了GAN的 核心概念——对抗性训练。在对抗性训练中，有两个网络相互竞争：生成网络（$G(\mathbf{z};\theta)$）和判别网络（$D(\mathbf{x};\phi)$）。

生成器网络的任务是生成尽可能接近真实数据分布的样本，而判别器网络的任务是尽可能准确地区分生成的样本和真实的样本。通过这种方式，生成器网络在不断地尝试欺骗判别器网络，而判别器网络则在不断地提高其识别能力。这种对抗性的过程最终会导致生成器网络能够生成非常接近真实数据分布的样本。

## Discriminator

Discriminator $D(\mathbf{x};\phi)$ 接收一个样本 $\mathbf{x}$ 作为输入，对于来自真实数据分布 $p_{r}(\mathbf{x})$ 的样本，其标签为 $y=1$，而对于来自 Generator 生成数据分布 $p_{g}(\mathbf{x})$ 的样本，其标签为 $y=0$。Discriminator 输出一个标量，表示 $\mathbf{x}$ 来自真实数据的概率，即
$$
D(\mathbf{x};\phi)=p(y=1\mid\mathbf{x})
$$
因此，作为一个二分类任务，Discriminator 的损失函数可以使用交叉熵损失函数来表示：
$$
\begin{align*}
\phi^*&=\mathop{\arg\min}_{\phi}-\mathbb{E}_{\mathbf{x}}[y\log D(\mathbf{x};\phi)+(1-y)\log(1-D(\mathbf{x};\phi))]\\
&=\mathop{\arg\max}_{\phi}\mathbb{E}_{\mathbf{x}}[y\log D(\mathbf{x};\phi)+(1-y)\log(1-D(\mathbf{x};\phi))]
\end{align*}
$$

设来自真实数据分布 $p_{r}(\mathbf{x})$ 的样本和来自 Generator 生成数据分布 $p_{g}(\mathbf{x})$ 的样本的比例相当，并且考虑到 Generator，损失函数的形式变成了
$$
\begin{align*}
\phi^*&=\mathop{\arg\max}_{\phi}\mathbb{E}_{\mathbf{x}\sim p_{r}(\mathbf{x})}[\log D(\mathbf{x};\phi)]+\mathbb{E}_{\mathbf{x}\sim p_{g}(\mathbf{x})}[\log(1-D(\mathbf{x};\phi))]\\
&=\mathop{\arg\max}_{\phi}\mathbb{E}_{\mathbf{x}\sim p_{r}(\mathbf{x})}[\log D(\mathbf{x};\phi)]+\mathbb{E}_{\mathbf{x}\sim p_{z}(\mathbf{z})}[\log(1-D(G(\mathbf{z};\theta);\phi))]
\end{align*}
$$

## Generator

Generator $G(\mathbf{z};\theta)$ 接收一个随机向量 $\mathbf{z}$ 作为输入，输出一个样本 $\mathbf{x}$。Generator 的目标是生成尽可能接近真实数据分布的样本，让 Discriminator 无法区分生成的样本和真实的样本。因此，Generator 的损失函数可以表示为：
$$
\begin{align*}
\theta^*&=\mathop{\arg\max}_{\theta}\mathbb{E}_{\mathbf{z}\sim p_{z}(\mathbf{z})}[\log D(G(\mathbf{z};\theta);\phi)]\\
&=\mathop{\arg\min}_{\theta}\mathbb{E}_{\mathbf{z}\sim p_{z}(\mathbf{z})}[\log(1-D(G(\mathbf{z};\theta);\phi))]
\end{align*}
$$

Generator 的两个损失函数是等价的，但是在实际训练中，第一个形式的梯度性质更好。

## Cost Function

由于 Generator 和 Discriminator 的对抗性，GAN 的训练过程并不容易。如果 Discriminator 的性能太强了，Generator 就无法学习到任何有用的信息，而如果 Generator 的性能太强了，Discriminator 就无法区分生成的样本和真实的样本。最好的情况是 Discriminator 的性能要比 Generator 略强一些，这样 Generator 才能从 Discriminator 的错误中学习到有用的信息。

因此，GAN 的训练过程可以分为两个阶段：
1. 训练 Discriminator，固定 Generator 的参数，可能需要多次迭代；
2. 训练 Generator，固定 Discriminator 的参数，只需要一次迭代。

将二者的损失函数合并起来，可以得到 GAN 理论上用的损失函数
$$
\begin{align*}
&\quad\,\min_{\theta}\max_{\phi}\mathbb{E}_{\mathbf{x}\sim p_{r}(\mathbf{x})}[\log D(\mathbf{x};\phi)]+\mathbb{E}_{\mathbf{z}\sim p_{z}(\mathbf{z})}[\log(1-D(G(\mathbf{z};\theta);\phi))]\\
&=\min_{\theta}\max_{\phi}\mathbb{E}_{\mathbf{x}\sim p_{r}(\mathbf{x})}[\log D(\mathbf{x};\phi)]+\mathbb{E}_{\mathbf{x}\sim p_{g}(\mathbf{x})}[\log(1-D(\mathbf{x};\phi))]\\
\end{align*}
$$

最优的判别器为
$$
D^*(\mathbf{x})=\frac{p_{r}(\mathbf{x})}{p_{r}(\mathbf{x})+p_{g}(\mathbf{x})}
$$
带入损失函数，可以得到
$$
\begin{align*}
\mathcal{L}(G|D^*)&=\mathbb{E}_{\mathbf{x}\sim p_{r}(\mathbf{x})}\left[\log\frac{p_{r}(\mathbf{x})}{p_{r}(\mathbf{x})+p_{g}(\mathbf{x})}\right]+\mathbb{E}_{\mathbf{x}\sim p_{g}(\mathbf{x})}\left[\log\frac{p_{g}(\mathbf{x})}{p_{r}(\mathbf{x})+p_{g}(\mathbf{x})}\right]\\
&=\mathrm{KL}\left(p_{r}(\mathbf{x})\middle\|\frac{1}{2}[p_{r}(\mathbf{x})+p_{g}(\mathbf{x})]\right)+\mathrm{KL}\left(p_{g}(\mathbf{x})\middle\|\frac{1}{2}[p_{r}(\mathbf{x})+p_{g}(\mathbf{x})]\right)-2\log 2
\end{align*}
$$
当 Generator 最优时，即 $p_{g}(\mathbf{x})=p_{r}(\mathbf{x})$ 时，损失函数最小，此时对应的损失函数值为 $-2\log 2$。
