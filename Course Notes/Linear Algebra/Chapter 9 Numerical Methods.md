# Chapter 9 数值方法 Numerical Methods

## 9.1 LU 分解 LU-Decompositions

### DEFINITION 1

A factorization of a square matrix $A$ as
$$
A=LU
$$
where $L$ is lower triangular and $U$ is upper triangular, is called an *LU-decomposition* (or LU-factorization) of $A$​.



## 9.4 奇异值分解 Singular Value Decomposition(SVD)

Diagonalization for $m\times n$ matrices.

### 方针的分解 Decompositions of Square Matrices

#### 特征值分解 Eigenvalue Decomposition(EVD)

Every *symmetric matrix* $A$ can be decomposed as
$$
A=PDP^T
$$
where $P$ is an $n\times n$ orthogonal matrix of eigenvectors of $A$, and $D$ is the diagonal matrix whose diagonal entries are the eigenvalues corresponding to the column vectors of $P$.

#### 拟上三角分解 Hessenberg Decomposition

If an $n\times n$ matrix $A$ is not symmetric
$$
A=PHP^T
$$
in which $P$ is an orthogonal matrix and $H$ is in upper Hessenberg form.

If $A$ has real eigenvalues, then it has a Schur decomposition
$$
A=PSP^T
$$
in which $P$ is an orthogonal matrix and $S$ is upper triangular.

#### 正交矩阵的作用

The orthogonal matrices that appear in these factorizations do not magnify roundoff error.

Suppose a vector $\hat{\mathbf x}$, after roundoff, it becomes $\mathbf x$, then the error is $\mathbf e =\mathbf x-\hat{\mathbf x}$. For orthogonal matrix $P$, **WHY?**
$$
\|P\mathbf e\|=\|P(\mathbf x-\hat{\mathbf x})\|=\|\mathbf x-\hat{\mathbf x}\|=\|\mathbf e\|
$$

### 奇异值 Singular Values

### THEOREM 9.4.1

If $A$ is an $m\times n$ matrix, then

1. $A$ and $A^TA$ have the same null space.
2. $A$ and $A^TA$ have the same row space.
3. $A^T$ and $A^TA$ have the same column space.
4. $A$ and $A^TA$ have the same rank.

#### Proof

1. If $\mathbf x_0$ is any solution of $A\mathbf x=\mathbf 0$, then $A^TA\mathbf x_0=A^T(A\mathbf x_0)=A^T\mathbf 0=\mathbf 0$, also the soltion of $A^TA\mathbf x=\mathbf 0$.
   If $\mathbf x_0$ is any solution of $A^TA\mathbf x=\mathbf 0$, then $\mathbf x_0$ is orthogonal to all vector in the row space of $A^TA$. Since $A^TA$ is symmetric, then $\mathbf x_0$ is orthogonal to all vector in the colunm space of $A^TA$, implice that $\mathbf x_0^TA^TA\mathbf=\mathbf 0^TA=\mathbf0^T$.
   $$
   \mathbf0^T\mathbf x_0=(\mathbf x_0^TA^TA)\mathbf x_0=\mathbf x_0^T(A^TA\mathbf x_0)=(A\mathbf x_0)^T(A\mathbf x_0)=\|A\mathbf x_0\|^2=0
   $$
   Then $A\mathbf x_0=\mathbf 0$.

2. **WHY?**

3. **WHY?**

4. **WHY?**

### THEOREM 9.4.2

If $A$ is an $m\times n$ matrix, then

1. $A^TA$ is orthogonally diagonalizable.
2. The eigenvalues of $A^TA$ are nonnegative.

#### Proof

1. $A^TA$ is symmetric then it is orthogonally diagonalizable.

2. Since $A^TA$ is orthogonally diagonalizable, then there is an orthonormal basis $\mathbf v_1,\dots\mathbf v_n$ consisting of eigenvectors of $A^TA$, let $\lambda_1,\dots,\lambda_n$ be correspoding eigenvalues, then
   $$
   \|A\mathbf v_i\|^2=(A\mathbf v_i)\cdot(A\mathbf v_i)=\mathbf v_i^TA^TA\mathbf v_i=\mathbf v_i^T\lambda_i\mathbf v_i=\lambda_i\|\mathbf v_i\|^2=\lambda_i\ge0
   $$

### DEFINITION 1 Singular Values

If $A$ is an $m\times n$ matrix, and if $\lambda_1,\dots,\lambda_n$ are the eigenvalues of $A^TA$, then the numbers
$$
\sigma_i=\sqrt{\lambda_i}
$$
are called the *singular values* of $A$.

### Singular Value Decomposition

### THEOREM9.4.3 Singular Value Decomposition (Brief Form)

`pass`

### THEOREM 9.4.4 Singular Value Decomposition (Expanded Form)

If $A$ is an $m\times n$ matrix of rank $k$, then $A$ can be factored as
$$
\begin{align*}
A&=U\Sigma V^T\\
&=\begin{bmatrix}\mathbf u_1&\cdots&\mathbf u_k|\mathbf u_{k+1}&\cdots&\mathbf u_m\end{bmatrix}
\left[
\begin{array}{ccc|c} 
    \sigma_1 &  \cdots  &     0    &          \\
    \vdots   &  \ddots  &  \vdots  & \mathbf0 \\
        0    &  \cdots  & \sigma_k &          \\
    \hline              
             & \mathbf0 &          & \mathbf0 \\
      
\end{array}
\right]
\begin{bmatrix}
\mathbf v_1^T\\
\vdots\\
\mathbf v_k^T\\
\rule{0.5cm}{0.05em}\\
\mathbf v_{k+1}^T\\
\vdots\\
\mathbf v_n^T
\end{bmatrix}
\end{align*}
$$
in which $U$, $\Sigma$, and $V$ have sizes $m \times m$, $m \times n$, and $n \times n$, respectively, and in which:

1. $V$ orthogonally diagonalizes $A^TA$.
2. The nonzero diagonal entries of $\Sigma$ are $\sigma_i=\sqrt{\lambda_i}$, $\lambda_1,\dots,\lambda_n$ are the eigenvalues of $A^TA$.
3. The column vectors of $V$ are ordered so that $\sigma_i$ are in non-increasing order.
4. $\mathbf u_i=\frac{A\mathbf v_i}{\|A\mathbf v_i\|}=\frac{1}{\sigma_i}A\mathbf v_i$.
5. $\{\mathbf u_1,\dots,\mathbf u_k\}$ is an orthonormal basis for $\text{col}(A)$.
6. $\{\mathbf u_1,\dots,\mathbf u_k,\mathbf u_{k+1},\dots,\mathbf u_m\}$ is an extension of $\{\mathbf u_1,\dots\mathbf u_k\}$ to an orthonormal basis for $\mathbb R^m$.

#### Proof

Let $A$ be a $n\times n$ matrix, for $m\times n$ matrix, we just need to account for the possibility that $m>n$ or $n>m$.

First we eigenvalue decomposite for $A^TA$ since it is symmetric
$$
A^TA=VDV^T\\
$$
where $V=\begin{bmatrix}\mathbf v_1&\cdots&\mathbf v_n\end{bmatrix}$ consists all the orthonormal eigenvector of $A^TA$, $D$ is a diagnol matrix whose diagnol entries are eigenvalues in descending order. Since $A$ has rank $k$, then $A^TA$ has rank $k$ too. Then $D$ has $k$ non-negative entries.

 Now consider the set of image vectors
$$
S=\{A\mathbf v_1,\cdots,A\mathbf v_k\}
$$
this is an orthogonal set since:
$$
A\mathbf v_i\cdot A\mathbf v_j=\mathbf v_i^TA^TA\mathbf v_j=\lambda_j\mathbf v_i^T\mathbf v_j=\mathbf 0
$$
Thus, $S$ is an orthogonal set of non-zero vectors in the column space of $A$. Since the column space of $\dim(\text{col}(A))=\text{rank}(A)=k$, then $S$ is the orthogonal basis for $\text{col}(A)$.

Now, we normalize it and obtain an orthonormal basis $\{\mathbf u_1,\cdots,\mathbf u_k\}$ 
$$
\mathbf u_i=\frac{A\mathbf v_i}{\|A\mathbf v_i\|}=\frac{1}{\sqrt{\lambda_i}}A\mathbf v_i=\frac{1}{\sigma_i}A\mathbf v_i
$$
Then we extend it tom an orthonormal basis for $\mathbb R^n$
$$
U=\begin{bmatrix}\mathbf u_1&\cdots&\mathbf u_k&\mathbf u_{k+1}&\cdots&\mathbf u_n\end{bmatrix}
$$
and let $\Sigma$ be the required diagnol matrix, then
$$
\begin{align*}
U\Sigma&=\begin{bmatrix}\sigma_1\mathbf u_1&\cdots&\sigma_k\mathbf u_k&0&\cdots&0\end{bmatrix}\\
&=\begin{bmatrix}A\mathbf v_1&\cdots&A\mathbf v_k&A\mathbf v_{k+1}&\cdots&A\mathbf v_n\end{bmatrix}\\
&=AV
\end{align*}\\
U\Sigma V^T=A
$$

> 奇异值分解的步骤：
>
> 1. 计算 $A^TA$ 的特征值和特征向量。
> 2. 降序排列 $A^TA$ 的特征值，对得到的特征值开根号得到 $\Sigma$。
> 3. 单位化对应特征值的 $A^TA$ 的特征向量，并且组成矩阵得到 $V$。
> 4. 用 $A\mathbf v_i/\sigma_i=\mathbf u_i$ 得到 $\{\mathbf u_1,\dots,\mathbf u_k\}$，其中 $k$ 为 $A$ 的秩。
> 5. 扩充 $\{\mathbf u_1,\dots,\mathbf u_k\}$ 使得其成为 $\mathbb R^n$ 的一组基，并且组成矩阵得到 $U$。

> Example:
> $$
> A=\begin{bmatrix}1&1\\0&1\\1&0\end{bmatrix}
> $$
> then 
> $$
> A^TA=\begin{bmatrix}1&0&1\\1&1&0\end{bmatrix}\begin{bmatrix}1&1\\0&1\\1&0\end{bmatrix}=\begin{bmatrix}2&1\\1&2\end{bmatrix}
> $$
> then  eigenvalues of $A^TA$ are
> $$
> \det(\lambda I-A^TA)=\begin{bmatrix}\lambda-2&-1\\-1&\lambda-2\end{bmatrix}=(\lambda-2)^2-1=(\lambda-1)(\lambda-3)=0
> $$
> then $\lambda_1=3,\lambda_2=1$, $\sigma_1=\sqrt3,\sigma_2=1$ (non-increasing order).
>
> ---
>
> The orthonormal basis for 
> $$
> \begin{bmatrix}1&-1\\-1&1\end{bmatrix}
> $$
> is
> $$
> \mathbf v_1=\begin{bmatrix}\frac{\sqrt{2}}{2}\\\frac{\sqrt{2}}{2}\end{bmatrix}
> $$
> The orthonormal basis for
> $$
> \begin{bmatrix}-1&-1\\-1&-1\end{bmatrix}
> $$
> is
> $$
> \mathbf v_2=\begin{bmatrix}\frac{\sqrt{2}}{2}\\-\frac{\sqrt{2}}{2}\end{bmatrix}
> $$
>
> ---
>
> Then 
> $$
> \mathbf u_1=\frac{1}{\sqrt{3}}A\mathbf v_1=\begin{bmatrix}1&1\\0&1\\1&0\end{bmatrix}\begin{bmatrix}\frac{\sqrt{2}}{2}\\\frac{\sqrt{2}}{2}\end{bmatrix}=\frac{1}{\sqrt{3}}\begin{bmatrix}\sqrt{2}\\\frac{\sqrt{2}}{2}\\\frac{\sqrt{2}}{2}\end{bmatrix}=\begin{bmatrix}\frac{\sqrt{6}}{3}\\\frac{\sqrt{6}}{6}\\\frac{\sqrt{6}}{6}\end{bmatrix}\\
> 
> \mathbf u_2=1A\mathbf v_2=\begin{bmatrix}1&1\\0&1\\1&0\end{bmatrix}\begin{bmatrix}\frac{\sqrt{2}}{2}\\-\frac{\sqrt{2}}{2}\end{bmatrix}=\begin{bmatrix}0\\-\frac{\sqrt{2}}{2}\\\frac{\sqrt{2}}{2}\end{bmatrix}
> $$
> Extend $\{\mathbf u_1,\mathbf u_2\}$ to an orthonormal basis for $\mathbb R^3$, then $\mathbf u_3$ satisfy that
> $$
> \begin{bmatrix}
> \mathbf u_1^T\\
> \mathbf u_2^T
> \end{bmatrix}\mathbf u_3=0\\
> \mathbf u_3=t\begin{bmatrix}-1\\1\\1\end{bmatrix}=\begin{bmatrix}-\frac{\sqrt{3}}{3}\\\frac{\sqrt{3}}{3}\\\frac{\sqrt{3}}{3}\end{bmatrix}
> $$
> Then
> $$
> A=U\Sigma V^T\\
> =\begin{bmatrix}\frac{\sqrt{6}}{3}&0&-\frac{\sqrt{3}}{3}\\\frac{\sqrt{6}}{6}&-\frac{\sqrt{2}}{2}&\frac{\sqrt{3}}{3}\\\frac{\sqrt{6}}{6}&\frac{\sqrt{2}}{2}&\frac{\sqrt{3}}{3}\end{bmatrix}\begin{bmatrix}\sqrt{3}&0\\0&1\\0&0\end{bmatrix}\begin{bmatrix}\frac{\sqrt{2}}{2}&\frac{\sqrt{2}}{2}\\\frac{\sqrt{2}}{2}&-\frac{\sqrt{2}}{2}\end{bmatrix}
> $$

## 9.5 用奇异值分解实现数据压缩 Data Compression Using Singular Value Decomposition

### 简化奇异值分解 Reduced Singular Value Decomposition

The products that involve zero blocks as factors drop out, leaving
$$
\begin{align*}
A&=U_1\Sigma_1 V_1^T\\
&=\begin{bmatrix}\mathbf u_1&\cdots&\mathbf u_k\end{bmatrix}
\left[
\begin{array}{ccc|c} 
    \sigma_1 &  \cdots  &     0   \\
    \vdots   &  \ddots  &  \vdots  \\
        0    &  \cdots  & \sigma_k \\      
\end{array}
\right]
\begin{bmatrix}
\mathbf v_1^T\\
\vdots\\
\mathbf v_k^T\\
\end{bmatrix}\\
&=\sigma_1\mathbf u_1\mathbf v_1^T+\sigma_2\mathbf u_2\mathbf v_2^T+\dots+\sigma_k\mathbf u_k\mathbf v_k^T
\end{align*}
$$

### 数据压缩和图像处理 Data Compression and Image Processing

If an image has size $m\times n$, then it needs to store $mn$ entries.

If we apply Reduced Singular Value Decomposition, it need to store $km+k+kn$ entries.

Notice that there are some $\sigma_i$ which are small enough to be ignored, the we can drop the corresponding entries to make the matrix rank $r$. This is called *rank $r$ approximation of $A$*.
