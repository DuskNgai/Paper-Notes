# k-space propagation models for acoustically heterogeneous media Application to biomedical photoacoustics

## 2 Forward Models In Photoacoustics

### 2.1 Photoacoustic Wave Equation

|       Symbols        |                   Descriptions                    |
| :------------------: | :-----------------------------------------------: |
|  $p(\mathbf{x},t)$   |            the acoustic pressure field            |
|   $c(\mathbf{x})$    |                the speed of sound                 |
|  $\rho(\mathbf{x})$  |          the mass density of the medium           |
| $\Gamma(\mathbf{x})$ |      the dimensionless Grueneisen parameter       |
|  $\mathcal{H}(r,t)$  | the heat energy per unit volume and per unit time |

$$
\frac{\partial^2{p}}{\partial{t}^2}-c^2\rho\nabla\cdot\left(\frac{1}{\rho}\nabla{p}\right)=\Gamma\frac{\partial\mathcal{H}}{\partial{t}}\tag{1}
$$

If $c(\mathbf{x})=c_0$ and $\rho(\mathbf{x})=\rho_0$:
$$
\left(\frac{\partial^2}{\partial{t}^2}-c_0^2\nabla^2\right){p}=\Gamma\frac{\partial\mathcal{H}}{\partial{t}}\tag{2}
$$
If $\mathcal{H}(\mathbf{x},t)$ is stationary, that is $\mathcal{H}(\mathbf{x},t)=H_x(\mathbf{x})H_t(t)$. If the heating pulse is assumed Gaussian, then:
$$
H_t(t)=\frac{e^{-(t/\tau)^2}}{\tau\sqrt{\pi}}\tag{3}
$$
As $\tau\to0$, $H_t(t)\to\delta(t)$. The forward problem reduces to the initial value problem for the homogeneous wave equation
$$
\left(\frac{\partial^2}{\partial{t}^2}-c_0^2\nabla^2\right){p}=0\tag{4}
$$
with initial value:
$$
p|_{t=0}=\Gamma H_x\quad\partial{p}/\partial{t}|_{t=0}=0\tag{5}
$$

### 2.2 Model Based on Poisson's Solution

$$
p(\mathbf{x},t)=\frac{\Gamma}{4\pi c_0}\frac{\partial}{\partial{t}}\int_{A(t)}\frac{H_x(\mathbf{x}')}{|\mathbf{x}-\mathbf{x}'|}\mathrm{d}A\tag{6}
$$

$A(t)$ is the surface on which $|\mathbf{x}-\mathbf{x}'|=c_0t$.





