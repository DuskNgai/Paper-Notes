# Image Formation Modeling in Cryogenic Electron Microscopy

## 1 Interaction Potential

为了描述由入射电子和样品组成的封闭系统，引入电子波函数的单体静态 Schroedinger 方程：
$$
\left[-\frac{\hbar^2}{2m}\nabla_{\mathbf{r}}^2+eV^{\text{int}}(\mathbf{r})\right](\psi_e)(\mathbf{r})=E_e\psi_e(\mathbf{r})
$$
$-\hbar^2/(2m)\nabla_r^2$ 是电子动能项，$eV^{\text{int}}$ 是电子势能项，$\psi_e$ 是入射电子波函数波函数，$E_e$ 是入射电子能量。当以下条件成立时，这个公式是合理的：

1. 磁场在样品厚度范围内近似恒定。
2. **连续入射电子可被视为独立事件。**
3. 相对效应可用非相对论的薛定谔方程（忽略电子自旋）与相对论波长和电子质量近似。
4. 这一现象与时间无关，即在电子过渡期间试样不会发生变化。
5. 假设入射电子具有高能量，则可以将多体薛定谔方程简化为单体薛定谔方程，并通过相互作用势来模拟试样的散射特性。

## 2 High-energy Electron-specimen Interaction

基于小角近似，入射电子与样品之间存在投影假设（Projection Assumption）和弱相位体近似（Weak-phase Object Approximation）。假设电子是 $z$ 方向入射，因此入射电子波函数波函数是：
$$
\psi_e(\mathbf{r})=\Psi(x,y,z)\exp(ikz)
$$
带入 Laplace 算子得到
$$
\begin{align*}
\nabla_{\mathbf{r}}^2\psi_e(\mathbf{r})&=(\nabla_{x,y}^2+\nabla_{z}^2)(\psi_e)(\mathbf{r})\\
&=[\nabla_{x,y}^2\Psi(x,y,z)]\exp(ikz)+\nabla_{z}^2[\Psi(x,y,z)\exp(ikz)]\\
&=[\nabla_{x,y}^2\Psi(x,y,z)]\exp(ikz)+\left[\frac{\partial^2}{\partial z^2}+2ik\frac{\partial}{\partial z}-k^2\right]\Psi(x,y,z)\exp(ikz)
\end{align*}
$$
其中 $k=\sqrt{2mE_e}/\hbar$，一并带入到 Schroedinger 方程得到：
$$
\begin{align*}
\left[-\frac{\hbar^2}{2m}\nabla_r^2+eV^{\text{int}}(\mathbf{r})\right](\psi_e)(\mathbf{r})&=E_e\psi_e(\mathbf{r})\\
\left[\nabla_r^2-\frac{2me}{\hbar^2}V^{\text{int}}(\mathbf{r})\right](\psi_e)(\mathbf{r})&=-\frac{2mE_e}{\hbar^2}\psi_e(\mathbf{r})\\
\left[\nabla_{x,y}^2+\frac{\partial^2}{\partial z^2}+2ik\frac{\partial}{\partial z}-\frac{2me}{\hbar^2}V^{\text{int}}(\mathbf{r})\right]\Psi(x,y,z)&=0
\end{align*}
$$
一开始波函数沿 $z$ 方向传播，散射后，产生了 $x,y$ 方向的分量，但是这部分的能量非常小，我们有：
$$
\left|\frac{\partial^2\Psi}{\partial z^2}\right|\ll \left|k\frac{\partial\Psi}{\partial z}\right|,k^2\gg k_x^2+k_y^2
$$
忽略 $\partial^2\Psi/\partial z^2$，得到
$$
\begin{align*}
2ik\frac{\partial}{\partial z}\Psi(x,y,z)&=\left[-\nabla_{x,y}^2+\frac{2me}{\hbar^2}V^{\text{int}}(\mathbf{r})\right]\Psi(x,y,z)\\
\frac{\partial\Psi(x,y,z)}{\partial z}&=\left[\frac{i\lambda}{4\pi}\nabla_{x,y}^2-\frac{ime}{\hbar^2k} V^{\text{int}}(\mathbf{r})\right]\Psi(x,y,z)\\
\frac{\partial\Psi(x,y,z)}{\partial z}&=\left[\frac{i\lambda}{4\pi}\nabla_{x,y}^2+\frac{2\pi m|e|\lambda}{h^2} V^{\text{int}}(\mathbf{r})\right]\Psi(x,y,z)\\
\end{align*}
$$
令 $\sigma=(2\pi m|e|\lambda)/h^2$，作为作用常数。

沿 $z$ 方向做切片积分，切片长度为 $\Delta{z}$，得到：
$$
\begin{align*}
\frac{\partial\Psi(x,y,z)}{\partial z}&=\left[\frac{i\lambda}{4\pi}\nabla_{x,y}^2+i\sigma V^{\text{int}}(\mathbf{r})\right]\Psi(x,y,z)\\
\int_{z}^{z+\Delta{z}}\frac{\partial\Psi(x,y,z)}{\Psi(x,y,z)}&=\int_{z}^{z+\Delta{z}}\left[\frac{i\lambda}{4\pi}\nabla_{x,y}^2+i\sigma V^{\text{int}}(\mathbf{r})\right]\partial z\\
\ln\Psi(x,y,z+\Delta{z})-\ln\Psi(x,y,z)&=\left[\frac{i\lambda}{4\pi}\nabla_{x,y}^2\Delta{z}+i\sigma\int_{z}^{z+\Delta{z}}V^{\text{int}}(x,y,z')\mathrm{d}z'\right]\\
\Psi(x,y,z+\Delta{z})&=\exp\left[\frac{i\lambda}{4\pi}\nabla_{x,y}^2\Delta{z}+i\sigma\int_{z}^{z+\Delta{z}}V^{\text{int}}(x,y,z')\mathrm{d}z'\right]\Psi(x,y,z)\\
&=\exp\left[\frac{i\lambda}{4\pi}\nabla_{x,y}^2\Delta{z}\right]\exp\left[i\sigma\int_{z}^{z+\Delta{z}}V^{\text{int}}(x,y,z')\mathrm{d}z'\right]\Psi(x,y,z)
\end{align*}
$$
定义 $t(x,y,z)=\exp\left[i\sigma\int_{z}^{z+\Delta{z}}V^{\text{int}}(x,y,z')\mathrm{d}z'\right]$ 描述了切片内的传输函数（相位光栅）。对它作 Fourier 变换：
$$
\mathcal{F}\left[\exp\left[\frac{i\lambda}{4\pi}\nabla_{x,y}^2\Delta{z}\right]t(x,y,z)\Psi(x,y,z)\right]=\exp\left[-i\pi\lambda\Delta{z}(q_x^2+q_y^2)\right]\mathcal{F}\left[t(x,y,z)\Psi(x,y,z)\right]
$$
其中 $q_x,q_y$ 是频域的坐标系，其中对 $\exp$ 算子的 Fourier 变换可以看作先 Taylor 展开分开做 Fourier 变换，再合并 Taylor 级数。定义 $P(q,\Delta{z})=\exp\left[-i\pi\lambda\Delta{z}(q_x^2+q_y^2)\right]$ 为 Fresnel 传播子，因此前 $n+1$ 个切片对于相位的贡献为：
$$
\Psi_{n+1}(x,y)=\mathcal{F}^{-1}[P_{n}(q,\Delta{z})\mathcal{F}[t_n(x,y)\Psi_n(x,y)]]
$$
形成了一个递推形式，因此可以迭代计算。但 $\Delta{z}$ 最好不要取得比电子波长小。

投影假设（PA）通常用于薄物体，即入射波为平面波时，透射波函数等于传输函数：
$$
\Psi_{\text{exit}}(x,y)=t(x,y,z)\Psi_{\text{plane}}(x,y)=t(x,y,z)=\exp\left[i\sigma\int_{z}^{z+\Delta{z}}V^{\text{int}}(x,y,z')\mathrm{d}z'\right]
$$
如果散射很弱，尤其是在轻原子的情况下，则可以使用弱相物体近似（WPAO）。定义 $V_{z}(x,y,z)=\int_{z}^{z+\Delta{z}}V^{\text{int}}(x,y,z')\mathrm{d}z'$。距离 z 处的波函数变为：
$$
\Psi(x,y,z)=\exp(i\sigma V_{z}(x,y,z))\approx1+i\sigma V_{z}(x,y,z)
$$
这个信号最后就会和 CTF 在频域相乘了：
$$
\text{CTF}(q)\sim\exp[-i\pi\lambda q^2(0.5C_s\lambda^2 q^2-\Delta{f})]
$$

## 3 Optical System

在低温电子显微镜中，我们记录的主要是相位对比产生的图像，这是电子出口波函数的非散射部分和散射部分之间干涉的结果（相差显微镜的基本原理）。从样品中出射的电子波还会受到显微镜像差带来的频率相关相移的影响。如果考虑到球差 $C_s$、散焦 $\Delta{f}$ 和两折散光 $(A_1,\alpha_1)$，极坐标下的总像差函数为
$$
\chi(q,\alpha)=\frac{2\pi}{\lambda}\left(\frac{1}{4}C_s\lambda^4q^4-\frac{1}{2}(\Delta{f}-A_1\cos(2(\alpha-\alpha_1)))\lambda^2q^2\right)
$$
其中 $q=\sqrt{q_x^2+q_y^2}$，$\Delta{f}>0$ 表示欠焦。

入射电子的能量扩散和有限的虚拟光源尺寸分别导致了时间和空间上的不一致性。这些都可以用空间频率域的阻尼包络来模拟。光源的时间不一致性可以用色度包络函数来模拟：
$$
K_c(q)=\exp\left[-\left(\frac{\pi\lambda qC_c\Delta{E}}{4E\sqrt{\ln2}}\right)^2\right]
$$
$C_c$ 是色差系数，$E$ 是能量，$\Delta{E}$ 是入射电子的能量散布。此外，有限的信号源尺寸会带来空间不一致性，从而导致空间包络函数：
$$
K_s(q,\alpha)=\exp\left[-\frac{(\pi C_s\lambda^2q^3-\pi\Delta{f}(\alpha)q)^2\alpha_i^2}{\ln2}\right]
$$
$\alpha_i$ 是照明孔径。物镜孔径函数（可选择包括相位板 $A_p (q)$）描述如下：
$$
A_p(q)=\begin{cases}
1&q\le q_{\text{cuton}}\\
\exp(i\pi/2)&q_{\text{cuton}}<q\le q_{\text{cutof}}\\
0&q> q_{\text{cutoff}}\\
\end{cases}
$$
因此最后的 CTF 是：
$$
T(q,\alpha)=K_s(q,\alpha)K_c(q)A_p(q)\exp(-i\chi(q, \alpha))
$$
最后在后焦面的电子波函数的 Fourier 变换就是：
$$
\mathcal{F}[\Psi(x,y)]T(q,\alpha)
$$

## 4 Detector Response

最终图像的实现需要通过照相机的检测将电子波函数转换为数字信号。这种照相机具有多种特性，如以 [ADU/e-] 为单位的照相机转换系数 CF、各种噪声源以及频率响应测量，如调制传递函数（MTF）和探测量子效率（DQE）。测量过程服从泊松统计，会产生拍摄噪声，在最终图像中增加读出噪声 $I_{\text{rn}}$ 和暗电流 $I_{\text{dc}}$，并通过探测器点扩散函数 $\text{PSF}(x, y)$ 模糊图像：
$$
I(x,y)=[\text{CF}\cdot\text{Poisson}(NI_0(x,y))]*\text{PSF}(x,y)+I_{\text{rn}}+I_{\text{dc}}
$$
$\text{Poisson}(NI_0(x,y))$ 是一个 Poisson 噪声，$NI_0(x,y)$ 是入射集成电子通量 [e-/area]。

泊松（射击）噪声与电子电流（或光）测量的不确定性有关，这是电子的离散性和电子检测的独立性所固有的。其方差与信号有关，是图像噪声的主要来源。对于较高的综合电子通量，泊松分布接近于正态（高斯）分布。然而，在冷冻显微镜下，为了避免辐射损伤和保留试样的结构细节，综合电子通量要保持在非常低的水平。因此，通过泊松分布建立噪声模型非常重要。信号通常由像素尺寸为 $\Delta{x}$ 和 $\Delta{y}$ 的像素化探测器（CCD 或 CMOS 相机）捕获。因此，像素 $i$ 检测到电子的概率为 $P_i = P(x_i, y_i) = |\Psi(x, y)|^2 \Delta{x}_i\Delta{y}_i$。$I(x,y)$ 也有：
$$
I(x,y)=\mathcal{F}^{-1}[\mathcal{F}[\text{CF}\cdot\text{Poisson}(NI_0(x,y))]\cdot\text{MTF}(q)]
$$
MTF 描述了信号振幅在不同空间频率下的传输方式。它是探测器 PSF 的傅立叶变换模数。仅 MTF 的衰减不会破坏图像质量。如果信号传输到奈奎斯特频率，并且 MTF 已知，理论上就可以通过解卷积恢复图像。实际上，解卷积会受到噪声的影响。DQE 描述了探测器增加的噪声。随机散射过程的噪声（如 TEM 检测器中的噪声）与信号的传输方式不同[28]。检测到的信号中的噪声不仅仅是被 MTF 衰减的输入信号中的噪声[20]。DQE 被定义为输出信号与输入信号之间信噪比（SNR）的平方比：
$$
\text{DQR}(q)=\left(\frac{\text{SNR}_{\text{out}}}{\text{SNR}_{\text{in}}}\right)^2
$$
有文献说
$$
\text{DQE}(q)=\text{CF}^2N\frac{\text{MTF}^2(q)}{\text{NPS}_{\text{out}}(q)}
$$
NPS 是噪声功率谱。定义噪声传递函数 NTF：
$$
\text{NTF}(q)=\frac{\text{NPS}_{\text{out}}(q)}{\text{CF}^2N}
$$
因此 DQE：
$$
\text{DQE}(q)=\frac{\text{MTF}^2(q)}{\text{NTF}^2(q)}
$$
我们面临的问题是如何分别处理信号和噪声的传递，因为泊松（射击）噪声取决于信号。我们采用以下方法将两者分离：1）将无噪声信号的傅立叶频谱与信号（MTF）和噪声（NTF）传输之间的比值进行阻尼（相乘）；2）将该信号与集成电子通量相乘，并加入所有噪声贡献；3）将该（噪声）信号的傅立叶频谱与 NTF 进行阻尼；4）将电子数与图像灰度值进行 CF 缩放。因此，我们可以将检测到的图像写成
$$
I(x,y)=\mathcal{F}^{-1}\left[\mathcal{F}\left[\text{CF}\cdot\text{Poisson}\left(N\mathcal{F}^{-1}\left[\mathcal{F}[I_0(x,y)]\sqrt{\text{DQE}(q)}\right]\right)\right]\cdot\text{MTF}(q)\right]
$$
