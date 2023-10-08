# Matrix Differentiation

## Overview

Scalars are small letter $a$; Vectors are small bold letter $\mathbf{a}$; Matrix are capital bold letter $\mathbf{A}$.

### Types of Matrix Derivatives

| Types  |                   Scalar                    |                        Vector                        |                   Matrix                    |
| :----: | :-----------------------------------------: | :--------------------------------------------------: | :-----------------------------------------: |
| Scalar |     $\dfrac{\mathrm{d}y}{\mathrm{d}x}$      |     $\dfrac{\mathrm{d}\mathbf{y}}{\mathrm{d}x}$      | $\dfrac{\mathrm{d}\mathbf{Y}}{\mathrm{d}x}$ |
| Vector | $\dfrac{\partial{y}}{\partial{\mathbf{x}}}$ | $\dfrac{\partial{\mathbf{y}}}{\partial{\mathbf{x}}}$ |                                             |
| Matrix | $\dfrac{\partial{y}}{\partial{\mathbf{X}}}$ |                                                      |                                             |

### Layout Notation

**分母布局 Numerator layout:**
$$
\frac{\mathrm{d}y}{\mathrm{d}x}\quad
\frac{\mathrm{d}\mathbf{y}}{\mathrm{d}x}=\begin{bmatrix}\dfrac{\mathrm{d}y_1}{\mathrm{d}x}\\\vdots\\\dfrac{\mathrm{d}y_m}{\mathrm{d}x}\end{bmatrix}\quad
\frac{\mathrm{d}\mathbf{Y}}{\mathrm{d}x}=\begin{bmatrix}\dfrac{\mathrm{d}y_{11}}{\mathrm{d}x}&\cdots&\dfrac{\mathrm{d}y_{1n}}{\mathrm{d}x}\\\vdots&\ddots&\vdots\\\dfrac{\mathrm{d}y_{m1}}{\mathrm{d}x}&\cdots&\dfrac{\mathrm{d}y_{mn}}{\mathrm{d}x}\end{bmatrix}
$$

$$
\frac{\partial{y}}{\partial{\mathbf{x}}}=\begin{bmatrix}\dfrac{\partial{y}}{\partial{x_1}}&\cdots&\dfrac{\partial{y}}{\partial{x_n}}\end{bmatrix}\quad
\frac{\partial{\mathbf{y}}}{\partial{\mathbf{x}}}=\begin{bmatrix}\dfrac{\partial{y_1}}{\partial{x_1}}&\cdots&\dfrac{\partial{y_1}}{\partial{x_n}}\\\vdots&\ddots&\vdots\\\dfrac{\partial{y_m}}{\partial{x_1}}&\cdots&\dfrac{\partial{y_m}}{\partial{x_n}}\end{bmatrix}
$$

$$
\frac{\partial{y}}{\partial{\mathbf{X}}}=\begin{bmatrix}\dfrac{\partial{y}}{\partial{x_{11}}}&\cdots&\dfrac{\partial{y}}{\partial{x_{m1}}}\\\vdots&\ddots&\vdots\\\dfrac{\partial{y}}{\partial{x_{1n}}}&\cdots&\dfrac{\partial{y}}{\partial{x_{mn}}}\end{bmatrix}
$$

分子布局 Denominator layout:
$$
\frac{\mathrm{d}y}{\mathrm{d}x}\quad
\frac{\mathrm{d}\mathbf{y}}{\mathrm{d}x}=\begin{bmatrix}\dfrac{\mathrm{d}y_1}{\mathrm{d}x}&\cdots&\dfrac{\mathrm{d}y_m}{\mathrm{d}x}\end{bmatrix}
$$

$$
\frac{\partial{y}}{\partial{\mathbf{x}}}=\begin{bmatrix}\dfrac{\partial{y}}{\partial{x_1}}\\\vdots\\\dfrac{\partial{y}}{\partial{x_n}}\end{bmatrix}\quad
\frac{\partial{\mathbf{y}}}{\partial{\mathbf{x}}}=\begin{bmatrix}\dfrac{\partial{y_1}}{\partial{x_1}}&\cdots&\dfrac{\partial{y_m}}{\partial{x_1}}\\\vdots&\ddots&\vdots\\\dfrac{\partial{y_1}}{\partial{x_n}}&\cdots&\dfrac{\partial{y_m}}{\partial{x_n}}\end{bmatrix}
$$

$$
\frac{\partial{y}}{\partial{\mathbf{X}}}=\begin{bmatrix}\dfrac{\partial{y}}{\partial{x_{11}}}&\cdots&\dfrac{\partial{y}}{\partial{x_{1n}}}\\\vdots&\ddots&\vdots\\\dfrac{\partial{y}}{\partial{x_{m1}}}&\cdots&\dfrac{\partial{y}}{\partial{x_{mn}}}\end{bmatrix}
$$

## Consequence

**Default to numerator layout.** Here $a$, $\mathbf{a}$, $\mathbf{A}$ are not functions of $x$ and $\mathbf{x}$.

### Scalar by Scalar

| Scalar by Scalar                                             |
| :----------------------------------------------------------- |
| $\dfrac{\mathrm{d}(\mathbf{u}^T\mathbf{v})}{\mathrm{d}x}=\dfrac{\mathrm{d}(\mathbf{v}^T\mathbf{u})}{\mathrm{d}x}=\mathbf{v}^T\dfrac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}+\mathbf{u}^T\dfrac{\mathrm{d}\mathbf{v}}{\mathrm{d}x}$ |
| $\dfrac{\mathrm{d}(\mathbf{u}^T\mathbf{A}\mathbf{v})}{\mathrm{d}x}=\mathbf{v}^T\mathbf{A}^T\dfrac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}+\mathbf{u}^T\mathbf{A}\dfrac{\mathrm{d}\mathbf{v}}{\mathrm{d}x}$ |

### Vector by Scalar

| Vector by Scalar                                             |
| :----------------------------------------------------------- |
| $\dfrac{\mathrm{d}\mathbf{a}}{\mathrm{d}x}=\mathbf{0}$       |
| $\dfrac{\mathrm{d}(a_1\mathbf{u}+a_2\mathbf{v})}{\mathrm{d}x}=a_1\dfrac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}+a_2\dfrac{\mathrm{d}\mathbf{v}}{\mathrm{d}x}$ |
| $\dfrac{\mathrm{d}\mathbf{u}^T}{\mathrm{d}x}=\left(\dfrac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}\right)^T$ |
| $\dfrac{\mathrm{d}\mathbf{A}\mathbf{u}}{\mathrm{d}x}=\mathbf{A}\dfrac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}$ |
| $\dfrac{\mathrm{d}\mathbf{g}(\mathbf{u})}{\mathrm{d}x}=\dfrac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{u}}\dfrac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}$ |
| $\dfrac{\mathrm{d}\mathbf{f}(\mathbf{g}(\mathbf{u}))}{\mathrm{d}x}=\dfrac{\partial\mathbf{f}(\mathbf{g})}{\partial\mathbf{g}}\dfrac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{u}}\dfrac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}$ |

### Matrix by Scalar

| Matrix by Scalar                                             |
| ------------------------------------------------------------ |
| $\dfrac{\mathrm{d}\mathbf{A}}{\mathrm{d}x}=\mathbf{0}$       |
| $\dfrac{\mathrm{d}(a_1\mathbf{U}+a_2\mathbf{V})}{\mathrm{d}x}=a_1\dfrac{\mathrm{d}\mathbf{U}}{\mathrm{d}x}+a_2\dfrac{\mathrm{d}\mathbf{V}}{\mathrm{d}x}$ |
| $\dfrac{\mathrm{d}(\mathbf{U}\mathbf{V})}{\mathrm{d}x}=\mathbf{V}\dfrac{\mathrm{d}\mathbf{U}}{\mathrm{d}x}+\mathbf{U}\dfrac{\mathrm{d}\mathbf{V}}{\mathrm{d}x}$ |
| $\dfrac{\mathrm{d}(\mathbf{A}\mathbf{U}\mathbf{B})}{\mathrm{d}x}=\mathbf{A}\dfrac{\mathrm{d}\mathbf{U}}{\mathrm{d}x}\mathbf{B}$ |

### Scalar by Vector

| Scalar by Vector                                             |
| ------------------------------------------------------------ |
| $\dfrac{\partial{a}}{\partial\mathbf{x}}=\mathbf{0}^T$       |
| $\dfrac{\partial(a_1u+a_2v)}{\partial\mathbf{x}}=a_1\dfrac{\partial{u}}{\partial\mathbf{x}}+a_2\dfrac{\partial{v}}{\partial\mathbf{x}}$ |
| $\dfrac{\partial{uv}}{\partial\mathbf{x}}=u\dfrac{\partial{v}}{\partial\mathbf{x}}+v\dfrac{\partial{u}}{\partial\mathbf{x}}$ |
| $\dfrac{\partial{g(u)}}{\partial{\mathbf{x}}}=\dfrac{\mathrm{d}g(u)}{\mathrm{d}u}\dfrac{\partial{u}}{\partial{\mathbf{x}}}$ |
| $\dfrac{\partial{f(g(u))}}{\partial{\mathbf{x}}}=\dfrac{\mathrm{d}f(g)}{\mathrm{d}g}\dfrac{\mathrm{d}g(u)}{\mathrm{d}u}\dfrac{\partial{u}}{\partial{\mathbf{x}}}$ |
| $\dfrac{\partial\mathbf{u}^T\mathbf{v}}{\partial\mathbf{x}}=\mathbf{u}^T\dfrac{\partial\mathbf{v}}{\partial\mathbf{x}}+\mathbf{v}^T\dfrac{\partial\mathbf{u}}{\partial\mathbf{x}}$ => $\dfrac{\partial\mathbf{a}^T\mathbf{x}}{\partial\mathbf{x}}=\dfrac{\partial\mathbf{x}^T\mathbf{a}}{\partial\mathbf{x}}=\mathbf{a}^T$ / $\dfrac{\partial\mathbf{x}^T\mathbf{x}}{\partial\mathbf{x}}=2\mathbf{x}^T$ |
| $\dfrac{\partial\mathbf{u}^T\mathbf{A}\mathbf{v}}{\partial\mathbf{x}}=\mathbf{u}^T\mathbf{A}\dfrac{\partial\mathbf{v}}{\partial\mathbf{x}}+\mathbf{v}^T\mathbf{A}^T\dfrac{\partial\mathbf{u}}{\partial\mathbf{x}}$ => $\dfrac{\partial\mathbf{x}^T\mathbf{A}\mathbf{x}}{\partial\mathbf{x}}=\mathbf{x}^T(\mathbf{A}+\mathbf{A}^T)$ |

### Vector by Vector

| Vector by Vector                                             |
| ------------------------------------------------------------ |
| $\dfrac{\partial{\mathbf{a}}}{\partial\mathbf{x}}=\mathbf{0}$ (matrix) |
| $\dfrac{\partial{u\mathbf{v}}}{\partial\mathbf{x}}=u\dfrac{\partial{\mathbf{v}}}{\partial\mathbf{x}}+\mathbf{v}\dfrac{\partial{u}}{\partial\mathbf{x}}$ |
| $\dfrac{\partial\mathbf{Au}}{\partial\mathbf{x}}=\mathbf{A}\dfrac{\partial\mathbf{u}}{\partial\mathbf{x}}$ => $\dfrac{\partial{\mathbf{Ax}}}{\partial\mathbf{x}}=\mathbf{A}$ / $\dfrac{\partial{\mathbf{x}}}{\partial\mathbf{x}}=\mathbf{I}$ |
| $\dfrac{\partial{\mathbf{x}^T\mathbf{A}}}{\partial\mathbf{x}}=\mathbf{A}^T$ |
| $\dfrac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{x}}=\dfrac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{u}}\dfrac{\partial\mathbf{u}}{\partial\mathbf{x}}$ |
| $\dfrac{\partial\mathbf{f}(\mathbf{g}(\mathbf{u}))}{\partial\mathbf{x}}=\dfrac{\partial\mathbf{f}(\mathbf{g})}{\partial\mathbf{g}}\dfrac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{u}}\dfrac{\partial\mathbf{u}}{\partial\mathbf{x}}$ |

### Scalar by Matrix

| Scalar by Matrix                                             |
| ------------------------------------------------------------ |
| $\dfrac{\partial{a}}{\partial\mathbf{X}}=\mathbf{0}$ (matrix) |
| $\dfrac{\partial(a_1u+a_2v)}{\partial\mathbf{X}}=a_1\dfrac{\partial{u}}{\partial\mathbf{X}}+a_2\dfrac{\partial{v}}{\partial\mathbf{X}}$ |
| $\dfrac{\partial{g(u)}}{\partial\mathbf{X}}=\dfrac{\mathrm{d}g(u)}{\mathrm{d}u}\dfrac{\partial{u}}{\partial\mathbf{X}}$ |
| $\dfrac{\partial{f(g(u))}}{\partial\mathbf{X}}=\dfrac{\mathrm{d}f(g)}{\mathrm{d}g}\dfrac{\mathrm{d}g(u)}{\mathrm{d}u}\dfrac{\partial{u}}{\partial\mathbf{X}}$ |

## Step by Step Deductions

### Scalar by Scalar

1. $$
   \begin{align*}
   \frac{\mathrm{d}(\mathbf{u}^T\mathbf{v})}{\mathrm{d}x}&=\dfrac{\mathrm{d}(\mathbf{v}^T\mathbf{u})}{\mathrm{d}x}=\frac{\mathrm{d}\sum_{i}u_iv_i}{\mathrm{d}x}\\
   &=\sum_{i}v_i\frac{\mathrm{d}u_i}{\mathrm{d}x}+\sum_{i}u_i\frac{\mathrm{d}v_i}{\mathrm{d}x}\\
   &=\mathbf{v}^T\frac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}+\mathbf{u}^T\frac{\mathrm{d}\mathbf{v}}{\mathrm{d}x}
   \end{align*}
   $$

2. $$
   \begin{align*}
   \frac{\mathrm{d}(\mathbf{u}^T\mathbf{A}\mathbf{v})}{\mathrm{d}x}&=\frac{\mathrm{d}\sum_{i}\sum_{j}u_ia_{ij}v_j}{\mathrm{d}x}\\
   &=\sum_{i}\sum_{j}a_{ij}v_j\frac{\mathrm{d}u_i}{\mathrm{d}x}+\sum_{i}\sum_{j}u_ia_{ij}\frac{\mathrm{d}v_j}{\mathrm{d}x}\\
   &=\sum_{j}\sum_{i}v_ja_{ji}\frac{\mathrm{d}u_i}{\mathrm{d}x}+\sum_{i}\sum_{j}u_ia_{ij}\frac{\mathrm{d}v_j}{\mathrm{d}x}\\
   &=\mathbf{v}^T\mathbf{A}^T\frac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}+\mathbf{u}^T\mathbf{A}\frac{\mathrm{d}\mathbf{v}}{\mathrm{d}x}
   \end{align*}
   $$

### Vector by Scalar

1. $$
   \left[\frac{\mathrm{d}\mathbf{a}}{\mathrm{d}x}\right]_{i}=\left[\frac{\mathrm{d}a_i}{\mathrm{d}x}\right]_{i}=\left[0\right]_{i}\Longrightarrow\frac{\mathrm{d}\mathbf{a}}{\mathrm{d}x}=\mathbf{0}
   $$

2. $$
   \begin{align*}
   \left[\frac{\mathrm{d}(a_1\mathbf{u}+a_2\mathbf{v})}{\mathrm{d}x}\right]_{i}&=\left[\frac{\mathrm{d}(a_1u_i+a_2v_i)}{\mathrm{d}x}\right]_{i}=\left[a_1\frac{\mathrm{d}u_i}{\mathrm{d}x}+a_2\frac{\mathrm{d}v_i}{\mathrm{d}x}\right]_{i}\\
   &=a_1\left[\frac{\mathrm{d}u_i}{\mathrm{d}x}\right]_{i}+a_2\left[\frac{\mathrm{d}v_i}{\mathrm{d}x}\right]_{i}\\
   &\Longrightarrow\frac{\mathrm{d}(a_1\mathbf{u}+a_2\mathbf{v})}{\mathrm{d}x}=a_1\dfrac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}+a_2\dfrac{\mathrm{d}\mathbf{v}}{\mathrm{d}x}
   \end{align*}
   $$

3. $$
   \frac{\mathrm{d}\mathbf{u}^T}{\mathrm{d}x}=\begin{bmatrix}\dfrac{\mathrm{d}y_1}{\mathrm{d}x}&\cdots&\dfrac{\mathrm{d}y_m}{\mathrm{d}x}\end{bmatrix}=\begin{bmatrix}\dfrac{\mathrm{d}y_1}{\mathrm{d}x}\\\vdots\\\dfrac{\mathrm{d}y_m}{\mathrm{d}x}\end{bmatrix}^T=\left(\frac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}\right)^T
   $$

4. $$
   \left[\frac{\mathrm{d}\mathbf{A}\mathbf{u}}{\mathrm{d}x}\right]_i=\left[\frac{\mathrm{d}(a_{i1}u_1+\cdots+a_{im}u_m)}{\mathrm{d}x}\right]_i=\left[a_{11}\dfrac{\mathrm{d}u_1}{\mathrm{d}x}+\cdots+a_{1m}\dfrac{\mathrm{d}u_m}{\mathrm{d}x}\right]_i\\
   \Longrightarrow
   \frac{\mathrm{d}\mathbf{A}\mathbf{u}}{\mathrm{d}x}=\mathbf{A}\frac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}
   $$

5. $$
   \left[\frac{\mathrm{d}\mathbf{g}(\mathbf{u})}{\mathrm{d}x}\right]_{i}=\left[\frac{\mathrm{d}g_i(\mathbf{u})}{\mathrm{d}x}\right]_{i}=\left[\sum_{j}\frac{\mathrm{d}g_i}{\mathrm{d}u_j}\frac{\mathrm{d}u_j}{\mathrm{d}x}\right]_{i}\\
   \Longrightarrow
   \frac{\mathrm{d}\mathbf{g}(\mathbf{u})}{\mathrm{d}x}=\frac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{u}}\frac{\mathrm{d}\mathbf{u}}{\mathrm{d}x}
   $$

### Matrix by Scalar

1. $$
   \left[\frac{\mathrm{d}\mathbf{A}}{\mathrm{d}x}\right]_{ij}=\left[\frac{\mathrm{d}a_{ij}}{\mathrm{d}x}\right]_{ij}=\left[\frac{\mathrm{d}0}{\mathrm{d}x}\right]_{ij}\Longrightarrow\frac{\mathrm{d}\mathbf{A}}{\mathrm{d}x}=\mathbf{0}
   $$

2. $$
   \begin{align*}
   \left[\frac{\mathrm{d}(a_1\mathbf{U}+a_2\mathbf{V})}{\mathrm{d}x}\right]_{ij}&=\left[\frac{\mathrm{d}(a_1u_{ij}+a_2v_{ij})}{\mathrm{d}x}\right]_{ij}=\left[a_1\frac{\mathrm{d}u_{ij}}{\mathrm{d}x}+a_2\frac{\mathrm{d}v_{ij}}{\mathrm{d}x}\right]_{ij}\\
   &\Longrightarrow\frac{\mathrm{d}(a_1\mathbf{U}+a_2\mathbf{V})}{\mathrm{d}x}=a_1\frac{\mathrm{d}\mathbf{U}}{\mathrm{d}x}+a_2\frac{\mathrm{d}\mathbf{V}}{\mathrm{d}x}
   \end{align*}
   $$

3. $$
   \begin{align*}
   \left[\frac{\mathrm{d}(\mathbf{U}\mathbf{V})}{\mathrm{d}x}\right]_{ij}&=\left[\frac{\mathrm{d}(u_{i1}v_{1j}+\cdots+u_{in}v_{nj})}{\mathrm{d}x}\right]_{ij}\\
   &=\left[u_{i1}\frac{\mathrm{d}v_{1j}}{\mathrm{d}x}+v_{i1}\frac{\mathrm{d}u_{1j}}{\mathrm{d}x}+\cdots+u_{in}\frac{\mathrm{d}v_{nj}}{\mathrm{d}x}+v_{in}\frac{\mathrm{d}u_{nj}}{\mathrm{d}x}\right]_{ij}\\
   &=\left[u_{i1}\frac{\mathrm{d}v_{1j}}{\mathrm{d}x}+\cdots+u_{in}\frac{\mathrm{d}v_{nj}}{\mathrm{d}x}+v_{i1}\frac{\mathrm{d}u_{1j}}{\mathrm{d}x}+\cdots+v_{in}\frac{\mathrm{d}u_{nj}}{\mathrm{d}x}\right]_{ij}\\
   &=\left[u_{i1}\frac{\mathrm{d}v_{1j}}{\mathrm{d}x}+\cdots+u_{in}\frac{\mathrm{d}v_{nj}}{\mathrm{d}x}\right]_{ij}+\left[mv_{i1}\frac{\mathrm{d}u_{1j}}{\mathrm{d}x}+\cdots+v_{in}\frac{\mathrm{d}u_{nj}}{\mathrm{d}x}\right]_{ij}\\
   &\Longrightarrow\frac{\mathrm{d}(\mathbf{U}\mathbf{V})}{\mathrm{d}x}=\mathbf{V}\frac{\mathrm{d}\mathbf{U}}{\mathrm{d}x}+\mathbf{U}\frac{\mathrm{d}\mathbf{V}}{\mathrm{d}x}
   \end{align*}
   $$

4. $$
   \begin{align*}
   \left[\frac{\mathrm{d}(\mathbf{A}\mathbf{U}\mathbf{B})}{\mathrm{d}x}\right]_{ij}&=\left[\frac{\mathrm{d}(\sum_{k}\sum_{l}a_{ik}u_{kl}b_{lj})}{\mathrm{d}x}\right]_{ij}\\
   &=\left[\frac{\mathrm{d}(\sum_{k}\sum_{l}a_{ik}u_{kl}b_{lj})}{\mathrm{d}x}\right]_{ij}\\
   &=\left[\sum_{k}\sum_{l}a_{ik}\frac{\mathrm{d}u_{kl}}{\mathrm{d}x}b_{lj}\right]_{ij}\\
   &\Longrightarrow\frac{\mathrm{d}(\mathbf{A}\mathbf{U}\mathbf{B})}{\mathrm{d}x}=\mathbf{A}\frac{\mathrm{d}\mathbf{U}}{\mathrm{d}x}\mathbf{B}
   \end{align*}
   $$

### Scalar by Vector

1. $$
   \frac{\partial{a}}{\partial\mathbf{x}}=\begin{bmatrix}\dfrac{\partial{a}}{\partial{x_1}}&\cdots&\dfrac{\partial{a}}{\partial{x_n}}\end{bmatrix}=\begin{bmatrix}0&\cdots&0\end{bmatrix}=\mathbf{0}^T
   $$

2. $$
   \begin{align*}
   \frac{\partial(a_1u+a_2v)}{\partial\mathbf{x}}&=\begin{bmatrix}\dfrac{\partial(a_1u+a_2v)}{\partial{x_1}}&\cdots&\dfrac{\partial(a_1u+a_2v)}{\partial{x_n}}\end{bmatrix}\\
   &=\begin{bmatrix}a_1\dfrac{\partial{v}}{\partial{x_1}}+a_2\dfrac{\partial{u}}{\partial{x_1}}&\cdots&a_1\dfrac{\partial{v}}{\partial{x_n}}+a_2\dfrac{\partial{u}}{\partial{x_n}}\end{bmatrix}\\
   &=a_1\frac{\partial{u}}{\partial\mathbf{x}}+a_2\frac{\partial{v}}{\partial\mathbf{x}}
   \end{align*}
   $$

3. $$
   \begin{align*}
   \frac{\partial{uv}}{\partial\mathbf{x}}&=\begin{bmatrix}\dfrac{\partial{uv}}{\partial{x_1}}&\cdots&\dfrac{\partial{uv}}{\partial{x_n}}\end{bmatrix}\\
   &=\begin{bmatrix}u\dfrac{\partial{v}}{\partial{x_1}}+v\dfrac{\partial{u}}{\partial{x_1}}&\cdots&u\dfrac{\partial{v}}{\partial{x_n}}+v\dfrac{\partial{u}}{\partial{x_n}}\end{bmatrix}\\
   &=u\frac{\partial{v}}{\partial\mathbf{x}}+v\frac{\partial{u}}{\partial\mathbf{x}}
   \end{align*}
   $$

4. $$
   \begin{align*}
   \frac{\partial{g(u)}}{\partial\mathbf{x}}&=\begin{bmatrix}\dfrac{\partial{g(u)}}{\partial{x_1}}&\cdots&\dfrac{\partial{g(u)}}{\partial{x_n}}\end{bmatrix}\\
   &=\begin{bmatrix}\dfrac{\mathrm{d}g(u)}{\mathrm{d}{u}}\dfrac{\partial{u}}{\partial{x_1}}&\cdots&\dfrac{\mathrm{d}g(u)}{\mathrm{d}{u}}\dfrac{\partial{u}}{\partial{x_n}}\end{bmatrix}\\
   &=\frac{\mathrm{d}g(u)}{\mathrm{d}{u}}\begin{bmatrix}\dfrac{\partial{u}}{\partial{x_1}}&\cdots&\dfrac{\partial{u}}{\partial{x_n}}\end{bmatrix}\\
   &=\frac{\mathrm{d}g(u)}{\mathrm{d}{u}}\frac{\partial{u}}{\partial\mathbf{x}}
   \end{align*}
   $$

5. $$
   \begin{align*}
   \frac{\partial{f(g(u))}}{\partial\mathbf{x}}&=\begin{bmatrix}\dfrac{\partial{f(g(u))}}{\partial{x_1}}&\cdots&\dfrac{\partial{f(g(u))}}{\partial{x_n}}\end{bmatrix}\\
   &=\begin{bmatrix}\dfrac{\mathrm{d}f(g)}{\mathrm{d}{g}}\dfrac{\mathrm{d}g(u)}{\mathrm{d}{u}}\dfrac{\partial{u}}{\partial{x_1}}&\cdots&\dfrac{\mathrm{d}f(g)}{\mathrm{d}{g}}\dfrac{\mathrm{d}g(u)}{\mathrm{d}{u}}\dfrac{\partial{u}}{\partial{x_n}}\end{bmatrix}\\
   &=\frac{\mathrm{d}f(g)}{\mathrm{d}{g}}\frac{\mathrm{d}g(u)}{\mathrm{d}{u}}\begin{bmatrix}\dfrac{\partial{u}}{\partial{x_1}}&\cdots&\dfrac{\partial{u}}{\partial{x_n}}\end{bmatrix}\\
   &=\dfrac{\mathrm{d}f(g)}{\mathrm{d}{g}}\frac{\mathrm{d}g(u)}{\mathrm{d}{u}}\frac{\partial{u}}{\partial\mathbf{x}}
   \end{align*}
   $$

6. $$
   \begin{align*}
   \frac{\partial\mathbf{u}^T\mathbf{v}}{\partial\mathbf{x}}&=\begin{bmatrix}\dfrac{\partial\sum_{i}u_iv_i}{\partial{x_1}}&\cdots&\dfrac{\partial\sum_{i}u_iv_i}{\partial{x_n}}\end{bmatrix}\\
   &=\begin{bmatrix}\sum_{i}u_i\dfrac{\partial{v_i}}{\partial{x_1}}+\sum_{i}v_i\dfrac{\partial{u_i}}{\partial{x_1}}&\cdots&\sum_{i}u_i\dfrac{\partial{v_i}}{\partial{x_n}}+\sum_{i}v_i\dfrac{\partial{u_i}}{\partial{x_n}}\end{bmatrix}\\
   &=\mathbf{u}^T\frac{\partial\mathbf{v}}{\partial\mathbf{x}}+\mathbf{v}^T\frac{\partial\mathbf{u}}{\partial\mathbf{x}}
   \end{align*}
   $$

7. $$
   \begin{align*}
   \left[\frac{\partial\mathbf{u}^T\mathbf{A}\mathbf{v}}{\partial\mathbf{x}}\right]_{i}&=\left[\frac{\partial\sum_{j}\sum_{k}u_ja_{jk}v_k}{\partial{x_i}}\right]_{i}\\
   &=\left[\sum_{j}\sum_{k}\frac{\partial{u_j}}{\partial{x_i}}a_{jk}v_k\right]_{i}+\left[\sum_{j}\sum_{k}u_ja_{jk}\frac{\partial{v_k}}{\partial{x_i}}\right]_{i}\\
   &\Longrightarrow\frac{\partial\mathbf{u}^T\mathbf{A}\mathbf{v}}{\partial\mathbf{x}}=\mathbf{u}^T\mathbf{A}\frac{\partial\mathbf{v}}{\partial\mathbf{x}}+\mathbf{v}^T\mathbf{A}^T\frac{\partial\mathbf{u}}{\partial\mathbf{x}}
   \end{align*}
   $$

### Vector by Vector

1. $$
   \left[\frac{\partial{\mathbf{a}}}{\partial\mathbf{x}}\right]_{ij}=\left[\frac{\partial{a_i}}{\partial{x_j}}\right]_{ij}=\left[0\right]_{ij}\Longrightarrow\frac{\partial{\mathbf{a}}}{\partial\mathbf{x}}=\mathbf{0}
   $$

2. $$
   \begin{align*}
   \left[\frac{\partial{u\mathbf{v}}}{\partial\mathbf{x}}\right]_{ij}&=\left[\frac{\partial{uv_i}}{\partial{x_j}}\right]_{ij}=\left[u\frac{\partial{v_i}}{\partial{x_j}}+v_i\frac{\partial{u}}{\partial{x_j}}\right]_{ij}\\
   &=\left[u\frac{\partial{v_i}}{\partial{x_j}}\right]_{ij}+\left[v_i\frac{\partial{u}}{\partial{x_j}}\right]_{ij}\\
   &=u\left[\frac{\partial{v_i}}{\partial{x_j}}\right]_{ij}+\left[v_i\frac{\partial{u}}{\partial{x_j}}\right]_{ij}\\
   &\Longrightarrow\frac{\partial{u\mathbf{v}}}{\partial\mathbf{x}}=u\frac{\partial{\mathbf{v}}}{\partial\mathbf{x}}+\mathbf{v}\frac{\partial{u}}{\partial\mathbf{x}}
   \end{align*}
   $$

3. $$
   \left[\frac{\partial\mathbf{Au}}{\partial\mathbf{x}}\right]_{ij}=\left[\frac{\partial(a_{i1}u_1+\cdots+a_{in}u_n)}{\partial{x_j}}\right]_{ij}=\left[a_{i1}\frac{\partial{u_1}}{\partial{x_j}}+\cdots+a_{in}\frac{\partial{u_n}}{\partial{x_j}}\right]_{ij}\\
   \Longrightarrow\frac{\partial{\mathbf{Au}}}{\partial\mathbf{x}}=\mathbf{A}\frac{\partial{\mathbf{u}}}{\partial\mathbf{x}}
   $$

4. $$
   \left[\frac{\partial\mathbf{x}^T\mathbf{A}}{\partial\mathbf{x}}\right]_{ij}=\left[\frac{\partial(a_{1i}x_1+\cdots+a_{ni}x_n)}{\partial{x_j}}\right]_{ij}=\left[a_{ji}\right]_{ij}\Longrightarrow\frac{\partial\mathbf{x}^T\mathbf{A}}{\partial\mathbf{x}}=\mathbf{A}^T
   $$

5. $$
   \left[\frac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{x}}\right]_{ij}=\left[\frac{\partial{g_i}(\mathbf{u})}{\partial{x_j}}\right]_{ij}=\left[\sum_{k}\frac{\partial{g_i}(u_k)}{\partial{u_k}}\frac{\partial{u_k}}{\partial{x_j}}\right]_{ij}\\
   \frac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{x}}=\frac{\partial\mathbf{g}(\mathbf{u})}{\partial\mathbf{u}}\frac{\partial\mathbf{u}}{\partial\mathbf{x}}
   $$

### Scalar by Matrix

1. $$
   \left[\dfrac{\partial{a}}{\partial\mathbf{X}}\right]_{ij}=\left[\dfrac{\partial{a}}{\partial{x_{ji}}}\right]_{ij}\Longrightarrow\dfrac{\partial{a}}{\partial\mathbf{X}}=\mathbf{0}
   $$

2. $$
   \begin{align*}
   \left[\frac{\partial(a_1u+a_2v)}{\partial\mathbf{X}}\right]_{ij}&=\left[\frac{\partial(a_1u+a_2v)}{\partial{x_{ji}}}\right]_{ij}=\left[a_1\frac{\partial{u}}{\partial{x_{ji}}}+a_2\frac{\partial{u}}{\partial{x_{ji}}}\right]_{ij}\\
   &=\left[a_1\frac{\partial{u}}{\partial{x_{ji}}}\right]_{ij}+\left[a_2\frac{\partial{u}}{\partial{x_{ji}}}\right]_{ij}\\
   &\Longrightarrow\frac{\partial(a_1u+a_2v)}{\partial\mathbf{X}}=a_1\frac{\partial{u}}{\partial\mathbf{X}}+a_2\frac{\partial{v}}{\partial\mathbf{X}}
   \end{align*}
   $$

3. $$
   \left[\frac{\partial{g(u)}}{\partial\mathbf{X}}\right]_{ij}=\left[\frac{\partial{g(u)}}{\partial{x_{ji}}}\right]_{ij}=\left[\frac{\mathrm{d}g(u)}{\mathrm{d}u}\frac{\partial{u}}{\partial{x_{ji}}}\right]_{ij}=\frac{\mathrm{d}g(u)}{\mathrm{d}u}\\
   \Longrightarrow\left[\frac{\partial{u}}{\partial{x_{ji}}}\right]_{ij}
   \frac{\partial{g(u)}}{\partial{\mathbf{X}}}=\dfrac{\mathrm{d}g(u)}{\mathrm{d}u}\frac{\partial{u}}{\partial{\mathbf{X}}}
   $$

## Linear Fitting

$X$ is the feature matrix, $w$ is the weight vector, $Y$ is label vector.
$$
\begin{align*}
E&=\|Xw-Y\|^2\\
&=(Xw-Y)^T(Xw-Y)\\
&=(w^TX^T-Y^T)(Xw-Y)\\
&=w^TX^TXw-w^TX^TY-Y^TXw+Y^TY
\end{align*}
$$
Then the derivative $w$ of $E$ is 0:
$$
\begin{align*}
\frac{\partial{E}}{\partial{w}}&=2w^TX^TX-2Y^TX=0\\
X^TXw&=X^TY\\
w&=(X^TX)^{-1}X^TY
\end{align*}
$$
