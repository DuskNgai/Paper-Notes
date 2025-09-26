# Chapter 4 Further Properties of Stochastic Processes

## 4.1 Sample Paths

随机过程的每一条具体实现（realization），我们称之为一条样本路径 (Sample Path)。实际上，由于 $\mathrm{d}w$ 是无限小的量，我们无法绘制出无限精确的样本路径，但我们可以探讨它的性质。Wiener 过程的样本路径是处处连续，但又处处不可微的。“连续”是因为 Wiener 过程没有跳跃。“不可微”则是因为 $\mathrm{d}w$ 的剧烈波动。我们可以尝试用导数的定义来计算：
$$
\frac{\mathrm{d}w}{\mathrm{d}t} = \lim_{h \to 0} \frac{w(t + h) - w(t)}{h} = \lim_{h \to 0} \frac{w(h)}{h}
$$
我们知道 $w(h)$ 是一个服从 $\mathcal{N}(0, h)$ 的随机变量，其标准差为 $\sqrt{h}$。所以，$w(h) / h$ 的量级是 $1 / \sqrt{h}$。当 $h \to 0$ 时，这个比值会发散，因此导数不存在。

Ito 规则 $(\mathrm{d}w)^2 = \mathrm{d}t$ 已经暗示了这一点：$\mathrm{d}w$ 的量级是 $\sqrt{\mathrm{d}t}$，当 $\mathrm{d}t \to 0$ 时，$\sqrt{\mathrm{d}t} \gg \mathrm{d}t$，它们的比值自然是无穷大。这种剧烈的、在任意小尺度上都存在的随机抖动，正是 Wiener 过程不可微的根源。

## 4.2 The Reflection Principle and the First-Passage Time

**首次穿越时间 (First-Passage Time)** 是一个经典问题：一个随机游走的粒子，需要多长时间才能第一次到达某个位置 $a$？我们记这个时间为 $T_{a}$，它本身也是一个随机变量。

为了求解 $T_{a}$ 的分布，我们需要 **反射原理 (Reflection Principle)**。其核心思想是利用 Wiener 过程的**对称性**。由于 Wiener 过程的增量 $\mathrm{d}w$ 是对称的（向上和向下走的概率相同），一条路径和它的“镜像”路径出现的概率是完全一样的。

考虑一条在 $T_{a}$ 时刻首次到达 $a$ 的路径 $w(t)$。我们可以构造一条新的“反射”路径 $\tilde{w}(t)$：
-   当 $t \le T_{a}$ 时，$\tilde{w}(t) = w(t)$ （两条路径重合）。
-   当 $t > T_{a}$ 时，$\tilde{w}(t) = 2a - w(t)$ （第二条路径是第一条路径关于水平线 $y = a$ 的镜像）。

按照反射原理，原始路径 $w(t)$ 和反射路径 $\tilde{w}(t)$ 出现的总概率是完全相等的。

---

设 $M(T) = \max_{0 \leq s \leq T} w(s)$ 是在时间 $[0, T]$ 内路径达到的最大值。现在我们来计算 $\mathbb{P}(M(T) \geq a)$。

任何一条在 $T$ 时刻终点值 $w(T) \ge a$ 的路径，它的最大值必然也大于等于 $a$。
对于任何一条在 $T$ 时刻终点值 $w(T) < a$ 但最大值 $M(T) \ge a$ 的路径，我们可以通过反射原理构造出一条与之等概率的、终点值 $\tilde{w}(T) > a$ 的路径。因此，所有最大值超过 $a$ 的路径，刚好是所有终点值超过 $a$ 的路径的两倍。因此：
$$
\begin{aligned}
\mathbb{P}(M(T) \geq a) &= 2 \mathbb{P}(w(T) \geq a) \\
&= \frac{2}{\sqrt{2 \pi T}} \int_{a}^{\infty} \exp\left(-\frac{x^{2}}{2T}\right) \mathrm{d}x
\end{aligned}
$$

首次穿越时间 $T_{a} \leq T$ 的事件，等价于在 $[0, T]$ 时间内的最大值 $M(T) \geq a$。所以，上式就是 $T_a$ 的累积分布函数。对其求导，便得到 $T_{a}$ 的概率密度函数 (PDF)：
$$
\mathbb{P}(T_{a}) = \frac{a}{\sqrt{2 \pi T^3}} \exp\left(-\frac{a^{2}}{2T}\right)
$$
一个有趣的结论是，这个分布的期望 $\mathbb{E}[T_{a}]$ 发散。这意味着虽然粒子几乎必然会到达 $a$，但它平均需要无穷长的时间。

## 4.3 The Stationary Auto-correlation Function, $g(\tau)$

对于一个平稳随机过程 $x(t)$，它的自相关函数 (Auto-correlation Function) 告诉了我们一个随机过程多久会“忘记”它的过去。我们定义平稳随机过程的相关系数为：
$$
C(x(t), x(t + \tau)) = \frac{\mathbb{E}[x(t) x(t + \tau)] - \mathbb{E}[x(t)] \mathbb{E}[x(t + \tau)]}{\sqrt{\mathbb{V}[x(t)] \mathbb{V}[x(t + \tau)]}}
$$
对于 Wiener 过程 $w(t)$，由于 $\mathbb{E}[w(t)] = 0$ 且 $\mathbb{V}(w(t)) = t$，我们有：
$$
C(w(t), w(t + \tau)) = \frac{\mathbb{E}[w(t) w(t + \tau)]}{\sqrt{t (t + \tau)}}
$$
其中，自相关项的计算如下 (假设 $\tau > 0$)：
$$
\begin{aligned}
\mathbb{E}[w(t) w(t + \tau)] &= \mathbb{E}[w(t) (w(t) + (w(t+\tau) - w(t)))] \\
&= \mathbb{E}[w(t)^2] + \mathbb{E}[w(t)(w(t+\tau) - w(t))] \\
&= \mathbb{V}[w(t)] + \mathbb{E}[w(t)] \mathbb{E}[w(t+\tau) - w(t)] \\
&= t + 0 \cdot 0 \\
&= t
\end{aligned}
$$
因此，Wiener 过程的相关系数为：
$$
C(w(t), w(t + \tau)) = \sqrt{\frac{t}{t + \tau}}
$$
这个结果表明，当 $\tau \to \infty$ 时，相关性会逐渐消失，过程会逐渐“忘记”过去。

因此，我们可以定义自相关函数 $g(t, \tau)$：
$$
g(t, \tau) = \mathbb{E}[x(t) x(t + \tau)]
$$
如果一个随机过程的均值不随时间变化，则 $g$ 只与 $\tau$ 有关，记为 $g(\tau)$。此时，$x$ 就是宽平稳随机过程。

## 4.4 Conditional Probability Density

只要一个随机过程在任意两个时刻 $t$ 和 $t'$ 的联合概率密度存在，我们就能计算其自相关函数。因此，我们来研究联合概率密度：
$$
\mathbb{P}(x(t) = x, x(t') = x') = \mathbb{P}(x(t) = x | x(t') = x') \mathbb{P}(x(t') = x')
$$
以标准 Wiener 过程为例 (假设 $t > t'$)：
$$
x(t) = x(t') + \int_{t'}^{t} \mathrm{d}w = x(t') + w(t - t')
$$
因此，$x(t)$ 和 $x(t')$ 的条件概率密度为：
$$
\mathbb{P}(x, t \mid x', t') = \frac{1}{\sqrt{2 \pi (t - t')}} \exp\left(-\frac{(x - x')^{2}}{2(t - t')}\right)
$$
而 $t'$ 时刻的概率密度为：
$$
\mathbb{P}(x', t') = \frac{1}{\sqrt{2 \pi t'}} \exp\left(-\frac{x'^{2}}{2t'}\right)
$$
将两者相乘，得到 $x(t)$ 和 $x(t')$ 的联合概率密度：
$$
\mathbb{P}(x, t, x', t') = \frac{1}{2 \pi \sqrt{t'(t - t')}} \exp\left(-\frac{(x - x')^{2}}{2(t - t')} - \frac{x'^{2}}{2t'}\right)
$$
利用这个联合密度函数进行积分，可以得到自相关函数：
$$
\mathbb{E}[x(t) x(t')] = \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} xx' \mathbb{P}(x, t, x', t') \mathrm{d}x \mathrm{d}x' = t' \quad (\text{其中 } t > t')
$$

## 4.5 The Power Spectrum

对于确定信号 $f$，
-   总能量定义为：
    $$
    E[f(t)] = \int_{-\infty}^{\infty} |f(t)|^{2} \mathrm{d}t = \int_{-\infty}^{\infty} |F(\nu)|^{2} \mathrm{d}\nu
    $$
-   自相关函数定义为：
    $$
    h(t, \tau) = \int_{-\infty}^{\infty} f(t) f(t + \tau) \mathrm{d}t
    $$

根据卷积定理，信号的能量谱密度 $|F(\nu)|^{2}$ 是其自相关函数的傅里叶变换：
$$
\begin{aligned}
|F(\nu)|^{2} &= F(\nu) F^*(\nu) \\
&= \left(\int_{-\infty}^{\infty} f(t) e^{-i 2 \pi \nu t} \mathrm{d}t\right) \left(\int_{-\infty}^{\infty} f(s) e^{i 2 \pi \nu s} \mathrm{d}s\right) \\
&= \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(t) f(s) e^{-i 2 \pi \nu (t-s)} \mathrm{d}s \mathrm{d}t && s = t - \tau \\
&= \int_{-\infty}^{\infty} \left(\int_{-\infty}^{\infty} f(t) f(t - \tau) \mathrm{d}t\right) e^{-i 2 \pi \nu \tau} \mathrm{d}\tau && t = t - \tau \\
&= \int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(t) f(t + \tau) \mathrm{d}t e^{-i 2 \pi \nu \tau} \mathrm{d}\tau \\
&= \int_{-\infty}^{\infty} g(\tau) e^{-i 2 \pi \nu \tau} \mathrm{d}\tau
\end{aligned}
$$

### 4.5.1 Signals with Finite Energy

对于随机信号 $x$，
-   总能量定义为：
    $$
    E[x(t)] = \int_{-\infty}^{\infty} \mathbb{E}[|x(t)|^{2}] \mathrm{d}t = \int_{-\infty}^{\infty} \mathbb{E}[|X(\nu)|^{2}] \mathrm{d}\nu
    $$
-   自相关函数定义为：
    $$
    g(t, \tau) = \mathbb{E}[x(t) x(t + \tau)]
    $$

### 4.5.2 Signals with Finite Power

对于确定信号 $f$，定义其平均功率为：
$$
P[f(t)] = \lim_{T \to \infty} \frac{1}{T} \int_{-T / 2}^{T / 2} f(t)^{2} \mathrm{d}t
$$
对于随机信号 $x$，定义其平均功率为：
$$
P[x(t)] = \lim_{T \to \infty} \frac{1}{T} \int_{-T / 2}^{T / 2} \mathbb{E}[x(t)^{2}] \mathrm{d}t
$$
其功率谱密度 $S(\nu)$ 描述了功率在频率上的分布，使得：
$$
P[x(t)] = \int_{-\infty}^{\infty} S(\nu) \mathrm{d}\nu
$$
**Wiener-Khinchin 定理** 指出，平稳随机过程的功率谱密度是平稳过程的自相关函数 $g(\tau) = \mathbb{E}[x(t)x(t + \tau)]$ 的 Fourier 变换：
$$
S(\nu) = \int_{-\infty}^{\infty} g(\tau) e^{-i 2 \pi \nu \tau} \mathrm{d}\tau
$$

## 4.6 White Noise

Wiener 过程不可导，但是其“导数”的自相关函数是存在的。我们用 $\xi(t)$ 表示 Wiener 过程的“导数”，然后探究一下它的自相关函数。因为 Wiener 过程是独立增量的，所以 $\xi(t)$ 和 $\xi(t + \tau)$ 的方向必然是无关的。因此，$\xi(t)$ 的自相关函数 $g(\tau)$ 在 $\tau > 0$ 的时候是 $0$。而在 $\tau = 0$ 的时候：
$$
\begin{aligned}
g(0) &= \mathbb{E}[\xi(t) \xi(t)] \\
&= \lim_{\Delta t \to 0} \mathbb{E}\left[\frac{\Delta w(t)}{\Delta t} \frac{\Delta w(t)}{\Delta t}\right] \\
&= \lim_{\Delta t \to 0} \mathbb{E}\left[\frac{\Delta w(t)^{2}}{\Delta t^{2}}\right] \\
&= \lim_{\Delta t \to 0} \frac{\mathbb{E}[\Delta w(t)^{2}]}{\Delta t^{2}} \\
&= \lim_{\Delta t \to 0} \frac{\Delta t}{\Delta t^{2}} = \infin
\end{aligned}
$$

因此，$\xi(t)$ 的自相关函数 $g(\tau) = \delta(\tau)$。其功率谱密度 $S(\nu) = 1$。


---

## Exercises

> 1. Show that
>    (1) If $f(t)$ is real then thr Fourier transform satisfies $F(\nu) = F^{*}(-\nu)$.
>    (2) If $F(\nu)$ is the Fourier transform of $f(t)$, then the Fourier transform of $f(at)$ is $F(\nu / a) / |a|$.
>    (3) If $F(\nu)$ is the Fourier transform of $f(t)$, and assuming that $f(t)$ is zero at $t = \pm \infty$, the Fourier transform of $\mathrm{d}^{n} f / \mathrm{d} t^{n}$ is $(2 \pi i \nu)^{n} F(\nu)$.

Pass

> 2. Show that
>    $$
>    \int_{-\infty}^{\infty} f(t)^{2} \mathrm{d}t = \int_{-\infty}^{\infty} |F(\nu)|^{2} \mathrm{d}\nu
>    $$
>    Hint: Use the fact that $\delta(t - \tau) = \int_{-\infty}^{\infty} \exp(-2 \pi i \nu (t - \tau)) \mathrm{d}\nu$.

Pass

> 3. Calculate the average power of the signal $f(t) = A \sin(\omega t)$.

$$
\begin{aligned}
P[f(t)] &= \lim_{T \to \infty} \frac{1}{T} \int_{-T / 2}^{T / 2} f(t)^{2} \mathrm{d}t \\
&= A^{2} \lim_{T \to \infty} \frac{1}{T} \int_{-T / 2}^{T / 2} \sin^{2}(\omega t) \mathrm{d}t \\
&= A^{2} \lim_{T \to \infty} \frac{1}{T} \int_{-T / 2}^{T / 2} \frac{1 - \cos(2 \omega t)}{2} \mathrm{d}t \\
&= \frac{1}{2}A^{2}
\end{aligned}
$$

> 4. Calculate the auto-correlation function of
>   $$
>   x(t) = x_{0} \cos(\omega t) + g \int_{0}^{t} \cos[\omega (t - s)] \mathrm{d} w(s)
>   $$
>   where $x_{0}$ is a random variable, independent of $w$, with mean zero and variance $V_{0}$.

> 5. Calculate the auto-correlation function of
>   $$
>   x(t) = \exp(-\beta w(t)) w(t)
>   $$

We have the independent increment $w(t + \tau) = w(t) + (w(t + \tau) - w(t)) = w(t) + w(\tau)$:

$$
\begin{aligned}
g(t, \tau) &= \mathbb{E}[x(t) x(t + \tau)] \\
&= \mathbb{E}[\exp(-\beta w(t)) w(t) \exp(-\beta w(t + \tau)) w(t + \tau)] \\
&= \mathbb{E}[\exp(-\beta w(t)) w(t) \exp(-\beta (w(t) + w(\tau))) (w(t) + w(\tau))] \\
&= \mathbb{E}[(w(t)^{2} + w(t)w(\tau)) \exp(-2 \beta w(t)) \exp(-\beta w(\tau))] \\
&= \mathbb{E}[w(t)^{2} \exp(-2 \beta w(t))] \mathbb{E}[\exp(-\beta w(\tau))] \\
&+ \mathbb{E}[w(t) \exp(-2 \beta w(t))] \mathbb{E}[w(\tau)\exp(-\beta w(\tau))] \\
\end{aligned}
$$

$$
\exp(-\beta w(t)) = \int_{-\infty}^{\infty} \frac{1}{\sqrt{2 \pi t}} \exp\left(-\frac{w^{2}}{2t} - \beta w\right) \mathrm{d}w = \exp\left(\frac{\beta^{2} t}{2}\right)
$$

$$
w(t) \exp(-\beta w(t)) = \int_{-\infty}^{\infty} \frac{1}{\sqrt{2 \pi t}} w \exp\left(-\frac{w^{2}}{2t} - \beta w\right) \mathrm{d}w = -\beta t \exp\left(\frac{\beta^{2} t}{2}\right)
$$

$$
w(t) \exp(-2 \beta w(t)) = \int_{-\infty}^{\infty} \frac{1}{\sqrt{2 \pi t}} w \exp\left(-\frac{w^{2}}{2t} - 2 \beta w\right) \mathrm{d}w = -2 \beta t \exp\left(2 \beta^{2} t\right)
$$

$$
w(t)^{2} \exp(-2 \beta w(t)) = \int_{-\infty}^{\infty} \frac{1}{\sqrt{2 \pi t}} w^{2} \exp\left(-\frac{w^{2}}{2t} - 2 \beta w\right) \mathrm{d}w = (t + 4 \beta^{2} t^{2}) \exp\left(2 \beta^{2} t\right)
$$

Therefore
$$
g(t, \tau) = (t + 2 \beta^{2}t\tau + 4 \beta^{2} t^{2}) \exp\left(2 \beta^{2} t\right) \exp\left(\frac{\beta^{2} \tau}{2}\right)
$$

> 6. 
>   (1) Calculate the auto-correlation function of the solution to
>   $$
>   \mathrm{d}x = -\gamma x \mathrm{d}t + g \mathrm{d}w
>   $$
>   (2) Show that in the limit as $t \to \infty$ the auto-correlation function is independent of the absolute time $t$. In this limit the process therefore reaches a “steady-state” in which it is wide-sense stationary.
>   (3) In this limit calculate the power spectral density of the process.

(1)

$$
x(t) = x(0) \exp(-\gamma t) + g \int_{0}^{t} \exp(-\gamma (t - s)) \mathrm{d}w(s)
$$

(2)

$$
\begin{aligned}
g(t, \tau) &= \mathbb{E}[x(t) x(t + \tau)] \\
&= \mathbb{E}\left[(x(0) \exp(-\gamma t) + g \int_{0}^{t} \exp(-\gamma (t - s)) \mathrm{d}w(s)) (x(0) \exp(-\gamma (t + \tau)) + g \int_{0}^{t + \tau} \exp(-\gamma (t + \tau - s)) \mathrm{d}w(s))\right] \\
&= \mathbb{E}\left[x(0)^{2} \exp(-\gamma(2t + \tau))\right] \\
&+ \mathbb{E}\left[x(0) \exp(-\gamma t) g \int_{0}^{t + \tau} \exp(-\gamma (t + \tau - s)) \mathrm{d}w(s)\right] \\
&+ \mathbb{E}\left[x(0) \exp(-\gamma (t + \tau)) g \int_{0}^{t} \exp(-\gamma (t - s)) \mathrm{d}w(s)\right] \\
&+ \mathbb{E}\left[g^{2} \int_{0}^{t} \exp(-\gamma (t - s)) \mathrm{d}w(s) \int_{0}^{t + \tau} \exp(-\gamma (t + \tau - s)) \mathrm{d}w(s)\right] \\
&= x(0)^{2} \exp(-\gamma(2t + \tau)) + \frac{g^{2}}{2 \gamma} \left(\exp(-\gamma t) - \exp(-\gamma (2t + \tau))\right) \\
\end{aligned}
$$

$$
\lim_{t \to \infty} g(t, \tau) = \frac{g^{2}}{2 \gamma} \exp(-\gamma \tau)
$$

(3)

$$
\begin{aligned}
S(\nu) &= \int_{-\infty}^{\infty} g(\tau) \exp(-i 2 \pi \nu \tau) \mathrm{d}\tau \\
&= \frac{g^{2}}{2 \gamma} \frac{2\gamma}{\gamma^{2} + \nu^{2}} \\
&= \frac{g^{2}}{\gamma^{2} + \nu^{2}}
\end{aligned}
$$

> 7. 
>   (1) Calculate the auto-correlation of the solution to
>   $$
>   \mathrm{d}x = -\frac{\beta^{2}}{2} x \mathrm{d}t + \beta x \mathrm{d}w
>   $$
>   (2) Show that in the limit as $t \to \infty$ the auto-correlation function is independent of the absolute time $t$. In this limit the process therefore reaches a “steady-state” in which it is wide-sense stationary.
>   (3) In this limit calculate the power spectral density of the process.

(1)

$$
x(t) = x(0) \exp\left(-\beta^{2} t + \beta w(t)\right)
$$

(2)

$$
\begin{aligned}
g(t, \tau) &= \mathbb{E}[x(t) x(t + \tau)] \\
&= \mathbb{E}\left[(x(0) \exp\left(-\beta^{2} t + \beta w(t)\right)) (x(0) \exp\left(-\beta^{2} (t + \tau) + \beta w(t + \tau)\right))\right] \\
&= \mathbb{E}\left[x(0)^{2} \exp\left(-2 \beta^{2} (t + \tau) + 2 \beta w(t) + 2 \beta w(t + \tau)\right)\right] \\
&= \mathbb{E}\left[x(0)^{2}\right] \exp\left(-\frac{\beta^{2}\tau}{2} \right) 
\end{aligned}
$$

$$
\lim_{t \to \infty} g(t, \tau) = \mathbb{E}\left[x(0)^{2}\right] \exp\left(-\frac{\beta^{2}\tau}{2} \right)
$$

(3)

$$
S(\nu) = \mathbb{E}\left[x(0)^{2}\right] \frac{4\beta^{2}}{\beta^{2} + 4\nu^{2}}
$$

> 8. Calculate the auto-correlation function of $x$ in the long-time limit, where
>    $$
>    \begin{aligned}
>    \mathrm{d}x &= (\omega y - \gamma x) \mathrm{d}t \\
>    \mathrm{d}y &= (-\omega x - \gamma y) \mathrm{d}t + g \mathrm{d}w
>    \end{aligned}
>    $$
