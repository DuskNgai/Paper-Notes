# Chapter 7 Diagonalization and Quadratic Forms

## 7.1 正交矩阵 Orthogonal Matrices

### DEFINITION 1

A square matrix $A$ is said to be *orthogonal* if its transpose is the same as its inverse, that is, if
$$
A^{-1}=A^T
$$
or, equivalently, if
$$
AA^T=A^TA=I
$$

### THEOREM 7.1.1

The following are equivalent for an $n\times n$ matrix $A$.

1. $A$ is orthogonal.
2. The row vectors of $A$ form an orthonormal set in $\mathbb{R}^n$ with the Euclidean inner product.
3. The column vectors of $A$ form an orthonormal set in $\mathbb{R}^n$ with the Euclidean inner product.

#### Proof

##### 1 => 2

Let $\mathbf{r}_i$ be the $i$-th row vector and $\mathbf{c}_j$ be $j$-th the column vector of $A$. By transpose, we have $\mathbf{c}_j^T=\mathbf{r}_i$. Then:
$$
AA^T=\begin{bmatrix}
\mathbf{r}_1\mathbf{c}_1^T&\mathbf{r}_1\mathbf{c}_2^T&\cdots&\mathbf{r}_1\mathbf{c}_n^T\\
\mathbf{r}_2\mathbf{c}_1^T&\mathbf{r}_2\mathbf{c}_2^T&\cdots&\mathbf{r}_2\mathbf{c}_n^T\\
\vdots&\vdots&\ddots&\vdots\\
\mathbf{r}_n\mathbf{c}_1^T&\mathbf{r}_n\mathbf{c}_2^T&\cdots&\mathbf{r}_n\mathbf{c}_n^T
\end{bmatrix}
$$
Since $A$ is orthogonal, then $\mathbf{r}_i\mathbf{c}_j^T=\mathbf{r}_i\cdot\mathbf{r}_j=\delta_{ij}$, which are true if and only if $\{\mathbf{r}_1,\mathbf{r}_2,\dots,\mathbf{r}_n\}$ is an orthonormal set in $\mathbb{R}^n$.

##### 1 => 3

Almost the same.

### THEOREM 7.1.2

1. The transpose of an orthogonal matrix is orthogonal.
2. The inverse of an orthogonal matrix is orthogonal.
3. A product of orthogonal matrices is orthogonal.
4. If $A$ is orthogonal, then $\det(A)=1$ or $\det(A)=-1$.

### THEOREM 7.1.3

If $A$ is an $n\times n$ matrix, then the following are equivalent.

1. $A$ is orthogonal.
2. $\|A\mathbf{x}\| = \|\mathbf{x}\|$ for all $\mathbf{x}$ in $\mathbb{R}^n$.
3. $A\mathbf{x}\cdot A\mathbf{y}=\mathbf{x}\cdot \mathbf{y}$ for all $\mathbf{x}$ and $\mathbf{y}$ in $\mathbb{R}^n$.

#### Proof

##### 1 => 2

$$
\|A\mathbf{x}\|=\sqrt{A\mathbf{x}\cdot A\mathbf{x}}=\sqrt{\mathbf{x}^TA^TA\mathbf{x}}=\sqrt{\mathbf{x}^T\mathbf{x}}=\|\mathbf{x}\|
$$

##### 2 => 3

$$
\begin{align*}
A\mathbf{x}\cdot A\mathbf{y}&=\frac{1}{4}\|A\mathbf{x}+A\mathbf{y}\|^2-\frac{1}{4}\|A\mathbf{x}-A\mathbf{y}\|^2\\
&=\frac{1}{4}\|\mathbf{x}+\mathbf{y}\|^2-\frac{1}{4}\|\mathbf{x}-\mathbf{y}\|^2\\
&=\mathbf{x}\cdot \mathbf{y}
\end{align*}
$$

##### 3 => 1

Since $\mathbf{x}^T\mathbf{y}=\mathbf{x}^TA^TA\mathbf{y}$, then $\mathbf{x}^T(A^TA-I)\mathbf{y}=0$. Since it holds for all $\mathbf{x}$, then let $\mathbf{x}=(A^TA-I)\mathbf{y}$, then $(A^TA-I)\mathbf{y}=\mathbf{0}$. Since $\mathbf{y}$ can be any vector, then $A^TA=I$.

### THEOREM 7.1.4

If $S$ is an orthonormal basis for an $n$-dimensional inner product space $V$, and if
$$
(\mathbf{u})_S=(u_1,u_2,\dots,u_n)\quad(\mathbf{v})_S=(v_1,v_2,\dots,v_n)
$$
then:

1. $\|\mathbf{u}\|=\sqrt{u_1^2+u_2^2+\dots+u_n^2}$
2. $d(\mathbf{u},\mathbf{v})=\sqrt{(u_1-v_1)^2+(u_2-v_2)^2+\dots+(u_n-v_n)^2}$
3. $\langle\mathbf{u},\mathbf{v}\rangle=u_1v_1+u_2v_2+\dots+u_nv_n$.

### THEOREM 7.1.5

Let $V$ be a finite-dimensional inner product space. If $P$ is the transition matrix from one orthonormal basis for $V$ to another orthonormal basis for $V$, then $P$ is an orthogonal matrix.

#### Proof

Assume that $V$ is an n-dimensional inner product space and that $P$ is the transition matrix from an orthonormal basis $B'$ to an orthonormal basis $B$.  

Since
$$
\|[\mathbf{u}]_B\|=\|[\mathbf{u}]_{B'}\|
$$
And
$$
\|P[\mathbf{u}]_{B'}\|=\|[\mathbf{u}]_{B'}\|
$$
Then $P$ is orthogonal by 7.1.3.

## 7.2 Orthogonal Diagonalization

### DEFINITION 1

If $A$ and $B$ are square matrices, then we say that $B$ is **orthogonally similar** to $A$ if there is an orthogonal matrix $P$ such that $B = P^TAP$.

If A is orthogonally similar to some diagonal matrix, then we say that $A$ is orthogonally diagonalizable and that $P$ orthogonally diagonalizes $A$.

### THEOREM 7.2.1

If $A$ is an $n\times n$ matrix with real entries, then the following are equivalent.

1. $A$ is orthogonally diagonalizable.
2. $A$ has an orthonormal set of $n$ eigenvectors.
3. $A$ is symmetric.

#### Proof

##### 1 => 2

Since $A$ is orthogonally diagonalizable, there is an orthogonal matrix $P$ such that $P^TAP$ is diagonal. And it has $n$ eigenvectors. Since $P$ is orthogonal, its column vectors are orthonormal.

##### 2 => 1

Since $A$ has an orthonormal set of $n$ eigenvectors, the matrix $P$ with these eigenvectors as columns diagonalizes $A$. Since these eigenvectors are orthonormal, $P$ is orthogonal and thus orthogonally diagonalizes $A$.

##### 1 => 3

$A$ must be symmetric since:
$$
A^T=(PDP^T)^T=PD^TP^T=PDP^T=A
$$

### THEOREM 7.2.2

If $A$ is a symmetric matrix with real entries, then:

1. The eigenvalues of $A$ are all real numbers.
2. Eigenvectors from different eigenspaces are orthogonal.

> 对称阵正交对角化的步骤：
>
> 1. 计算对称阵的特征值和特征向量。
> 2. 用 Gram-Schmidt 正交化处理特征向量，得到特征空间的正交基。
> 3. $P$ 为正交基组成的矩阵。
> 4. $D$ 为对应的特征值组成的对角矩阵。

### Spectral Decomposition

$$
\begin{align*}
A=PDP^T&=\begin{bmatrix}
\mathbf{u}_1&\mathbf{u}_2&\cdots&\mathbf{u}_n
\end{bmatrix}
\begin{bmatrix}
\lambda_1&0&\cdots&0\\
0&\lambda_2&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&\lambda_n
\end{bmatrix}
\begin{bmatrix}
\mathbf{u}_1^T\\\mathbf{u}_2^T\\\vdots\\\mathbf{u}_n^T
\end{bmatrix}\\
&=\lambda_1\mathbf{u}_1\mathbf{u}_1^T+\lambda_2\mathbf{u}_2\mathbf{u}_2^T+\dots+\lambda_n\mathbf{u}_n\mathbf{u}_n^T
\end{align*}
$$

### THEOREM 7.2.3 Schur's Theorem

If $A$ is an $n\times n$ matrix with real entries and real eigenvalues, then there is an orthogonal matrix $P$ such that $P^TAP$ is an upper triangular matrix of the form
$$
PAP^T=
\begin{bmatrix}
\lambda_1&*&\cdots&*\\
0&\lambda_2&\cdots&*\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&\lambda_n
\end{bmatrix}
$$
in which $\lambda_1,\lambda_2,\dots,\lambda_n$ are the eigenvalues of A repeated according to multiplicity.

### THEOREM 7.2.3 Hessenberg's Theorem

If $A$ is an $n\times n$ matrix with real entries, then there is an orthogonal matrix $P$ such that $P^TAP$ is a matrix of the form
$$
PAP^T=
\begin{bmatrix}
*&*&\cdots&*&*\\
*&*&\cdots&*&*\\
0&*&\cdots&*&*\\
\vdots&\vdots&\ddots&\vdots&\vdots\\
0&0&\cdots&*&*
\end{bmatrix}
$$

## 7.3 Quadratic Forms

If $A$ is a symmetric $n\times n$ matrix and $\mathbf{x}$ is an $n\times1$ column vector of variables, then we call the function
$$
Q_A(\mathbf{x})=\mathbf{x}^TA\mathbf{x}
$$
the *quadratic form associated with $A$*.

### THEOREM 7.3.1 The Principal Axes Theorem









