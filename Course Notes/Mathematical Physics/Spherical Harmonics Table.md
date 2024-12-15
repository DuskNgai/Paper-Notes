# Spherical Harmonics Table

## Definition

归一化的球谐函数：
$$
Y_{l}^{m}(\theta,\phi)=\sqrt{\frac{(2l+1)}{4\pi}\frac{(l-m)!}{(l+m)!}}P_{l}^{m}(\cos\theta)\exp(im\phi)\\
l\in\{0,\dots,n\},m\in\{-l,\dots,l\},\theta\in[0,\pi],\phi\in[0,2\pi]
$$
其中：
$$
\begin{aligned}
P_{l}^{m}(x)&=\frac{(-1)^m}{2^ll!}(1-x^2)^{m/2}\frac{\mathrm{d}^{l+m}}{\mathrm{d}x^{l+m}}(x^2-1)^l\\
P_l(x)&=\frac{1}{2^ll!}\frac{\mathrm{d}^l}{\mathrm{d}x^l}(x^2-1)^l=\frac{1}{2^l}\sum_{k=0}^{\lfloor l/2\rfloor}(-1)^{k}\frac{(2l-2k)!}{k!(l-k)!(l-2k)!}x^{l-2k}
\end{aligned}
$$

## Associated Legendre Polynomial Table

一般使用如下递归形式生成 Legendre Polynomial：
$$
P_{n}(x)=\frac{(2n-1)xP_{n-1}(x)-(n-1)P_{n-2}(x)}{n}
$$

前 10 阶的 Associated Legendre Polynomial 如下：

**0**
$$
P_{0}^{ 0}(x)=1
$$
**1**
$$
\begin{align*}
P_{1}^{-1}(x)&=\frac{1}{2} \sqrt{1 - x^{2}}\\
P_{1}^{ 0}(x)&=x\\
P_{1}^{ 1}(x)&=- \sqrt{1 - x^{2}}\\
\end{align*}
$$
**2**
$$
\begin{align*}
P_{2}^{-2}(x)&=\frac{1}{8} \left(1 - x^{2}\right)\\
P_{2}^{-1}(x)&=\frac{1}{2}x \sqrt{1 - x^{2}}\\
P_{2}^{ 0}(x)&=\frac{1}{2} \left(3 x^{2} - 1\right)\\
P_{2}^{ 1}(x)&=- 3 x \sqrt{1 - x^{2}}\\
P_{2}^{ 2}(x)&=3 \left(1 - x^{2}\right)\\
\end{align*}
$$
**3**
$$
\begin{align*}
P_{3}^{-3}(x)&=\frac{1}{48} \left(1 - x^{2}\right)^{\frac{3}{2}}\\
P_{3}^{-2}(x)&=\frac{1}{8} x \left(1 - x^{2}\right)\\
P_{3}^{-1}(x)&=\frac{1}{8} \sqrt{1 - x^{2}} \left(5 x^{2} - 1\right)\\
P_{3}^{ 0}(x)&=\frac{1}{2} x \left(5 x^{2} - 3\right)\\
P_{3}^{ 1}(x)&=\frac{3}{2} \left(1 - 5 x^{2}\right) \sqrt{1 - x^{2}}\\
P_{3}^{ 2}(x)&=15 x \left(1 - x^{2}\right)\\
P_{3}^{ 3}(x)&=- 15 \left(1 - x^{2}\right)^{\frac{3}{2}}\\
\end{align*}
$$
**4**
$$
\begin{align*}
P_{4}^{-4}(x)&=\frac{1}{384} \left(1 - x^{2}\right)^{2}\\
P_{4}^{-3}(x)&=\frac{1}{48} x \left(1 - x^{2}\right)^{\frac{3}{2}}\\
P_{4}^{-2}(x)&=\frac{1}{48} \left(1 - x^{2}\right) \left(7 x^{2} - 1\right)\\
P_{4}^{-1}(x)&=\frac{1}{8} x \sqrt{1 - x^{2}} \left(7 x^{2} - 3\right)\\
P_{4}^{ 0}(x)&=\frac{1}{8} \left(35 x^{4} - 30 x^{2} + 3\right)\\
P_{4}^{ 1}(x)&=\frac{5}{2} x \sqrt{1 - x^{2}} \left(3 - 7 x^{2}\right)\\
P_{4}^{ 2}(x)&=\frac{15}{2} \left(1 - x^{2}\right) \left(7 x^{2} - 1\right)\\
P_{4}^{ 3}(x)&=- 105 x \left(1 - x^{2}\right)^{\frac{3}{2}}\\
P_{4}^{ 4}(x)&=105 \left(1 - x^{2}\right)^{2}\\
\end{align*}
$$
**5**
$$
\begin{align*}
P_{5}^{-5}(x)&=\frac{1}{3840} \left(1 - x^{2}\right)^{\frac{5}{2}}\\
P_{5}^{-4}(x)&=\frac{1}{384} x \left(1 - x^{2}\right)^{2}\\
P_{5}^{-3}(x)&=\frac{1}{384} \left(1 - x^{2}\right)^{\frac{3}{2}} \left(9 x^{2} - 1\right)\\
P_{5}^{-2}(x)&=\frac{1}{16} x \left(1 - x^{2}\right) \left(3 {x}^2 - 1\right)\\
P_{5}^{-1}(x)&=\frac{1}{16} \sqrt{1 - x^{2}} \left(21 x^{4} - 14 x^{2} + 1\right)\\
P_{5}^{ 0}(x)&=\frac{1}{8} x \left(63 x^{4} - 70 x^{2} + 15\right)\\
P_{5}^{ 1}(x)&=\frac{15}{8} \sqrt{1 - x^{2}} \left(- 21 x^{4} + 14 x^{2} - 1\right)\\
P_{5}^{ 2}(x)&=\frac{105}{2} x \left(1 - x^{2}\right) \left(3 {x}^2 - 1\right)\\
P_{5}^{ 3}(x)&=\frac{105}{2} \left(1 - 9 x^{2}\right) \left(1 - x^{2}\right)^{\frac{3}{2}}\\
P_{5}^{ 4}(x)&=945 x \left(1 - x^{2}\right)^{2}\\
P_{5}^{ 5}(x)&=- 945 \left(1 - x^{2}\right)^{\frac{5}{2}}\\
\end{align*}
$$
**6**
$$
\begin{align*}
P_{6}^{-6}(x)&=\frac{1}{46080} \left(1 - x^{2}\right)^{3}\\
P_{6}^{-5}(x)&=\frac{1}{3840} x \left(1 - x^{2}\right)^{\frac{5}{2}}\\
P_{6}^{-4}(x)&=\frac{1}{3840} \left(1 - x^{2}\right)^{2} \left(11 x^{2} - 1\right)\\
P_{6}^{-3}(x)&=\frac{1}{384} x \left(1 - x^{2}\right)^{\frac{3}{2}} \left(11 x^{2} - 3\right)\\
P_{6}^{-2}(x)&=\frac{1}{128} \left(1 - x^{2}\right) \left(33 x^{4} - 18 x^{2} + 1\right)\\
P_{6}^{-1}(x)&=\frac{1}{16} x \sqrt{1 - x^{2}} \left(33 x^{4} - 30 x^{2} + 5\right)\\
P_{6}^{ 0}(x)&=\frac{1}{16} \left(231 x^{6} - 315 x^{4} + 105 x^{2} - 5\right)\\
P_{6}^{ 1}(x)&=\frac{21}{8} x \sqrt{1 - x^{2}} \left(- 33 x^{4} + 30 x^{2} - 5\right)\\
P_{6}^{ 2}(x)&=\frac{105}{8} \left(1 - x^{2}\right) \left(33 x^{4} - 18 x^{2} + 1\right)\\
P_{6}^{ 3}(x)&=- \frac{315}{2} x \left(1 - x^{2}\right)^{\frac{3}{2}} \left(11 x^{2} - 3\right)\\
P_{6}^{ 4}(x)&=\frac{945}{2} \left(1 - x^{2}\right)^{2} \left(11 x^{2} - 1\right)\\
P_{6}^{ 5}(x)&=- 10395 x \left(1 - x^{2}\right)^{\frac{5}{2}}\\
P_{6}^{ 6}(x)&=10395 \left(1 - x^{2}\right)^{3}\\
\end{align*}
$$
**7**
$$
\begin{align*}
P_{7}^{-7}(x)&=\frac{1}{645120} \left(1 - x^{2}\right)^{\frac{7}{2}}\\
P_{7}^{-6}(x)&=\frac{1}{46080} x \left(1 - x^{2}\right)^{3}\\
P_{7}^{-5}(x)&=\frac{1}{46080} \left(1 - x^{2}\right)^{\frac{5}{2}} \left(13 x^{2} - 1\right)\\
P_{7}^{-4}(x)&=\frac{1}{3840} x \left(1 - x^{2}\right)^{2} \left(13 x^{2} - 3\right)\\
P_{7}^{-3}(x)&=\frac{1}{3840} \left(1 - x^{2}\right)^{\frac{3}{2}} \left(143 x^{4} - 66 x^{2} + 3\right)\\
P_{7}^{-2}(x)&=\frac{1}{384} x \left(1 - x^{2}\right) \left(143 x^{4} - 110 x^{2} + 15\right)\\
P_{7}^{-1}(x)&=\frac{1}{128} \sqrt{1 - x^{2}} \left(429 x^{6} - 495 x^{4} + 135 x^{2} - 5\right)\\
P_{7}^{ 0}(x)&=\frac{1}{16} x \left(429 x^{6} - 693 x^{4} + 315 x^{2} - 35\right)\\
P_{7}^{ 1}(x)&=- \frac{7}{16} \sqrt{1 - x^{2}} \left(429 x^{6} - 495 x^{4} + 135 x^{2} - 5\right)\\
P_{7}^{ 2}(x)&=\frac{63}{8} x \left(1 - x^{2}\right) \left(143 x^{4} - 110 x^{2} + 15\right)\\
P_{7}^{ 3}(x)&=\frac{315}{8} \left(1 - x^{2}\right)^{\frac{3}{2}} \left(143 x^{4} - 66 x^{2} + 3\right)\\
P_{7}^{ 4}(x)&=\frac{3465}{2} x \left(1 - x^{2}\right)^{2} \left(13 x^{2} - 3\right)\\
P_{7}^{ 5}(x)&=\frac{10395}{2} \left(1 - 13 x^{2}\right) \left(1 - x^{2}\right)^{\frac{5}{2}}\\
P_{7}^{ 6}(x)&=135135 x \left(1 - x^{2}\right)^{3}\\
P_{7}^{ 7}(x)&=- 135135 \left(1 - x^{2}\right)^{\frac{7}{2}}\\
\end{align*}
$$
**8** 
$$
\begin{align*}
P_{8}^{-8}(x)&=\frac{1}{10321920} \left(1 - x^{2}\right)^{4}\\
P_{8}^{-7}(x)&=\frac{1}{645120} x \left(1 - x^{2}\right)^{\frac{7}{2}}\\
P_{8}^{-6}(x)&=\frac{1}{645120} \left(15 x^{2} - 1\right) \left(1 - x^{2}\right)^{3}\\
P_{8}^{-5}(x)&=\frac{1}{15360} x \left(1 - x^{2}\right)^{\frac{5}{2}} \left(5 x^{2} - 1\right)\\
P_{8}^{-4}(x)&=\frac{1}{15360} \left(1 - x^{2}\right)^{2} \left(65 x^{4} - 26 x^{2} + 1\right)\\
P_{8}^{-3}(x)&=\frac{1}{768}x \left(1 - x^{2}\right)^{\frac{3}{2}} \left(39 x^{4} - 26 x^{2} + 3\right)\\
P_{8}^{-2}(x)&=\frac{1}{256} \left(1 - x^{2}\right) \left(143 x^{6} - 143 x^{4} + 33 x^{2} - 1\right)\\
P_{8}^{-1}(x)&=\frac{1}{128} x \sqrt{1 - x^{2}} \left(715 x^{6} - 1001 x^{4} + 385 x^{2} - 35\right)\\
P_{8}^{ 0}(x)&=\frac{1}{128} \left(6435 x^{8} - 12012 x^{6} + 6930 x^{4} - 1260 x^{2} + 35\right)\\
P_{8}^{ 1}(x)&=\frac{9}{16} x \sqrt{1 - x^{2}} \left(- 715 x^{6} + 1001 x^{4} - 385 x^{2} + 35\right)\\
P_{8}^{ 2}(x)&=\frac{315}{16} \left(1 - x^{2}\right) \left(143 x^{6} - 143 x^{4} + 33 x^{2} - 1\right)\\
P_{8}^{ 3}(x)&=\frac{3465}{8}x \left(1 - x^{2}\right)^{\frac{3}{2}} \left(39 x^{4} - 26 x^{2} + 3\right)\\
P_{8}^{ 4}&=\frac{10395}{8} \left(1 - x^{2}\right)^{2} \left(65 x^{4} - 26 x^{2} + 1\right)\\
P_{8}^{ 5}(x)&=- \frac{135135}{2} x \left(1 - x^{2}\right)^{\frac{5}{2}} \left(5 x^{2} - 1\right)\\
P_{8}^{ 6}(x)&=\frac{135135}{2} \left(15 x^{2} - 1\right) \left(1 - x^{2}\right)^{3}\\
P_{8}^{ 7}(x)&=- 2027025 x \left(1 - x^{2}\right)^{\frac{7}{2}}\\
P_{8}^{ 8}(x)&=2027025 \left(1 - x^{2}\right)^{4}\\
\end{align*}
$$
**9**
$$
\begin{align*}
P_{9}^{-9}&=\frac{1}{185794560} \left(1 - x^{2}\right)^{\frac{9}{2}}\\
P_{9}^{-8}&=\frac{1}{10321920} x \left(1 - x^{2}\right)^{4}\\
P_{9}^{-7}&=\frac{1}{10321920} \left(1 - x^{2}\right)^{\frac{7}{2}} \left(17 x^{2} - 1\right)\\
P_{9}^{-6}&=- \frac{1}{645120} x \left(1 - x^{2}\right)^{6} \left(17 x^{2} - 3\right)\\
P_{9}^{-5}&=\frac{1}{215040} \left(1 - x^{2}\right)^{\frac{5}{2}} \left(85 x^{4} - 30 x^{2} + 1\right)\\
P_{9}^{-4}&=\frac{1}{3072} x \left(1 - x^{2}\right)^{2} \left(17 x^{4} - 10 x^{2} + 1\right)\\
P_{9}^{-3}&=- \frac{1}{3072} \left(1 - x^{2}\right)^{\frac{3}{2}} \left(221 x^{6} - 195 x^{4} + 39 x^{2} - 1\right)\\
P_{9}^{-2}&=\frac{1}{256} x \left(1 - x^{2}\right) \left(221 x^{6} - 273 x^{4} + 91 x^{2} - 7\right)\\
P_{9}^{-1}&=\frac{1}{256} \sqrt{1 - x^{2}} \left(2431 x^{8} - 4004 x^{6} + 2002 x^{4} - 308 x^{2} + 7\right)\\
P_{9}^{ 0}&=\frac{1}{128} x \left(12155 x^{8} - 25740 x^{6} + 18018 x^{4} - 4620 x^{2} + 315\right)\\
P_{9}^{ 1}&=- \frac{45}{128} \sqrt{1 - x^{2}} \left(2431 x^{8} - 4004 x^{6} + 2002 x^{4} - 308 x^{2} + 7\right)\\
P_{9}^{ 2}&=\frac{495}{16} x \left(1 - x^{2}\right) \left(221 x^{6} - 273 x^{4} + 91 x^{2} - 7\right)\\
P_{9}^{ 3}&=- \frac{3465}{16} \left(1 - x^{2}\right)^{\frac{3}{2}} \left(221 x^{6} - 195 x^{4} + 39 x^{2} - 1\right)\\
P_{9}^{ 4}&=\frac{135135}{8} x \left(1 - x^{2}\right)^{2} \left(17 x^{4} - 10 x^{2} + 1\right)\\
P_{9}^{ 5}&=- \frac{135135}{8} \left(1 - x^{2}\right)^{\frac{5}{2}} \left(85 x^{4} - 30 x^{2} + 1\right)\\ 
P_{9}^{ 6}&=\frac{675675}{2} x \left(1- x^{2}\right)^{6} \left(17 x^{2} - 3\right)\\
P_{9}^{ 7}&=- \frac{2027025}{2} \left(1 - x^{2}\right)^{\frac{7}{2}} \left(17 x^{2} - 1\right)\\
P_{9}^{ 8}&=34459425 x \left(1 - x^{2}\right)^{4}\\
P_{9}^{ 9}&=- 34459425 \left(1 - x^{2}\right)^{\frac{9}{2}}\\
\end{align*}
$$

## Spherical Harmonics

**0**
$$
Y_{0}^{ 0}(\theta, \phi)=\frac{1}{2 \sqrt{\pi}}
$$
**1**
$$
\begin{align*}
Y_{1}^{-1}(\theta, \phi)&=\frac{\sqrt{6}}{4 \sqrt{\pi}} \sin\theta e^{- i \phi}\\
Y_{1}^{ 0}(\theta, \phi)&=\frac{\sqrt{3}}{2 \sqrt{\pi}} \cos\theta\\
Y_{1}^{ 1}(\theta, \phi)&=- \frac{\sqrt{6}}{4 \sqrt{\pi}} \sin\theta e^{i \phi}\\
\end{align*}
$$
**2**
$$
\begin{align*}
Y_{2}^{-2}(\theta, \phi)&=\frac{\sqrt{30}}{8 \sqrt{\pi}} \sin^{2}\theta e^{- 2 i \phi}\\
Y_{2}^{-1}(\theta, \phi)&=\frac{\sqrt{30}}{4 \sqrt{\pi}} \sin\theta \cos\theta e^{- i \phi}\\
Y_{2}^{ 0}(\theta, \phi)&=\frac{\sqrt{5}}{4 \sqrt{\pi}} \left(3 \cos^{2}\theta - 1\right)\\
Y_{2}^{ 1}(\theta, \phi)&=- \frac{\sqrt{30}}{4 \sqrt{\pi}} \sin\theta \cos\theta e^{i \phi}\\
Y_{2}^{ 2}(\theta, \phi)&=\frac{\sqrt{30}}{8 \sqrt{\pi}} \sin^{2}\theta e^{2 i \phi}\\
\end{align*}
$$
**3**
$$
\begin{align*}
Y_{3}^{-3}(\theta, \phi)&=\frac{\sqrt{35}}{8 \sqrt{\pi}} \sin^{3}\theta e^{- 3 i \phi}\\
Y_{3}^{-2}(\theta, \phi)&=\frac{\sqrt{210}}{8 \sqrt{\pi}} \sin^{2}\theta \cos\theta e^{- 2 i \phi}\\
Y_{3}^{-1}(\theta, \phi)&=\frac{\sqrt{21}}{8 \sqrt{\pi}} \sin\theta \left(5 \cos^{2}\theta - 1\right) e^{- i \phi}\\
Y_{3}^{ 0}(\theta, \phi)&=\frac{\sqrt{7}}{4 \sqrt{\pi}} \left(5 \cos^{2}\theta - 3\right) \cos\theta\\
Y_{3}^{ 1}(\theta, \phi)&=\frac{\sqrt{21}}{8 \sqrt{\pi}} \left(1 - 5 \cos^{2}\theta\right) \sin\theta e^{i \phi}\\
Y_{3}^{ 2}(\theta, \phi)&=\frac{\sqrt{210}}{8 \sqrt{\pi}} \sin^{2}\theta \cos\theta e^{2 i \phi}\\
Y_{3}^{ 3}(\theta, \phi)&=- \frac{\sqrt{35}}{8 \sqrt{\pi}} \sin^{3}\theta e^{3 i \phi}\\
\end{align*}
$$
**4**
$$
\begin{align*}
Y_{4}^{-4}(\theta, \phi)&=\frac{3 \sqrt{70}}{32 \sqrt{\pi}} \sin^{4}\theta e^{- 4 i \phi}\\
Y_{4}^{-3}(\theta, \phi)&=\frac{3 \sqrt{35}}{8 \sqrt{\pi}} \sin^{3}\theta \cos\theta e^{- 3 i \phi}\\
Y_{4}^{-2}(\theta, \phi)&=\frac{3 \sqrt{10}}{16 \sqrt{\pi}} \sin^2\theta \left(7 \cos^{2}\theta - 1\right) e^{- 2 i \phi}\\
Y_{4}^{-1}(\theta, \phi)&=\frac{3 \sqrt{5}}{8 \sqrt{\pi}} \sin\theta \left(7 \cos^{2}\theta - 3\right) \cos\theta e^{- i \phi}\\
Y_{4}^{ 0}(\theta, \phi)&=\frac{3}{16 \sqrt{\pi}} \left(35 \cos^{4}\theta - 30 \cos^{2}\theta + 3\right)\\
Y_{4}^{ 1}(\theta, \phi)&=\frac{3 \sqrt{5}}{8 \sqrt{\pi}} \sin\theta \left(3 - 7 \cos^{2}\theta\right) \cos\theta e^{i \phi}\\
Y_{4}^{ 2}(\theta, \phi)&=\frac{3 \sqrt{10}}{16 \sqrt{\pi}} \sin^2\theta \left(7 \cos^{2}\theta - 1\right) e^{2 i \phi}\\
Y_{4}^{ 3}(\theta, \phi)&=- \frac{3 \sqrt{35}}{8 \sqrt{\pi}} \sin^{3}\theta \cos\theta e^{3 i \phi}\\
Y_{4}^{ 4}(\theta, \phi)&=\frac{3 \sqrt{70}}{32 \sqrt{\pi}} \sin^{4}\theta e^{4 i \phi}\\
\end{align*}
$$
**5**
$$
\begin{align*}
Y_{5}^{-5}(\theta, \phi)&=\frac{3 \sqrt{77}}{32 \sqrt{\pi}} \sin^{5}\theta e^{- 5 i \phi}\\
Y_{5}^{-4}(\theta, \phi)&=\frac{3 \sqrt{770}}{32 \sqrt{\pi}} \sin^{4}\theta \cos\theta e^{- 4 i \phi}\\
Y_{5}^{-3}(\theta, \phi)&=\frac{\sqrt{385}}{32 \sqrt{\pi}} \sin^{3}\theta \left(9 \cos^{2}\theta - 1\right) e^{- 3 i \phi}\\
Y_{5}^{-2}(\theta, \phi)&=\frac{\sqrt{2310}}{16 \sqrt{\pi}} \sin^{2}\theta \left(3 \cos^{2}\theta - 1\right)\cos\theta e^{- 2 i \phi}\\
Y_{5}^{-1}(\theta, \phi)&=\frac{\sqrt{330}}{32 \sqrt{\pi}} \sin\theta \left(21 \cos^{4}\theta - 14 \cos^{2}\theta + 1\right) e^{- i \phi}\\
Y_{5}^{ 0}(\theta, \phi)&=\frac{\sqrt{11}}{16 \sqrt{\pi}} \left(63 \cos^{4}\theta - 70 \cos^{2}\theta + 15\right) \cos\theta\\
Y_{5}^{ 1}(\theta, \phi)&=\frac{\sqrt{330}}{32 \sqrt{\pi}} \sin\theta \left(- 21 \cos^{4}\theta + 14 \cos^{2}\theta - 1\right) e^{i \phi}\\
Y_{5}^{ 2}(\theta, \phi)&=\frac{\sqrt{2310}}{16 \sqrt{\pi}} \sin^{2}\theta \left(3 \cos^{2}\theta - 1\right)\cos\theta e^{2 i \phi}\\
Y_{5}^{ 3}(\theta, \phi)&=\frac{\sqrt{385}}{32 \sqrt{\pi}} \sin^{3}\theta \left(1 - 9 \cos^{2}\theta\right) e^{3 i \phi}\\
Y_{5}^{ 4}(\theta, \phi)&=\frac{3 \sqrt{770}}{32 \sqrt{\pi}} \sin^{4}\theta \cos\theta e^{4 i \phi}\\
Y_{5}^{ 5}(\theta, \phi)&=- \frac{3 \sqrt{77}}{32 \sqrt{\pi}} \sin^{5}\theta e^{5 i \phi}\\
\end{align*}
$$
**6**
$$
\begin{align*}
Y_{6}^{-6}(\theta, \phi)&=\frac{\sqrt{3003}}{64 \sqrt{\pi}} \sin^{6}\theta e^{- 6 i \phi}\\
Y_{6}^{-5}(\theta, \phi)&=\frac{3 \sqrt{1001}}{32 \sqrt{\pi}} \sin^{5}\theta \cos\theta e^{- 5 i \phi}\\
Y_{6}^{-4}(\theta, \phi)&=\frac{3 \sqrt{182}}{64 \sqrt{\pi}} \sin^{4}\theta \left(11 \cos^{2}\theta - 1\right) e^{- 4 i \phi}\\
Y_{6}^{-3}(\theta, \phi)&=\frac{\sqrt{1365} }{32 \sqrt{\pi}}\sin^{3}\theta \left(11 \cos^{2}\theta - 3\right) \cos\theta e^{- 3 i \phi}\\
Y_{6}^{-2}(\theta, \phi)&=\frac{\sqrt{1365}}{64 \sqrt{\pi}} \sin^{2}\theta \left(33 \cos^{4}\theta - 18\cos^{2}\theta + 1\right) e^{- 2 i \phi}\\
Y_{6}^{-1}(\theta, \phi)&=\frac{\sqrt{546}}{32 \sqrt{\pi}} \sin\theta \left(33 \cos^{4}\theta - 30 \cos^{2}\theta + 5\right) \cos\theta e^{- i \phi}\\
Y_{6}^{ 0}(\theta, \phi)&=\frac{\sqrt{13}}{32 \sqrt{\pi}} \left(231 \cos^{6}\theta - 315 \cos^{4}\theta + 105 \cos^{2}\theta - 5\right)\\
Y_{6}^{ 1}(\theta, \phi)&=\frac{\sqrt{546}}{32 \sqrt{\pi}} \sin\theta \left(- 33 \cos^{4}\theta + 30 \cos^{2}\theta - 5\right) \cos\theta e^{i \phi}\\
Y_{6}^{ 2}(\theta, \phi)&=\frac{\sqrt{1365}}{64 \sqrt{\pi}} \sin^{2}\theta \left(33 \cos^{4}\theta - 18\cos^{2}\theta + 1\right) e^{2 i \phi}\\
Y_{6}^{ 3}(\theta, \phi)&=- \frac{\sqrt{1365}}{32 \sqrt{\pi}} \sin^{3}\theta \left(11 \cos^{2}\theta - 3\right) \cos\theta e^{3 i \phi}\\
Y_{6}^{ 4}(\theta, \phi)&=\frac{3 \sqrt{182}}{64 \sqrt{\pi}} \sin^{4}\theta \left(11 \cos^{2}\theta - 1\right) e^{4 i \phi}\\
Y_{6}^{ 5}(\theta, \phi)&=- \frac{3 \sqrt{1001}}{32 \sqrt{\pi}} \sin^{5}\theta \cos\theta e^{5 i \phi}\\
Y_{6}^{ 6}(\theta, \phi)&=\frac{\sqrt{3003}}{64 \sqrt{\pi}} \sin^{6}\theta e^{6 i \phi}\\
\end{align*}
$$
**7**
$$
\begin{align*}
Y_{7}^{-7}(\theta, \phi)&=\frac{3 \sqrt{1430} }{128 \sqrt{\pi}}\sin^{7}\theta e^{- 7 i \phi}\\
Y_{7}^{-6}(\theta, \phi)&=\frac{3 \sqrt{5005}}{64 \sqrt{\pi}} \sin^{6}\theta \cos\theta e^{- 6 i \phi}\\
Y_{7}^{-5}(\theta, \phi)&=\frac{3 \sqrt{770}}{128 \sqrt{\pi}} \sin^{5}\theta \left(13 \cos^{2}\theta - 1\right) e^{- 5 i \phi}\\
Y_{7}^{-4}(\theta, \phi)&=\frac{3 \sqrt{770}}{64 \sqrt{\pi}} \sin^{4}\theta \left(13 \cos^{2}\theta - 3\right) \cos\theta e^{- 4 i \phi}\\
Y_{7}^{-3}(\theta, \phi)&=\frac{3 \sqrt{70}}{128 \sqrt{\pi}} \sin^{3}\theta \left(143 \cos^{4}\theta - 66 \cos^{2}\theta +  3\right) e^{- 3 i \phi}\\
Y_{7}^{-2}(\theta, \phi)&=\frac{3 \sqrt{35}}{64 \sqrt{\pi}}\sin^{2}\theta \left(143 \cos^{4}\theta - 110 \cos^{2}\theta + 15\right) \cos\theta e^{- 2 i \phi}\\
Y_{7}^{-1}(\theta, \phi)&=\frac{\sqrt{210}}{128 \sqrt{\pi}} \sin\theta \left(429 \cos^{6}\theta - 495 \cos^{4}\theta + 135 \cos^{2}\theta - 5\right) e^{- i \phi}\\
Y_{7}^{ 0}(\theta, \phi)&=\frac{\sqrt{15}}{32 \sqrt{\pi}} \left(429 \cos^{6}\theta - 693 \cos^{4}\theta + 315 \cos^{2}\theta - 35\right) \cos\theta\\
Y_{7}^{ 1}(\theta, \phi)&=\frac{\sqrt{210}}{128 \sqrt{\pi}} \sin\theta \left(- 429 \cos^{6}\theta + 495 \cos^{4}\theta - 135 \cos^{2}\theta + 5\right) e^{i \phi}\\
Y_{7}^{ 2}(\theta, \phi)&=\frac{3 \sqrt{35}}{64 \sqrt{\pi}} \sin^{2}\theta \left(143 \cos^{4}\theta - 110 \cos^{2}\theta + 15\right) \cos\theta e^{2 i \phi}\\
Y_{7}^{ 3}(\theta, \phi)&=\frac{3 \sqrt{70}}{128 \sqrt{\pi}} \sin^{3}\theta \left(143 \cos^{4}\theta - 66 \cos^{2}\theta +  3\right) e^{3 i \phi}\\
Y_{7}^{ 4}(\theta, \phi)&=\frac{3 \sqrt{770}}{64 \sqrt{\pi}} \sin^{4}\theta \left(13 \cos^{2}\theta - 3\right) \cos\theta e^{4 i \phi}\\
Y_{7}^{ 5}(\theta, \phi)&=\frac{3 \sqrt{770}}{128 \sqrt{\pi}} \sin^{5}\theta \left(1 - 13 \cos^{2}\theta\right) e^{5 i \phi}\\
Y_{7}^{ 6}(\theta, \phi)&=\frac{3 \sqrt{5005}}{64 \sqrt{\pi}} \sin^{6}\theta \cos\theta e^{6 i \phi}\\
Y_{7}^{ 7}(\theta, \phi)&=- \frac{3 \sqrt{1430}}{128 \sqrt{\pi}} \sin^{7}\theta e^{7 i \phi}\\
\end{align*}
$$
**8**
$$
\begin{align*}
Y_{8}^{-8}(\theta, \phi)&=\frac{3 \sqrt{24310}}{512 \sqrt{\pi}} \sin^{8}\theta e^{- 8 i \phi}\\
Y_{8}^{-7}(\theta, \phi)&=\frac{3 \sqrt{24310}}{128 \sqrt{\pi}} \sin^{7}\theta \cos\theta e^{- 7 i \phi}\\
Y_{8}^{-6}(\theta, \phi)&=\frac{\sqrt{7293}}{128 \sqrt{\pi}} \sin^{6}\theta \left(15 \cos^{2}\theta - 1\right) e^{- 6 i \phi}\\
Y_{8}^{-5}(\theta, \phi)&=\frac{3 \sqrt{34034}}{128 \sqrt{\pi}} \sin^{5}\theta \left(5 \cos^{2}\theta - 1\right) e^{- 5 i \phi} \cos\theta\\
Y_{8}^{-4}(\theta, \phi)&=\frac{3 \sqrt{2618}}{256 \sqrt{\pi}} \sin^{4}\theta \left(65 \cos^{4}\theta - 26 \cos^{2}\theta + 1\right) e^{- 4 i \phi}\\
Y_{8}^{-3}(\theta, \phi)&=\frac{\sqrt{39270}}{128 \sqrt{\pi}} \sin^{3}\theta \left(39 \cos^{5}\theta - 26 \cos^{2}\theta + 3\right) \cos\theta e^{- 3 i \phi}\\
Y_{8}^{-2}(\theta, \phi)&=\frac{3 \sqrt{595}}{128 \sqrt{\pi}} \sin^{2}\theta \left(143 \cos^{6}\theta - 143 \cos^{4}\theta + 33 \cos^{2}\theta - 1\right) e^{- 2 i \phi}\\
Y_{8}^{-1}(\theta, \phi)&=\frac{3 \sqrt{34}}{128 \sqrt{\pi}} \sin\theta \left(715 \cos^{6}\theta - 1001 \cos^{4}\theta + 385 \cos^{2}\theta - 35\right) \cos\theta e^{- i \phi}\\
Y_{8}^{ 0}(\theta, \phi)&=\frac{\sqrt{17}}{256 \sqrt{\pi}} \left(6435 \cos^{8}\theta - 12012 \cos^{6}\theta + 6930 \cos^{4}\theta - 1260 \cos^{2}\theta + 35\right)\\
Y_{8}^{ 1}(\theta, \phi)&=\frac{3 \sqrt{34}}{128 \sqrt{\pi}} \sin\theta \left(- 715 \cos^{6}\theta + 1001 \cos^{4}\theta - 385 \cos^{2}\theta + 35\right) \cos\theta e^{i \phi}\\
Y_{8}^{ 2}(\theta, \phi)&=\frac{3 \sqrt{595}}{128 \sqrt{\pi}} \sin^{2}\theta \left(143 \cos^{6}\theta - 143 \cos^{4}\theta + 33 \cos^{2}\theta - 1\right) e^{2 i \phi}\\
Y_{8}^{ 3}(\theta, \phi)&=\frac{\sqrt{39270}}{128 \sqrt{\pi}} \sin^{3}\theta \left(39 \cos^{5}\theta - 26 \cos^{2}\theta + 3\right) \cos\theta e^{3 i \phi}\\
Y_{8}^{ 4}(\theta, \phi)&=\frac{3 \sqrt{2618}}{256 \sqrt{\pi}} \sin^{4}\theta \left(65 \cos^{4}\theta - 26 \cos^{2}\theta + 1\right) e^{4 i \phi}\\
Y_{8}^{ 5}(\theta, \phi)&=- \frac{3 \sqrt{34034}}{128 \sqrt{\pi}} \sin^{5}\theta \left(5 \cos^{2}\theta - 1\right) \cos\theta e^{5 i \phi}\\
Y_{8}^{ 6}(\theta, \phi)&=\frac{\sqrt{7293}}{128 \sqrt{\pi}} \sin^{6}\theta \left(15 \cos^{2}\theta - 1\right) e^{6 i \phi}\\
Y_{8}^{ 7}(\theta, \phi)&=- \frac{3 \sqrt{24310}}{128 \sqrt{\pi}} \sin^{7}\theta \cos\theta e^{7 i \phi}\\
Y_{8}^{ 8}(\theta, \phi)&=\frac{3 \sqrt{24310}}{512 \sqrt{\pi}} \sin^{8}\theta e^{8 i \phi}\\
\end{align*}
$$
**9**
$$
\begin{align*}
Y_{9}^{-9}(\theta, \phi)&=\frac{\sqrt{230945}}{512 \sqrt{\pi}} \sin^{9}\theta e^{- 9 i \phi}\\
Y_{9}^{-8}(\theta, \phi)&=\frac{3 \sqrt{461890}}{512 \sqrt{\pi}} \sin^{8}\theta \cos\theta e^{- 8 i \phi}\\
Y_{9}^{-7}(\theta, \phi)&=\frac{3 \sqrt{13585}}{512 \sqrt{\pi}} \sin^{7}\theta \left(17 \cos^{2}\theta - 1\right) e^{- 7 i \phi}\\
Y_{9}^{-6}(\theta, \phi)&=\frac{\sqrt{40755}}{128 \sqrt{\pi}} \sin^{6}\theta \left(17 \cos^{2}\theta - 3\right) \cos\theta e^{- 6 i \phi}\\  
Y_{9}^{-5}(\theta, \phi)&=\frac{3 \sqrt{2717}}{256 \sqrt{\pi}} \sin^{5}\theta \left(85 \cos^{4}\theta - 30 \cos^{2}\theta + 1\right) e^{- 5 i \phi}\\
Y_{9}^{-4}(\theta, \phi)&=\frac{3 \sqrt{190190}}{256 \sqrt{\pi}} \sin^{4}\theta \left(17 \cos^{4}\theta - 10 \cos^{2}\theta + 1\right) \cos\theta e^{- 4 i \phi}\\
Y_{9}^{-3}(\theta, \phi)&=\frac{\sqrt{21945}}{256 \sqrt{\pi}} \sin^{3}\theta \left(221 \cos^{6}\theta - 195 \cos^{4}\theta + 39 \cos^{2}\theta - 1\right) e^{- 3 i \phi}\\
Y_{9}^{-2}(\theta, \phi)&=\frac{3 \sqrt{1045}}{128 \sqrt{\pi}} \sin^{2}\theta \left(221 \cos^{6}\theta - 273 \cos^{4}\theta + 91 \cos^{2}\theta - 7\right) \cos\theta e^{- 2 i \phi}\\
Y_{9}^{-1}(\theta, \phi)&=\frac{3 \sqrt{190}}{512 \sqrt{\pi}} \sin\theta \left(2431 \cos^{8}\theta - 4004 \cos^{6}\theta + 2002 \cos^{4}\theta - 308 \cos^{2}\theta + 7\right) e^{- i \phi}\\
Y_{9}^{ 0}(\theta, \phi)&=\frac{\sqrt{19}}{256 \sqrt{\pi}} \left(12155 \cos^{8}\theta - 25740 \cos^{6}\theta + 18018 \cos^{4}\theta - 4620 \cos^{2}\theta + 315\right) \cos\theta\\
Y_{9}^{ 1}(\theta, \phi)&=\frac{3 \sqrt{190}}{512 \sqrt{\pi}} \sin\theta \left(- 2431 \cos^{8}\theta + 4004 \cos^{6}\theta - 2002 \cos^{4}\theta + 308 \cos^{2}\theta - 7\right) e^{i \phi}\\
Y_{9}^{ 2}(\theta, \phi)&=\frac{3 \sqrt{1045}}{128 \sqrt{\pi}} \sin^{2}\theta \left(221 \cos^{6}\theta - 273 \cos^{4}\theta + 91 \cos^{2}\theta - 7\right) \cos\theta e^{2 i \phi}\\
Y_{9}^{ 3}(\theta, \phi)&=- \frac{\sqrt{21945}}{256 \sqrt{\pi}} \sin^{3}\theta \left(221 \cos^{6}\theta - 195 \cos^{4}\theta + 39 \cos^{2}\theta - 1\right) e^{3 i \phi}\\
Y_{9}^{ 4}(\theta, \phi)&=\frac{3 \sqrt{190190}}{256 \sqrt{\pi}} \sin^{4}\theta \left(17 \cos^{4}\theta - 10 \cos^{2}\theta + 1\right) \cos\theta e^{4 i \phi}\\
Y_{9}^{ 5}(\theta, \phi)&=-\frac{3 \sqrt{2717}}{256 \sqrt{\pi}} \sin^{5}\theta \left(85 \cos^{4}\theta - 30 \cos^{2}\theta + 1\right) e^{5 i \phi}\\
Y_{9}^{ 6}(\theta, \phi)&=\frac{\sqrt{40755}}{128 \sqrt{\pi}} \sin^{6}\theta \left(17 \cos^{2}\theta - 3\right) \cos\theta e^{6 i \phi}\\
Y_{9}^{ 7}(\theta, \phi)&=-\frac{3 \sqrt{13585}}{512 \sqrt{\pi}} \sin^{7}\theta \left(17 \cos^{2}\theta - 1\right) e^{7 i \phi}\\
Y_{9}^{ 8}(\theta, \phi)&=\frac{3 \sqrt{461890}}{512 \sqrt{\pi}} \sin^{8}\theta \cos\theta e^{8 i \phi}\\
Y_{9}^{ 9}(\theta, \phi)&=- \frac{\sqrt{230945}}{512 \sqrt{\pi}} \sin^{9}\theta e^{9 i \phi}\\
\end{align*}
$$

## Multi Angle Formula

一些可能会用到的多倍角公式

$$
\begin{align*}
\sin\left(2 x\right)&=2 \sin{\left(x \right)} \cos{\left(x \right)}\\
\sin\left(3 x\right)&=- \sin^{3}{\left(x \right)} + 3 \sin{\left(x \right)} \cos^{2}{\left(x \right)}\\
\sin\left(4 x\right)&=- 4 \sin^{3}{\left(x \right)} \cos{\left(x \right)} + 4 \sin{\left(x \right)} \cos^{3}{\left(x \right)}\\
\sin\left(5 x\right)&=\sin^{5}{\left(x \right)} - 10 \sin^{3}{\left(x \right)} \cos^{2}{\left(x \right)} + 5 \sin{\left(x \right)} \cos^{4}{\left(x \right)}\\
\sin\left(6 x\right)&=6 \sin^{5}{\left(x \right)} \cos{\left(x \right)} - 20 \sin^{3}{\left(x \right)} \cos^{3}{\left(x \right)} + 6 \sin{\left(x \right)} \cos^{5}{\left(x \right)}\\
\sin\left(7 x\right)&=- \sin^{7}{\left(x \right)} + 21 \sin^{5}{\left(x \right)} \cos^{2}{\left(x \right)} - 35 \sin^{3}{\left(x \right)} \cos^{4}{\left(x \right)} + 7 \sin{\left(x \right)} \cos^{6}{\left(x \right)}\\
\sin\left(8 x\right)&=- 8 \sin^{7}{\left(x \right)} \cos{\left(x \right)} + 56 \sin^{5}{\left(x \right)} \cos^{3}{\left(x \right)} - 56 \sin^{3}{\left(x \right)} \cos^{5}{\left(x \right)} + 8 \sin{\left(x \right)} \cos^{7}{\left(x \right)}\\
\sin\left(9 x\right)&=\sin^{9}{\left(x \right)} - 36 \sin^{7}{\left(x \right)} \cos^{2}{\left(x \right)} + 126 \sin^{5}{\left(x \right)} \cos^{4}{\left(x \right)} - 84 \sin^{3}{\left(x \right)} \cos^{6}{\left(x \right)} + 9 \sin{\left(x \right)} \cos^{8}{\left(x \right)}\\
\cos\left(2 x\right)&=- \sin^{2}{\left(x \right)} + \cos^{2}{\left(x \right)}\\
\cos\left(3 x\right)&=- 3 \sin^{2}{\left(x \right)} \cos{\left(x \right)} + \cos^{3}{\left(x \right)}\\
\cos\left(4 x\right)&=\sin^{4}{\left(x \right)} - 6 \sin^{2}{\left(x \right)} \cos^{2}{\left(x \right)} + \cos^{4}{\left(x \right)}\\
\cos\left(5 x\right)&=5 \sin^{4}{\left(x \right)} \cos{\left(x \right)} - 10 \sin^{2}{\left(x \right)} \cos^{3}{\left(x \right)} + \cos^{5}{\left(x \right)}\\
\cos\left(6 x\right)&=- \sin^{6}{\left(x \right)} + 15 \sin^{4}{\left(x \right)} \cos^{2}{\left(x \right)} - 15 \sin^{2}{\left(x \right)} \cos^{4}{\left(x \right)} + \cos^{6}{\left(x \right)}\\
\cos\left(7 x\right)&=- 7 \sin^{6}{\left(x \right)} \cos{\left(x \right)} + 35 \sin^{4}{\left(x \right)} \cos^{3}{\left(x \right)} - 21 \sin^{2}{\left(x \right)} \cos^{5}{\left(x \right)} + \cos^{7}{\left(x \right)}\\
\cos\left(8 x\right)&=\sin^{8}{\left(x \right)} - 28 \sin^{6}{\left(x \right)} \cos^{2}{\left(x \right)} + 70 \sin^{4}{\left(x \right)} \cos^{4}{\left(x \right)} - 28 \sin^{2}{\left(x \right)} \cos^{6}{\left(x \right)} + \cos^{8}{\left(x \right)}\\
\cos\left(9 x\right)&=9 \sin^{8}{\left(x \right)} \cos{\left(x \right)} - 84 \sin^{6}{\left(x \right)} \cos^{3}{\left(x \right)} + 126 \sin^{4}{\left(x \right)} \cos^{5}{\left(x \right)} - 36 \sin^{2}{\left(x \right)} \cos^{7}{\left(x \right)} + \cos^{9}{\left(x \right)}\\
\end{align*}
$$

## Real Spherical Harmonics

定义实值的 Spherical Harmonics 为：
$$
Y_{lm}=\begin{cases}
\dfrac{i}{\sqrt{2}}\left(Y_{l}^{m}-(-1)^{m}Y_{l}^{-m}\right)&m<0\\
Y_{l}^{0}&m=0\\
\dfrac{1}{\sqrt{2}}\left(Y_{l}^{-m}+(-1)^{m}Y_{l}^{m}\right)&m>0\\
\end{cases}
$$
因此我们就有：
$$
Y_{l}^{m}=\begin{cases}
\dfrac{1}{\sqrt{2}}\left(Y_{l|m|}-iY_{l,-|m|}\right)&m<0\\
Y_{l0}&m=0\\
\dfrac{(-1)^m}{\sqrt{2}}\left(Y_{l|m|}+iY_{l,-|m|}\right)&m>0\\
\end{cases}
$$
$1/\sqrt{2}$ 保证了模长都是归一化的。

**0**
$$
Y_{0, 0}(\theta, \phi)=\frac{1}{2 \sqrt{\pi}}
$$
**1**
$$
\begin{align*}
Y_{1,-1}(\theta, \phi)&=\frac{\sqrt{3}}{2 \sqrt{\pi}} \sin\theta \sin\phi&=&\frac{\sqrt{3}}{2 \sqrt{\pi}} y\\
Y_{1, 0}(\theta, \phi)&=\frac{\sqrt{3}}{2 \sqrt{\pi}} \cos\theta&=&\frac{\sqrt{3}}{2 \sqrt{\pi}} z\\
Y_{1, 1}(\theta, \phi)&=\frac{\sqrt{3}}{2 \sqrt{\pi}} \sin\theta \cos \phi&=&\frac{\sqrt{3}}{2 \sqrt{\pi}} x\\
\end{align*}
$$
**2**
$$
\begin{align*}
Y_{2,-2}(\theta, \phi)&=\frac{\sqrt{15}}{4 \sqrt{\pi}} \sin^{2} \theta \sin2 \phi&=&\frac{\sqrt{15}}{2 \sqrt{\pi}} xy\\
Y_{2,-1}(\theta, \phi)&=\frac{\sqrt{15}}{2 \sqrt{\pi}} \sin\theta \cos\theta \sin\phi&=&\frac{\sqrt{15}}{2 \sqrt{\pi}} yz\\
Y_{2, 0}(\theta, \phi)&=\frac{\sqrt{5}}{4 \sqrt{\pi}} \left(3 \cos^{2} \theta - 1\right)&=&\frac{\sqrt{5}}{4 \sqrt{\pi}} \left(3 z^{2} - 1\right)\\
Y_{2, 1}(\theta, \phi)&=\frac{\sqrt{15}}{2 \sqrt{\pi}} \sin\theta \cos\theta \cos \phi&=&\frac{\sqrt{15}}{2 \sqrt{\pi}} xz\\
Y_{2, 2}(\theta, \phi)&=\frac{\sqrt{15}}{4 \sqrt{\pi}} \sin^{2} \theta \cos2 \phi&=&\frac{\sqrt{15}}{4 \sqrt{\pi}} (x^{2} - y^{2})\\
\end{align*}
$$
**3**
$$
\begin{align*}
Y_{3,-3}(\theta, \phi)&=\frac{\sqrt{70}}{8 \sqrt{\pi}} \sin^{3} \theta \sin3 \phi&=&\frac{\sqrt{70}}{8 \sqrt{\pi}} y \left(3 x^{2} - y^{2}\right)\\
Y_{3,-2}(\theta, \phi)&=\frac{\sqrt{105}}{4 \sqrt{\pi}} \sin^{2} \theta \cos\theta \sin2 \phi&=&\frac{\sqrt{105}}{2 \sqrt{\pi}} xyz\\
Y_{3,-1}(\theta, \phi)&=\frac{\sqrt{42}}{8 \sqrt{\pi}} \left(5 \cos^{2} \theta - 1\right) \sin\theta \sin\phi&=&\frac{\sqrt{42}}{8 \sqrt{\pi}} y \left(5 z^{2} - 1\right)\\
Y_{3, 0}(\theta, \phi)&=\frac{\sqrt{7}}{4 \sqrt{\pi}} \left(5 \cos^{2} \theta - 3\right) \cos\theta&=&\frac{\sqrt{7}}{4 \sqrt{\pi}} z \left(5 z^{2} - 3\right)\\
Y_{3, 1}(\theta, \phi)&=\frac{\sqrt{42}}{8 \sqrt{\pi}} \left(5 \cos^{2} \theta - 1\right) \sin\theta \cos \phi&=&\frac{\sqrt{42}}{8 \sqrt{\pi}} x \left(5 z^{2} - 1\right)\\
Y_{3, 2}(\theta, \phi)&=\frac{\sqrt{105}}{4 \sqrt{\pi}} \sin^{2} \theta \cos\theta \cos2 \phi&=&\frac{\sqrt{105}}{4 \sqrt{\pi}} z (x^{2} - y^{2})\\
Y_{3, 3}(\theta, \phi)&=\frac{\sqrt{70}}{8 \sqrt{\pi}} \sin^{3} \theta \cos3 \phi&=&\frac{\sqrt{70}}{8 \sqrt{\pi}} x (x^{2} - 3 y^{2})\\
\end{align*}
$$
**4**
$$
\begin{align*}
Y_{4,-4}(\theta, \phi)&=\frac{3 \sqrt{35}}{16 \sqrt{\pi}} \sin^{4} \theta \sin4 \phi&=&\frac{3 \sqrt{35}}{4 \sqrt{\pi}} xy \left(x^{2} - y^{2}\right)\\
Y_{4,-3}(\theta, \phi)&=\frac{3 \sqrt{70}}{8 \sqrt{\pi}} \sin^{3}\theta \cos\theta \sin3 \phi&=&\frac{3 \sqrt{70}}{8 \sqrt{\pi}} yz \left(3 x^{2} - y^{2}\right)\\
Y_{4,-2}(\theta, \phi)&=\frac{3 \sqrt{5}}{8 \sqrt{\pi}} \left(7 \cos^{2} \theta - 1\right) \sin^{2} \theta \sin2 \phi&=&\frac{3 \sqrt{5}}{4 \sqrt{\pi}} xy \left(7 z^{2} - 1\right)\\
Y_{4,-1}(\theta, \phi)&=\frac{3 \sqrt{10}}{8 \sqrt{\pi}} \left(7 \cos^{2} \theta - 3\right) \sin \theta \cos\theta \sin\phi&=&\frac{3 \sqrt{10}}{8 \sqrt{\pi}} yz \left(7 z^{2} - 3\right)\\
Y_{4, 0}(\theta, \phi)&=\frac{3}{16 \sqrt{\pi}} \left(35 \cos^{4} \theta - 30 \cos^{2} \theta + 3\right)&=&\frac{3}{16 \sqrt{\pi}} \left(35 z^{4} - 30 z^{2} + 3\right)\\
Y_{4, 1}(\theta, \phi)&=\frac{3 \sqrt{10}}{8 \sqrt{\pi}} \left(7 \cos^{2} \theta - 3\right) \sin \theta \cos\theta \cos \phi&=&\frac{3 \sqrt{10}}{8 \sqrt{\pi}} xz \left(7 z^{2} - 3\right)\\
Y_{4, 2}(\theta, \phi)&=\frac{3 \sqrt{5}}{8 \sqrt{\pi}} \left(7 \cos^{2} \theta - 1\right) \sin^{2} \theta \cos2 \phi&=&\frac{3 \sqrt{5}}{8 \sqrt{\pi}} \left(x^{2} - y^{2}\right) \left(7 z^{2} - 1\right)\\
Y_{4, 3}(\theta, \phi)&=\frac{3 \sqrt{70}}{8 \sqrt{\pi}} \sin^{3}\theta \cos\theta \cos3 \phi&=&\frac{3 \sqrt{70}}{8 \sqrt{\pi}} xz \left(x^{2} - 3 y^{2}\right)\\
Y_{4, 4}(\theta, \phi)&=\frac{3 \sqrt{35}}{16 \sqrt{\pi}} \sin^{4} \theta \cos4 \phi&=&\frac{3 \sqrt{35}}{16 \sqrt{\pi}} \left(x^{4} - 6 x^{2} y^{2} + y^{4}\right)\\
\end{align*}
$$
**5**
$$
\begin{align*}
Y_{5,-5}(\theta, \phi)&=\frac{3 \sqrt{154}}{32 \sqrt{\pi}} \sin^{5} \theta \sin5 \phi&=&\frac{3 \sqrt{154}}{32 \sqrt{\pi}} y \left(5 x^{4} - 10 x^{2} y^{2} + y^{4}\right)\\
Y_{5,-4}(\theta, \phi)&=\frac{3 \sqrt{385}}{16 \sqrt{\pi}} \sin^{4} \theta \cos\theta \sin4 \phi&=&\frac{3 \sqrt{385}}{4 \sqrt{\pi}}xyz \left(x^{2} - y^{2}\right)\\
Y_{5,-3}(\theta, \phi)&=\frac{\sqrt{770}}{32 \sqrt{\pi}} \left(9 \cos^{2} \theta - 1\right) \sin^{3}\theta \sin3 \phi&=&\frac{\sqrt{770}}{32 \sqrt{\pi}} y \left(9 z^{2} - 1\right) \left(3 x^{2} - y^{2}\right)\\
Y_{5,-2}(\theta, \phi)&=\frac{\sqrt{1155}}{8 \sqrt{\pi}} \left(3 \cos^{2} \theta - 1\right) \sin^{2} \theta \cos\theta \sin2 \phi&=&\frac{\sqrt{1155}}{4 \sqrt{\pi}} xyz \left(3 z^{2} - 1\right)\\
Y_{5,-1}(\theta, \phi)&=\frac{\sqrt{165}}{16 \sqrt{\pi}} \left(21 \cos^{4} \theta - 14 \cos^{2} \theta + 1\right) \sin \theta \sin \phi&=&\frac{\sqrt{165}}{16 \sqrt{\pi}} y \left(21 z^{4} - 14 z^{2} + 1\right)\\
Y_{5, 0}(\theta, \phi)&=\frac{\sqrt{11}}{16 \sqrt{\pi}} \left(63 \cos^{4} \theta - 70 \cos^{2} \theta + 15\right) \cos\theta&=&\frac{\sqrt{11}}{16 \sqrt{\pi}} z \left(63 z^{4} - 70 z^{2} + 15\right)\\
Y_{5, 1}(\theta, \phi)&=\frac{\sqrt{165}}{16 \sqrt{\pi}} \left(21 \cos^{4} \theta - 14 \cos^{2} \theta + 1\right) \sin \theta \cos \phi&=&\frac{\sqrt{165}}{16 \sqrt{\pi}} x \left(21 z^{4} - 14 z^{2} + 1\right)\\
Y_{5, 2}(\theta, \phi)&=\frac{\sqrt{1155}}{8 \sqrt{\pi}} \left(3 \cos^{2} \theta - 1\right) \sin^{2} \theta \cos\theta \cos2 \phi&=&\frac{\sqrt{1155}}{8 \sqrt{\pi}} z \left(x^{2} - y^{2}\right) \left(3 z^{2} - 1\right)\\
Y_{5, 3}(\theta, \phi)&=\frac{\sqrt{770}}{32 \sqrt{\pi}} \left(9 \cos^{2} \theta - 1\right) \sin^{3}\theta \cos3 \phi&=&\frac{\sqrt{770}}{32 \sqrt{\pi}} x \left(9 z^{2} - 1\right)  (x^{2} - 3 y^{2})\\
Y_{5, 4}(\theta, \phi)&=\frac{3 \sqrt{385}}{16 \sqrt{\pi}} \sin^{4} \theta \cos\theta \cos4 \phi&=&\frac{3 \sqrt{385}}{16 \sqrt{\pi}} z \left(x^{4} - 6 x^{2} y^{2} + y^{4}\right)\\
Y_{5, 5}(\theta, \phi)&=\frac{3 \sqrt{154}}{32 \sqrt{\pi}} \sin^{5} \theta \cos5 \phi&=&\frac{3 \sqrt{154}}{32 \sqrt{\pi}} x \left(x^{4} - 10 x^{2} y^{2} + 5 y^{4}\right)\\
\end{align*}
$$
**6**
$$
\begin{align*}
Y_{6,-6}(\theta, \phi)&=\frac{\sqrt{6006}}{64 \sqrt{\pi}} \sin^{6} \theta \sin6 \phi&=&\frac{\sqrt{6006}}{32 \sqrt{\pi}} xy \left(3 x^{4} - 10 x^{2} y^{2} + 3 y^{4}\right)\\
Y_{6,-5}(\theta, \phi)&=\frac{3 \sqrt{2002}}{32 \sqrt{\pi}} \sin^{5} \theta \cos\theta \sin5 \phi&=&\frac{3 \sqrt{2002}}{32 \sqrt{\pi}} yz \left(5 x^{4} - 10 x^{2} y^{2} + y^{4}\right)\\
Y_{6,-4}(\theta, \phi)&=\frac{3 \sqrt{91}}{32 \sqrt{\pi}} \left(11 \cos^{2} \theta - 1\right) \sin^{4} \theta \sin4 \phi&=&\frac{3 \sqrt{91}}{8 \sqrt{\pi}} xy \left(11 z^{2} - 1\right) \left(x^{2} - y^{2}\right)\\
Y_{6,-3}(\theta, \phi)&=\frac{\sqrt{2730}}{32 \sqrt{\pi}} \left(11 \cos^{2} \theta - 3\right) \sin^{3}\theta \cos\theta \sin3 \phi&=&\frac{\sqrt{2730}}{32 \sqrt{\pi}} yz \left(11 z^{2} - 3\right) \left(3 x^{2} - y^{2}\right)\\
Y_{6,-2}(\theta, \phi)&=\frac{\sqrt{2730}}{64 \sqrt{\pi}} \left(33 \cos^{4} \theta - 18 \cos^{2} \theta + 1\right) \sin^{2} \theta \sin2 \phi&=&\frac{\sqrt{2730}}{32 \sqrt{\pi}} xy \left(33 z^{4} - 18 z^{2} + 1\right)\\
Y_{6,-1}(\theta, \phi)&=\frac{\sqrt{273}}{16 \sqrt{\pi}} \left(33 \cos^{4} \theta - 30 \cos^{2} \theta + 5\right) \sin \theta \cos\theta \sin\phi&=&\frac{\sqrt{273}}{16 \sqrt{\pi}} yz \left(33 z^{4} - 30 z^{2} + 5\right)\\
Y_{6, 0}(\theta, \phi)&=\frac{\sqrt{13}}{32 \sqrt{\pi}} \left(231 \cos^{6} \theta - 315 \cos^{4} \theta + 105 \cos^{2} \theta - 5\right)&=&\frac{\sqrt{13}}{32 \sqrt{\pi}} \left(231 z^{6} - 315 z^{4} + 105 z^{2} - 5\right)\\
Y_{6, 1}(\theta, \phi)&=\frac{\sqrt{273}}{16 \sqrt{\pi}} \left(33 \cos^{4} \theta - 30 \cos^{2} \theta + 5\right) \sin \theta \cos\theta \cos \phi&=&\frac{\sqrt{273}}{16 \sqrt{\pi}} xz \left(33 z^{4} - 30 z^{2} + 5\right)\\
Y_{6, 2}(\theta, \phi)&=\frac{\sqrt{2730}}{64 \sqrt{\pi}} \left(33 \cos^{4} \theta - 18 \cos^{2} \theta + 1\right) \sin^{2} \theta \cos2 \phi&=&\frac{\sqrt{2730}}{64 \sqrt{\pi}} \left(x^{2} - y^{2}\right) \left(33 z^{4} - 18 z^{2} + 1\right)\\
Y_{6, 3}(\theta, \phi)&=\frac{\sqrt{2730}}{32 \sqrt{\pi}} \left(11 \cos^{2} \theta - 3\right) \sin^{3}\theta \cos\theta \cos3 \phi&=&\frac{\sqrt{2730}}{32 \sqrt{\pi}} xz \left(x^{2} - 3 y^{2}\right) \left(11 z^{2} - 3\right)\\
Y_{6, 4}(\theta, \phi)&=\frac{3 \sqrt{91}}{32 \sqrt{\pi}} \left(11 \cos^{2} \theta - 1\right) \sin^{4} \theta \cos4 \phi&=&\frac{3 \sqrt{91}}{32 \sqrt{\pi}} \left(11 z^{2} - 1\right) \left(x^{4} - 6 x^{2} y^{2} + y^{4}\right)\\
Y_{6, 5}(\theta, \phi)&=\frac{3 \sqrt{2002}}{32 \sqrt{\pi}} \sin^{5} \theta \cos\theta \cos5 \phi&=&\frac{3 \sqrt{2002}}{32 \sqrt{\pi}} xz \left(x^{4} - 10 x^{2} y^{2} + 5 y^{4}\right)\\
Y_{6, 6}(\theta, \phi)&=\frac{\sqrt{6006}}{64 \sqrt{\pi}} \sin^{6} \theta \cos6 \phi&=&\frac{\sqrt{6006}}{64 \sqrt{\pi}} \left(x^{6} - 15 x^{4} y ^{2} + 15 x^{2} y^{4} - y^{6}\right)\\
\end{align*}
$$
**7**
$$
\begin{align*}
Y_{7,-7}(\theta, \phi)&=\frac{3 \sqrt{715}}{64 \sqrt{\pi}} \sin^{7} \theta \sin7 \phi&=&\frac{3 \sqrt{715}}{64 \sqrt{\pi}} y \left(7 x^{6} - 35 y^{2} x^{4} + 21 y^{4} x^{2} - y^{6}\right)\\
Y_{7,-6}(\theta, \phi)&=\frac{3 \sqrt{10010}}{64 \sqrt{\pi}} \sin^{6} \theta \cos\theta \sin6 \phi&=&\frac{3 \sqrt{10010}}{32 \sqrt{\pi}} xyz \left(3 x^{4} - 10 x^{2} y^{2} + 3 y^{4}\right)\\
Y_{7,-5}(\theta, \phi)&=\frac{3 \sqrt{385}}{128 \sqrt{\pi}} \left(13 \cos^{2} \theta - 1\right) \sin^{5} \theta \sin5 \phi&=&\frac{3 \sqrt{385}}{128 \sqrt{\pi}} y \left(13 z^{2} - 1\right) \left(5 x^{4} - 10 x^{2} y^{2} + y^{4}\right)\\
Y_{7,-4}(\theta, \phi)&=\frac{3 \sqrt{385}}{32 \sqrt{\pi}} \left(13 \cos^{2} \theta - 3\right) \sin^{4} \theta \cos\theta \sin4 \phi&=&\frac{3 \sqrt{385}}{8 \sqrt{\pi}} xyz \left(13 z^{2} - 3\right) \left(x^{2} - y^{2}\right)\\
Y_{7,-3}(\theta, \phi)&=\frac{3 \sqrt{35}}{128 \sqrt{\pi}} \left(- 143 \sin^{4} \theta + 220 \sin^{2} \theta - 80\right) \sin^{3} \theta \sin3 \phi&=&\frac{3 \sqrt{35}}{128 \sqrt{\pi}} y \left(143 z^{4} - 66 z^{2} + 3\right) \left(3 x^{2} - y^{2}\right)\\
Y_{7,-2}(\theta, \phi)&=\frac{3 \sqrt{70}}{64 \sqrt{\pi}} \left(143 \sin^{4} \theta - 176 \sin^{2} \theta + 48\right) \sin^{2} \theta \cos\theta \sin2 \phi&=&\frac{3 \sqrt{70}}{32 \sqrt{\pi}} xyz \left(143 z^{4} - 110 z^{2} + 15\right)\\
Y_{7,-1}(\theta, \phi)&=\frac{\sqrt{105}}{64 \sqrt{\pi}} \left(429 \cos^{6} \theta - 495 \cos^{4} \theta + 135 \cos^{2} \theta - 5\right) \sin \theta \sin \phi&=&\frac{\sqrt{105}}{64 \sqrt{\pi}} y \left(429 z^{6} - 495 z^{4} + 135 z^{2} - 5\right)\\
Y_{7, 0}(\theta, \phi)&=\frac{\sqrt{15}}{32 \sqrt{\pi}} \left(429 \cos^{6} \theta - 693 \cos^{4} \theta + 315 \cos^{2} \theta - 35\right) \cos\theta&=&\frac{\sqrt{15}}{32 \sqrt{\pi}} z \left(429 z^{6} - 693 z^{4} + 315 z^{2} - 35\right)\\
Y_{7, 1}(\theta, \phi)&=\frac{\sqrt{105}}{64 \sqrt{\pi}} \left(429 \cos^{6} \theta - 495 \cos^{4} \theta + 135 \cos^{2} \theta - 5\right) \sin \theta \cos \phi&=&\frac{\sqrt{105}}{64 \sqrt{\pi}} x \left(429 z^{6} - 495 z^{4} + 135 z^{2} - 5\right)\\
Y_{7, 2}(\theta, \phi)&=\frac{3 \sqrt{70}}{64 \sqrt{\pi}} \left(143 \sin^{4} \theta - 176 \sin^{2} \theta + 48\right) \sin^{2} \theta \cos\theta \cos2 \phi&=&\frac{3 \sqrt{70}}{64 \sqrt{\pi}} z \left(143 z^{4} - 110 z^{2} + 15\right) (x^{2} - y^{2})\\
Y_{7, 3}(\theta, \phi)&=\frac{3 \sqrt{35}}{64 \sqrt{\pi}} \left(143 \sin^{4} \theta - 220 \sin^{2} \theta + 80\right) \sin^{3}\theta \cos3 \phi&=&\frac{3 \sqrt{35}}{64 \sqrt{\pi}} x \left(143 z^{4} - 66 z^{2} + 3\right) (x^{2} - 3 y^{2})\\
Y_{7, 4}(\theta, \phi)&=\frac{3 \sqrt{385}}{32 \sqrt{\pi}} \left(13 \cos^{2} \theta - 3\right) \sin^{4} \theta \cos\theta \cos4 \phi&=&\frac{3 \sqrt{385}}{32 \sqrt{\pi}} z \left(13 z^{2} - 3\right) \left(x^{4} - 6 x^{2} y^{2} + y^{4}\right)\\
Y_{7, 5}(\theta, \phi)&=\frac{3 \sqrt{385}}{64 \sqrt{\pi}} \left(13 \cos^{2} \theta - 1\right) \sin^{5} \theta \cos5 \phi&=&\frac{3 \sqrt{385}}{64 \sqrt{\pi}} x \left(13 z^{2} - 1\right) \left(x^{4} - 10 x^{2} y^{2} + 5 y^{4}\right)\\
Y_{7, 6}(\theta, \phi)&=\frac{3 \sqrt{10010}}{64 \sqrt{\pi}} \sin^{6} \theta \cos\theta \cos6 \phi&=&\frac{3 \sqrt{10010}}{64 \sqrt{\pi}} z  \left(x^{6} - 15 x^{4} y ^{2} + 15 x^{2} y^{4} - y^{6}\right)\\
Y_{7, 7}(\theta, \phi)&=\frac{3 \sqrt{715}}{64 \sqrt{\pi}} \sin^{7} \theta \cos7 \phi&=&\frac{3 \sqrt{715}}{64 \sqrt{\pi}} x \left(x^{6} - 21 x^{4} y^{2} + 35 x^{2} y^{4} - 7 y^{6}\right)\\
\end{align*}
$$
$$
\begin{align*}
Y_{8,-8}(\theta, \phi)&=\frac{3 \sqrt{12155}}{256 \sqrt{\pi}} \sin^{8} \theta \sin8 \phi&=&\frac{3 \sqrt{12155}}{256 \sqrt{\pi}} \sin^{8} \theta \sin8 \phi\\
Y_{8,-7}(\theta, \phi)&=\frac{3 \sqrt{12155}}{64 \sqrt{\pi}} \sin^{7} \theta \cos \theta \sin7 \phi&=&\frac{3 \sqrt{12155}}{64 \sqrt{\pi}} \sin^{7} \theta \cos \theta \sin7 \phi\\
Y_{8,-6}(\theta, \phi)&=\frac{\sqrt{14586}}{256 \sqrt{\pi}} \left(15 \cos^{2} \theta - 1\right) \sin^{6} \theta \sin6 \phi&=&\frac{\sqrt{14586}}{256 \sqrt{\pi}} \left(15 \cos^{2} \theta - 1\right) \sin^{6} \theta \sin6 \phi\\
Y_{8,-5}(\theta, \phi)&=\frac{3 \sqrt{17017}}{64 \sqrt{\pi}} \left(5 \cos^{2} \theta - 1\right) \sin^{5} \theta \cos \theta \sin5 \phi&=&\frac{3 \sqrt{17017}}{64 \sqrt{\pi}} \left(5 \cos^{2} \theta - 1\right) \sin^{5} \theta \cos \theta \sin5 \phi\\
Y_{8,-4}(\theta, \phi)&=\frac{3 \sqrt{1309}}{2 \sqrt{\pi}} \left(65 \sin^{4} \theta - 104 \sin^{2} \theta + 40\right) \sin^{4} \theta \sin4 \phi&=&\frac{3 \sqrt{1309}}{2 \sqrt{\pi}} \left(65 \sin^{4} \theta - 104 \sin^{2} \theta + 40\right) \sin^{4} \theta \sin4 \phi\\
Y_{8,-3}(\theta, \phi)&=\frac{\sqrt{19635}}{64 \sqrt{\pi}} \left(39 \left(\cos^{2} \theta - 1\right)^{2} + 52 \cos^{2} \theta - 36\right) \sin^{3} \theta \cos \theta \sin3 \phi&=&\frac{\sqrt{19635}}{64 \sqrt{\pi}} \left(39 \left(\cos^{2} \theta - 1\right)^{2} + 52 \cos^{2} \theta - 36\right) \sin^{3} \theta \cos \theta \sin3 \phi\\
Y_{8,-2}(\theta, \phi)&=\frac{3 \sqrt{1190}}{128 \sqrt{\pi}} \left(143 \cos^{8} \theta - 286 \cos^{6} \theta + 176 \cos^{4} \theta - 34 \cos^{2} \theta + 1\right) \sin2 \phi&=&\frac{3 \sqrt{1190}}{128 \sqrt{\pi}} \left(143 \cos^{8} \theta - 286 \cos^{6} \theta + 176 \cos^{4} \theta - 34 \cos^{2} \theta + 1\right) \sin2 \phi\\
Y_{8,-1}(\theta, \phi)&=\frac{3 \sqrt{17}}{64 \sqrt{\pi}} \left(715 \cos^{6} \theta - 1001 \cos^{4} \theta + 385 \cos^{2} \theta - 35\right) \sin \theta \cos \theta \sin \phi&=&\frac{3 \sqrt{17}}{64 \sqrt{\pi}} \left(715 \cos^{6} \theta - 1001 \cos^{4} \theta + 385 \cos^{2} \theta - 35\right) \sin \theta \cos \theta \sin \phi\\
Y_{8, 0}(\theta, \phi)&=\frac{\sqrt{17}}{256 \sqrt{\pi}} \left(6435 \cos^{8} \theta - 12012 \cos^{6} \theta + 6930 \cos^{4} \theta - 1260 \cos^{2} \theta + 35\right)&=&\frac{\sqrt{17}}{256 \sqrt{\pi}} \left(6435 \cos^{8} \theta - 12012 \cos^{6} \theta + 6930 \cos^{4} \theta - 1260 \cos^{2} \theta + 35\right)\\
Y_{8, 1}(\theta, \phi)&=\frac{3 \sqrt{17}}{64 \sqrt{\pi}} \left(715 \cos^{6} \theta - 1001 \cos^{4} \theta + 385 \cos^{2} \theta - 35\right) \sin \theta \cos \theta \cos \phi&=&\frac{3 \sqrt{17}}{64 \sqrt{\pi}} \left(715 \cos^{6} \theta - 1001 \cos^{4} \theta + 385 \cos^{2} \theta - 35\right) \sin \theta \cos \theta \cos \phi\\
Y_{8, 2}(\theta, \phi)&=- \frac{3 \sqrt{1190}}{128 \sqrt{\pi}} \left(143 \sin^{4} \theta - 253 \sin^{2} \theta - 143 \cos^{6} \theta + 111\right) \sin^{2} \theta \cos2 \phi&=&- \frac{3 \sqrt{1190}}{128 \sqrt{\pi}} \left(143 \sin^{4} \theta - 253 \sin^{2} \theta - 143 \cos^{6} \theta + 111\right) \sin^{2} \theta \cos2 \phi\\
Y_{8, 3}(\theta, \phi)&=\frac{\sqrt{19635}}{64 \sqrt{\pi}} \left(39 \left(\cos^{2} \theta - 1\right)^{2} + 52 \cos^{2} \theta - 36\right) \sin^{3} \theta \cos \theta \cos3 \phi&=&\frac{\sqrt{19635}}{64 \sqrt{\pi}} \left(39 \left(\cos^{2} \theta - 1\right)^{2} + 52 \cos^{2} \theta - 36\right) \sin^{3} \theta \cos \theta \cos3 \phi\\
Y_{8, 4}(\theta, \phi)&=\frac{3 \sqrt{1309}}{128 \sqrt{\pi}} \left(65 \sin^{4} \theta - 104 \sin^{2} \theta + 40\right) \sin^{4} \theta \cos 4 \phi&=&\frac{3 \sqrt{1309}}{128 \sqrt{\pi}} \left(65 \sin^{4} \theta - 104 \sin^{2} \theta + 40\right) \sin^{4} \theta \cos 4 \phi\\
Y_{8, 5}(\theta, \phi)&=\frac{3 \sqrt{17017}}{64 \sqrt{\pi}} \left(5 \cos^{2} \theta - 1\right) \sin^{5} \theta \cos \theta \cos5 \phi&=&\frac{3 \sqrt{17017}}{64 \sqrt{\pi}} \left(5 \cos^{2} \theta - 1\right) \sin^{5} \theta \cos \theta \cos5 \phi\\
Y_{8, 6}(\theta, \phi)&=\frac{\sqrt{14586}}{128 \sqrt{\pi}} \left(15 \cos^{2} \theta - 1\right) \sin^{6} \theta \cos6 \phi&=&\frac{\sqrt{14586}}{128 \sqrt{\pi}} \left(15 \cos^{2} \theta - 1\right) \sin^{6} \theta \cos6 \phi\\
Y_{8, 7}(\theta, \phi)&=\frac{3 \sqrt{12155}}{64 \sqrt{\pi}} \sin^{7} \theta \cos \theta \cos 7 \phi&=&\frac{3 \sqrt{12155}}{64 \sqrt{\pi}} \sin^{7} \theta \cos \theta \cos 7 \phi\\
Y_{8, 8}(\theta, \phi)&=\frac{3 \sqrt{12155}}{256 \sqrt{\pi}} \sin^{8} \theta \cos8 \phi&=&\frac{3 \sqrt{12155}}{256 \sqrt{\pi}} \sin^{8} \theta \cos8 \phi\\
\end{align*}
$$
$$
\begin{align*}
Y_{9,-9}(\theta, \phi)&=- \frac{\sqrt{461890} \sin^{9} \theta \sin9 \phi}{512 \sqrt{\pi}}\\
Y_{9,-8}(\theta, \phi)&=- \frac{3 \sqrt{230945} \sin^{8} \theta \sin8 \phi \cos \theta}{256 \sqrt{\pi}}\\
Y_{9,-7}(\theta, \phi)&=- \frac{3 \sqrt{27170} \sin^{7} \theta \left(17 \cos^{2} \theta - 1\right) \sin7 \phi}{512 \sqrt{\pi}}\\
Y_{9,-6}(\theta, \phi)&=\frac{\sqrt{81510} \sin^{6} \theta \left(17 \cos^{2} \theta - 3\right) \sin6 \phi \cos \theta}{128 \sqrt{\pi}}\\    
Y_{9,-5}(\theta, \phi)&=- \frac{3 \sqrt{5434} \sin \theta \left(85 \cos^{8} \theta - 200 \cos^{6} \theta + 146 \cos^{4} \theta - 32 \cos^{2} \theta + 1\right) \sin5 \phi}{256 \sqrt{\pi}}\\
Y_{9,-4}(\theta, \phi)&=- \frac{3 \sqrt{95095} \left(17 \cos^{8} \theta - 44 \cos^{6} \theta + 38 \cos^{4} \theta - 12 \cos^{2} \theta + 1\right) \sin4 \phi \cos \theta}{128 \sqrt{\pi}}\\
Y_{9,-3}(\theta, \phi)&=\frac{\sqrt{43890} \sin \theta \left(221 \cos^{8} \theta - 416 \cos^{6} \theta + 234 \cos^{4} \theta - 40 \cos^{2} \theta + 1\right) \sin3 \phi}{256 \sqrt{\pi}}\\
Y_{9,-2}(\theta, \phi)&=\frac{3 \sqrt{2090} \left(221 \cos^{8} \theta - 494 \cos^{6} \theta + 364 \cos^{4} \theta - 98 \cos^{2} \theta + 7\right) \sin2 \phi \cos \theta}{128 \sqrt{\pi}}\\
Y_{9,-1}(\theta, \phi)&=- \frac{3 \sqrt{95} \sin \theta \left(2431 \cos^{8} \theta - 4004 \cos^{6} \theta + 2002 \cos^{4} \theta - 308 \cos^{2} \theta + 7\right) \sin \phi}{256 \sqrt{\pi}}\\Y_{9, 0}(\theta, \phi)&=\frac{\sqrt{19} \left(12155 \cos^{8} \theta - 25740 \cos^{6} \theta + 18018 \cos^{4} \theta - 4620 \cos^{2} \theta + 315\right) \cos \theta}{256 \sqrt{\pi}}\\
Y_{9, 1}(\theta, \phi)&=\frac{3 \sqrt{95} \sin \theta \left(- 2431 \cos^{8} \theta + 4004 \cos^{6} \theta - 2002 \cos^{4} \theta + 308 \cos^{2} \theta - 7\right) \cos \phi}{256 \sqrt{\pi}}\\
Y_{9, 2}(\theta, \phi)&=\frac{3 \sqrt{2090} \left(- 221 \cos^{8} \theta + 494 \cos^{6} \theta - 364 \cos^{4} \theta + 98 \cos^{2} \theta - 7\right) \cos2 \phi \cos \theta}{128 \sqrt{\pi}}\\
Y_{9, 3}(\theta, \phi)&=\frac{\sqrt{43890} \sin \theta \left(221 \cos^{8} \theta - 416 \cos^{6} \theta + 234 \cos^{4} \theta - 40 \cos^{2} \theta + 1\right) \cos3 \phi}{256 \sqrt{\pi}}\\
Y_{9, 4}(\theta, \phi)&=\frac{3 \sqrt{95095} \left(17 \cos^{8} \theta - 44 \cos^{6} \theta + 38 \cos^{4} \theta - 12 \cos^{2} \theta + 1\right) \cos4 \phi \cos \theta}{128 \sqrt{\pi}}\\
Y_{9, 5}(\theta, \phi)&=\frac{3 \sqrt{5434} \sin \theta \left(- 85 \cos^{8} \theta + 200 \cos^{6} \theta - 146 \cos^{4} \theta + 32 \cos^{2} \theta - 1\right) \cos5 \phi}{256 \sqrt{\pi}}\\
Y_{9, 6}(\theta, \phi)&=- \frac{\sqrt{81510} \sin^{6} \theta \left(17 \cos^{2} \theta - 3\right) \cos6 \phi \cos \theta}{128 \sqrt{\pi}}\\
Y_{9, 7}(\theta, \phi)&=\frac{3 \sqrt{27170} \left(1 - 17 \cos^{2} \theta\right) \sin^{7} \theta \cos7 \phi}{512 \sqrt{\pi}}\\
Y_{9, 8}(\theta, \phi)&=\frac{3 \sqrt{230945} \sin^{8} \theta \cos8 \phi \cos \theta}{256 \sqrt{\pi}}\\
Y_{9, 9}(\theta, \phi)&=- \frac{\sqrt{461890} \sin^{9} \theta \cos9 \phi}{512 \sqrt{\pi}}\\
\end{align*}
$$
