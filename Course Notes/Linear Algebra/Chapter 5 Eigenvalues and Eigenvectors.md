# Chapter 5 Eigenvalues and Eigenvectors

## 5.1 特征值和特征向量 Eigenvalues and Eigenvectors

### DEFINITION 1

If $A$ is an $n\times n$ matrix, then a nonzero vector $\mathbf x$ in $\mathbb R^n$ is called an *eigenvector* of $A$ (or of the matrix operator $T_A$) if $A\mathbf x$ is a scalar multiple of $\mathbf x$; that is,
$$
A\mathbf x=\lambda\mathbf x
$$
for some scalar $\lambda$. The scalar $\lambda$ is called an *eigenvalue* of $A$ (or of $T_A$), and $\mathbf x$ is said to be an eigenvector corresponding to $\lambda$.

### THEOREM 5.1.1

If $A$ is an $n\times n$ matrix, then $\lambda$ is an eigenvalue of $A$ if and only if it satisfies the equation
$$
\det(\lambda I − A) = 0
$$
This is called the 特征方程 *characteristic equation* of $A$.

#### Proof

$$
\begin{align*}
A\mathbf x&=\lambda\mathbf x\\
A\mathbf x&=\lambda I\mathbf x\\
(\lambda I-A)\mathbf x&=\mathbf 0&(\mathbf x\ne\mathbf 0)\\
\det(\lambda I-A)&=0
\end{align*}
$$

When the characteristic equation $\det(\lambda I − A)$ is expanded, it takes the form
$$
\lambda^n+c_1\lambda^{n-1}+\dots+c_n=0
$$
then we called 
$$
p(\lambda)=\lambda^n+c_1\lambda^{n-1}+\dots+c_n
$$
the 特征多项式 *characteristic polynomial* of $A$.

### THEOREM 5.1.2

If $A$ is an $n\times n$ triangular matrix, (upper triangular, lower triangular, or diagonal), then the eigenvalues of $A$ are the entries on the main diagonal of $A$.

The diagonal entries of triangular matrix will be the eigenvalues of it, and the product of the diagonal entries of triangular matrix will be the determinant of it.

### THEOREM 5.1.3

If $A$ is an $n\times n$ matrix, the following statements are equivalent.

1. $\lambda$ is an eigenvalue of $A$.
2. $\lambda$ is a solution of the characteristic equation $\det(\lambda I − A) = 0$.
3. The system of equations $(\lambda I − A)\mathbf x = \mathbf 0$ has nontrivial solutions.
4. There is a nonzero vector $\mathbf x$ such that $A\mathbf x = \lambda\mathbf x$.

### 特征空间 The eigenspace of A

1. The solution/null space of $(\lambda I − A)\mathbf x = \mathbf 0$.
2. The kernel of the matrix operator $T_{\lambda I-A}:R^n\to R^n$.
3. The span of the set of vectors for which $A\mathbf x = \lambda\mathbf x$.

### THEOREM 5.1.4

A square matrix $A$ is invertible if and only if $\lambda = 0$ is not an eigenvalue of $A$.

#### Proof

Suppose A is an $n\times n$ matrix, and suppose $\lambda = 0$ is an eigenvalue of $A$, then for 
$$
p(\lambda)=\lambda^n+c_1\lambda^{n-1}+\dots+c_n=0
$$
$c_n$ should be $0$.

But if $\lambda = 0$
$$
\det(\lambda I-A)=\det(-A)=(-1)^n\det(A)=c_n=0
$$
then $A$ is invertible.

### DEFINITION 2

If $T : V\to V$ is a linear operator on a vector space $V$ , then a nonzero vector $\mathbf x$ in $V$ is called an eigenvector of $T$ if $T(\mathbf x)$ is a scalar multiple of $\mathbf x$; that is, for some scalar $\lambda$ . The scalar $\lambda$  is called an eigenvalue of $T$ , and $\mathbf x$ is said to be an eigenvector corresponding to $\lambda$.
## 5.2 对角化 Diagonalization

### DEFINITION 1

If $A$ and $B$ are square matrices, then we say that $B$ is similar to $A$ if there is an invertible matrix $P$ such that $B = P^{-1}AP$

A and B are 相似矩阵 *similar matrices* to each other.

### DEFINITION 2

A square matrix $A$ is said to be *diagonalizable* if it is similar to some diagonal matrix; that is, if there exists an invertible matrix $P$ such that $P^{−1}AP$ is diagonal. In this case the matrix $P$ is said to diagonalize $A$.

### THEOREM 5.2.1

If $A$ is an $n\times n$ matrix, the following statements are equivalent.

1. $A$ is diagonalizable.
2. $A$ has $n$ linearly independent eigenvectors.

#### Proof

##### 1 => 2

$$
AP=PD\\
A\begin{bmatrix}\mathbf p_1&\dots&\mathbf p_n\end{bmatrix}=\begin{bmatrix}A\mathbf p_1&\dots&A\mathbf p_n\end{bmatrix}=\begin{bmatrix}\lambda_1\mathbf p_1&\dots&\lambda_n\mathbf p_n\end{bmatrix}\\
A\mathbf p_i=\lambda_i\mathbf p_i
$$

Since $P$ is invertible, then $\mathbf p_i$ are linearly independent (non-zero), then they are the eigenvectors.

##### 2 => 1

$$
AP=A\begin{bmatrix}\mathbf p_1&\dots&\mathbf p_n\end{bmatrix}=\begin{bmatrix}A\mathbf p_1&\dots&A\mathbf p_n\end{bmatrix}=\begin{bmatrix}\lambda_1\mathbf p_1&\dots&\lambda_n\mathbf p_n\end{bmatrix}=PD\\
$$

Since $\{\mathbf p_1,\dots,\mathbf p_n\}$ are linearly independent, then $P$ is invertible, then $D=P^{-1}AP$.

### THEOREM 5.2.2

1. If $\lambda_1, \lambda_2,\dots,\lambda_k$ are distinct eigenvalues of a matrix $A$, and if $\mathbf v_1,\mathbf  v_2,\dots, \mathbf v_k$ are corresponding eigenvectors, then $\{\mathbf v_1,\mathbf  v_2,\dots, \mathbf v_k\}$ is a linearly independent set.
2. An $n\times n$ matrix with $n$ distinct eigenvalues is diagonalizable.

#### Proof

##### 1

Suppose $\{\mathbf v_1,\mathbf  v_2,\dots, \mathbf v_k\}$ is a linearly dependent set.

Since $\mathbf v_i\ne\mathbf 0$, then $\{\mathbf v_1\}$ is a linearly independent set. Let $r(1\le r< k)$ be the largest integer such that $\{\mathbf v_1,\mathbf  v_2,\dots, \mathbf v_r\}$ is a linearly dependent set. Then $\{\mathbf v_1,\mathbf  v_2,\dots, \mathbf v_{r+1}\}$ is a linearly dependent set.
$$
c_1\mathbf v_1+c_2\mathbf  v_2+\dots+c_{r+1} \mathbf v_{r+1}=\mathbf0\tag{1}\\
$$
Multiplying both sides of (1) by $A$
$$
A(c_1\mathbf v_1+c_2\mathbf  v_2+\dots+c_{r+1} \mathbf v_{r+1})=\mathbf0\\

c_1\lambda_1\mathbf v_1+c_2\lambda_2\mathbf  v_2+\dots+c_{r+1}\lambda_{r+1}\mathbf v_{r+1}=\mathbf0\tag{2}\\
$$
$(2)-\lambda_{r+1}(1)$
$$
c_1(\lambda_1-\lambda_{r+1})\mathbf v_1+c_2(\lambda_2-\lambda_{r+1})\mathbf  v_2+\dots+c_{r+1}(\lambda_{r+1}-\lambda_{r+1})\mathbf v_{r+1}=\mathbf0\\
c_1(\lambda_1-\lambda_{r+1})\mathbf v_1+c_2(\lambda_2-\lambda_{r+1})\mathbf  v_2+\dots+c_{r}(\lambda_{r}-\lambda_{r+1})\mathbf v_{r}=\mathbf0\\
$$
Since $\{\mathbf v_1,\mathbf  v_2,\dots, \mathbf v_r\}$ is a linearly dependent set, then 
$$
c_1=c_2=\dots=c_r=0
$$
But $c_{r+1}\ne0$, then $\mathbf v_{r+1}=\mathbf0$, which is not an proper eigenvector, then $\{\mathbf v_1,\mathbf  v_2,\dots, \mathbf v_k\}$ is a linearly independent set.

##### 2

Since matrix has $n$ distinct eigenvalues, then it has $n$ linearly independent eigenvectors, then it is diagonalizable.

> 方阵对角化的步骤：
>
> 1. 计算方阵的特征值和特征向量。
> 2. 观察特征向量是否为 $n$ 个线性无关的向量，否则不可对角化。
> 3. $P$ 为 $n$ 个特征向量组成的矩阵。
> 4. $D$ 为对应的特征值组成的对角矩阵。

> Example:
> $$
> A=\begin{bmatrix}0&0&-2\\1&2&1\\1&0&3\end{bmatrix}
> $$
> Then
> $$
> \det(\lambda I-A)=\begin{bmatrix}\lambda &0&2\\-1&\lambda -2&-1\\-1&0&\lambda -3\end{bmatrix}=(\lambda -1)(\lambda -2)^2=0
> $$
> For $\lambda=2$:
> $$
> \begin{bmatrix}2&0&2\\-1&0&-1\\-1&0&-1\end{bmatrix}\thicksim\begin{bmatrix}1&0&1\\0&0&0\\0&0&0\end{bmatrix}\\
> x_1=-s,x_2=t,x_3=s
> $$
> then
> $$
> \mathbf p_1=\begin{bmatrix}1\\0\\-1\end{bmatrix}\quad\mathbf p_2=\begin{bmatrix}0\\1\\0\end{bmatrix}
> $$
> For $\lambda=2$:
> $$
> \begin{bmatrix}1&0&2\\-1&-1&-1\\-1&0&-2\end{bmatrix}\thicksim\begin{bmatrix}1&0&2\\0&1&-1\\0&0&0\end{bmatrix}\\
> x_1=-2s,x_2=s,x_3=s
> $$
> then
> $$
> \mathbf p_3=\begin{bmatrix}-2\\1\\1\end{bmatrix}
> $$
> Then
> $$
> P=\begin{bmatrix}1&0&-2\\0&1&1\\-1&0&1\end{bmatrix}\quad
> D=\begin{bmatrix}2&0&0\\0&2&0\\0&0&1\end{bmatrix}
> $$

### THEOREM 5.2.3 矩阵幂的特征值 Eigenvalues of Powers of a Matrix

If $k$ is a positive integer, $\lambda$ is an eigenvalue of a matrix $A$, and $\mathbf x$ is a corresponding eigenvector, then $\lambda^k$ is an eigenvalue of $A^k$ and $\mathbf x$ is a corresponding eigenvector.

#### Proof

$$
(A^k)\mathbf x=(A^{k-1})A\mathbf x=\lambda(A^{k-1})\mathbf x=\lambda^k\mathbf x
$$

Notice that
$$
D^k=\begin{bmatrix}\lambda_1^k&\cdots&0\\\vdots&\ddots&\vdots\\0&\cdots&\lambda_n^k\end{bmatrix}
$$
then
$$
\begin{align*}
A^k&=(P^{-1}DP)^k\\
&=(P^{-1}DPP^{-1}DP)(P^{-1}DP)^{k-2}\\
&=(P^{-1}D^2P)(P^{-1}DP)^{k-2}\\
&=P^{-1}D^kP
\end{align*}
$$
Easy for computing.

### 几何重数和代数重数 Geometric and Algebraic Multiplicity

Notice that the example shows that the converse of THEOREM 5.2.2(2) does not hold.

If $\lambda_0$ is an eigenvalue of an $n\times n$ matrix $A$.

The geometric multiplicity of $\lambda_0$: the dimension of the eigenspace corresponding to $\lambda_0$.

The algebraic multiplicity of $\lambda_0$: the number of times that $\lambda − \lambda_0$ appears as a factor in the characteristic polynomial of $A$.

### THEOREM 5.2.4

`without proof`

If $A$ is a square matrix, then:

1. For every eigenvalue of $A$, the geometric multiplicity is less than or equal to the algebraic multiplicity.
2. A is diagonalizable if and only if the geometric multiplicity of every eigenvalue is equal to the algebraic multiplicity.

## 5.3 复线性空间 Complex Vector Spaces

### DEFINITION 1

If $n$ is a positive integer, then a complex $n$-tuple is a sequence of $n$ complex numbers ($v_1,v_2,\dots, v_n$). The set of all complex $n$-tuples is called complex $n$-space and is denoted by $\mathbb C ^n$. Scalars are complex numbers, and the operations of addition, subtraction, and scalar multiplication are performed component-wise.

Any vector in $\mathbb C^n$ can be split into 实部 *real* and 虚部 *imaginary* parts

$$
\begin{align*}
\mathbf v&=(v_1,v_2,\dots, v_n)=(a_1+ib_1,a_2+ib_2,\dots,a_n+ib_n)\\
&=(a_1,a_2,\dots,a_n)+i(b_1,b_2,\dots,b_n)\\
&=\Re e(\mathbf v)+i\Im m(\mathbf v)
\end{align*}
$$
then we define the *complex conjugate* of vector $\mathbf v$
$$
\bar{\mathbf v}=\Re e(\mathbf v)-i\Im m(\mathbf v)
$$
Real matrix: entries are real

Complex matrix: entries are real or complex

Similarly, we can define the *complex conjugate* of matrix $A$ as $\overline{A}$.

### THEOREM 5.3.1

If $\mathbf u$ and $\mathbf v$ are vectors in $\mathbb C^n$, and if $k$ is a scalar, then:

1. $\overline{\overline{\mathbf u}} = \mathbf u$
2. $\overline{k\mathbf u} = \overline{k}\overline{\mathbf u}$
3. $\overline{\mathbf u \pm \mathbf v} = \overline{\mathbf u} \pm \overline{\mathbf v}$

### THEOREM 5.3.2

If A is an $m \times k$ complex matrix and $B$ is a $k \times n$ complex matrix, then:

1. $\overline{\overline{A}} = A$
2. $(\overline{A^T}) = (\overline{A})^T$
3. $\overline{AB} =\overline{A}\overline{B}$

### DEFINITION 2

If $u = (u_1,u_2,\dots, u_n)$ and $v = (v_1,v_2,\dots, v_n)$ are vectors in $\mathbb C^n$, then the *complex Euclidean inner product* of $\mathbf u$ and $\mathbf v$ (also called the complex dot product) is denoted by $\mathbf u \cdot \mathbf v$ and is defined as
$$
\mathbf u \cdot \mathbf v = u_1\overline{v_1} + u_2\overline{v_2} +\cdots+ u_n\overline{v_n}\\
=\mathbf u^T\overline{\mathbf v}=\overline{\mathbf v}^T\mathbf u
$$
We also define *the Euclidean norm on $\mathbb C^n$* to be
$$
\|\mathbf v\| = \sqrt{\mathbf v \cdot \mathbf v} = \sqrt{|v_1|^2 + |v_2|^2 +\dots+|v_n|^2}
$$
Unit vector and orthogonal are the same as real.

### THEOREM 5.3.3

If $\mathbf u$, $\mathbf v$, and $\mathbf w$ are vectors in $\mathbb C^n$, and if $k$ is a scalar, then the complex Euclidean inner product has the following properties:

1. $\mathbf u \cdot \mathbf v = \overline{\mathbf v \cdot \mathbf u}$ [ Antisymmetry property]
2. $\mathbf u \cdot (\mathbf v + \mathbf w) = \mathbf u \cdot \mathbf v + \mathbf u \cdot \mathbf w$ [Distributive property]
3. $k(\mathbf u \cdot \mathbf v) = (k\mathbf u) \cdot \mathbf v$ [Homogeneity property]
4. $\mathbf u \cdot k\mathbf v = \overline{k}(\mathbf u \cdot \mathbf v)$ [ Anti-homogeneity property]
5. $\mathbf v \cdot \mathbf v ≥ 0$ and $\mathbf v \cdot \mathbf v = \mathbf 0$ if and only if $\mathbf v = \mathbf 0$. [Positivity property]

#### Proof

##### 1

$$
\mathbf u \cdot \mathbf v = u_1\overline{v_1} + u_2\overline{v_2} +\cdots+ u_n\overline{v_n}\\=\overline{\overline{u_1}v_1 + \overline{u_2}v_2 +\cdots+ \overline{u_n}v_n}=\overline{\mathbf v \cdot \mathbf u}
$$

##### 2, 3, 5

`pass`

##### 4

$$
\mathbf u \cdot k\mathbf v = \mathbf u^T\overline{k\mathbf v}=\overline{k}(\mathbf u^T\overline{\mathbf v})=
\overline{k}(\mathbf u \cdot \mathbf v)
$$

**$\mathbb R^n$ is not a subspace of $\mathbb C^n$! (not close under scalar multiplication)**

### THEOREM 5.3.4

If $\lambda$ is an eigenvalue of a *real* $n\times n$ matrix $A$, and if $\mathbf x$ is a corresponding eigenvector, then $\overline{\lambda}$ is also an eigenvalue of $A$, and $\overline{\mathbf x}$ is a corresponding eigenvector.

#### Proof

$$
\begin{align*}
A\mathbf x&=\lambda\mathbf x&(\mathbf x\ne\mathbf 0)\\
\overline{A\mathbf x}&=\overline{A}\overline{\mathbf x}=A\overline{\mathbf x}\\
\overline{\lambda\mathbf x}&=\overline{\lambda}\overline{\mathbf x}\\
A\overline{\mathbf x}&=\overline{\lambda}\overline{\mathbf x}\\
\end{align*}
$$

### THEOREM 5.3.5

If $A$ is a $2\times2$ matrix with real entries, then the characteristic equation of $A$ is $\lambda^2 − \text{tr}(A)\lambda + \det(A) = 0$ and
$$
\lambda_1,\lambda_2=\frac{\text{tr}(A)\pm\sqrt{\text{tr}^2(A)-4\det(A)}}{2}=\frac{ad\pm\sqrt{a^2d^2-4(ad-bc)}}{2}
$$

### THEOREM 5.3.6

If $A$ is a real symmetric matrix, then $A$ has real eigenvalues

#### Proof

$$
A\mathbf x=\lambda\mathbf x\quad(\lambda\in\mathbb C, \mathbf x\in\mathbb C^n, \mathbf x \ne \mathbf 0)
$$

$$
\overline{\mathbf x}^TA\mathbf x=\overline{\mathbf x}^T(\lambda\mathbf x)=\lambda\|\mathbf x\|^2\\
\lambda=\frac{\overline{\mathbf x}^TA\mathbf x}{\|\mathbf x\|^2}
$$

Since
$$
\overline{\overline{\mathbf x}^TA\mathbf x}=
\overline{\overline{\mathbf x}}^T\overline{A\mathbf x}=
(\overline{A\mathbf x})^T\mathbf x=
(A\overline{\mathbf x})^T\mathbf x=
\overline{\mathbf x}^TA^T\mathbf x=
\overline{\mathbf x}^TA\mathbf x
$$
Then $\overline{\mathbf x}^TA\mathbf x$ is real, then $\lambda$ is real.

### THEOREM 5.3.7 几何角度看复特征值 A Geometric Interpretation of Complex Eigenvalues

The eigenvalues of the real matrix
$$
C=\begin{bmatrix}a&-b\\b&a\end{bmatrix}
$$
are $\lambda=a\pm bi$. If $a,b$ are not both $0$, then
$$
\begin{bmatrix}a&-b\\b&a\end{bmatrix}=\begin{bmatrix}|\lambda|&0\\0&|\lambda|\end{bmatrix}\begin{bmatrix}\cos\phi&-\sin\phi\\\sin\phi&\cos\phi\end{bmatrix}
$$
$\phi=\angle(a+bi)$.

#### Proof

Since $\lambda=a\pm bi$ Easy to get $a=|\lambda|\cos\phi, b=|\lambda|\sin\phi$, then
$$
\begin{bmatrix}a&-b\\b&a\end{bmatrix}=\begin{bmatrix}|\lambda|&0\\0&|\lambda|\end{bmatrix}\begin{bmatrix}\frac{a}{|\lambda|}&-\frac{b}{|\lambda|}\\\frac{b}{|\lambda|}&\frac{a}{|\lambda|}\end{bmatrix}=\begin{bmatrix}|\lambda|&0\\0&|\lambda|\end{bmatrix}\begin{bmatrix}\cos\phi&-\sin\phi\\\sin\phi&\cos\phi\end{bmatrix}
$$

### THEOREM 5.3.8

Let $A$ be a real $2\times2$ matrix with complex eigenvalues $\lambda=a\pm bi$ (where $b\ne0$). If $\mathbf x$ is an eigenvector of $A$ corresponding to $\lambda=a-bi$, then the matrix $P = \begin{bmatrix}\Re e(\mathbf x)&\Im m(\mathbf x)\end{bmatrix}$ is invertible and
$$
A=P\begin{bmatrix}a&-b\\b&a\end{bmatrix}P^{-1}=P\begin{bmatrix}|\lambda|&0\\0&|\lambda|\end{bmatrix}\begin{bmatrix}\cos\phi&-\sin\phi\\\sin\phi&\cos\phi\end{bmatrix}P^{-1}=PSR_{\phi}P^{-1}
$$

#### Proof

Let $\mathbf u=\Re e(\mathbf x), \mathbf v=\Im m(\mathbf x)$
$$
\begin{align*}
A\mathbf x&=\lambda\mathbf x\\
A\mathbf x&=a\mathbf x-ib\mathbf x\\
A\mathbf u+iA\mathbf v&=a\mathbf u+ia\mathbf v+b\mathbf v-ib\mathbf u\\
A\mathbf u+iA\mathbf v&=a\mathbf u+b\mathbf v+i(-b\mathbf u+a\mathbf v)\\
A\begin{bmatrix}\mathbf u&\mathbf v\end{bmatrix}&=\begin{bmatrix}\mathbf u&\mathbf v\end{bmatrix}\begin{bmatrix}a&-b\\b&a\end{bmatrix}\\
AP&=P\begin{bmatrix}a&-b\\b&a\end{bmatrix}
\end{align*}
$$
If $P$ is not invertible, then $\mathbf u=c\mathbf v, c\in\mathbb R$, then
$$
cA\mathbf v+iA\mathbf v=a\mathbf u+b\mathbf v+i(-b\mathbf u+a\mathbf v)=(ac+b)\mathbf v+i(-bc+a)\mathbf v\\
\frac{ac+b}{c}=\frac{-bc+a}{1}\\
ac+b=-bc^2+ac\\
(1+c^2)b=0
$$
then $b=0$. But $b\ne0$, so t this leads to a contradiction, thereby proving that $P$ is invertible.

> 几何意义：
>
> 1. $P^{-1}\mathbf x_0$ 把 $\mathbf x_0$ 从标准基所在空间映射到另一个空间。
> 2. $SR_{\phi}P^{-1}\mathbf x_0$ 先旋转后伸缩这个向量。
> 3. $PSR_{\phi}P^{-1}\mathbf x_0$ 映射回去。

## 5.4 微分方程 Differential Equations

`pass`

## 5.5 动态系统与马尔可夫链 Dynamical Systems and Markov Chains

From linear algebra viewpoint.

### 动态系统 Dynamical Systems

A *dynamical system* is a finite set of variables whose values change with time.

The value of a variable at a point in time is called 该时间的状态变量 *the state of the variable at that time*, and the vector formed from these states is called 该时间的状态向量 *the state vector of the dynamical system at that time*.

### 马尔可夫链 Markov Chains

In many dynamical systems the states of the <u>variables are not known with certainty but can be expressed as probabilities</u>; such dynamical systems are called 随机过程 *stochastic processes*.

In a stochastic process with $n$ possible states, the state vector (or so called *probability vector*) at each time $t$ has the form
$$
\mathbf x(t)=\begin{bmatrix}x_1(t)\\x_2(t)\\\vdots\\x_n(t)\end{bmatrix}
$$
$x_i(t)$ is the probability that the system is in state $i$. Some restriction: $x_i(t)\ge0,\sum x_i(t)=1$.

### DEFINITION 1 Markov chain

A Markov chain is a dynamical system whose state vectors at a succession of equally spaced times are probability vectors and for which the state vectors at successive times are related by an equation of the form
$$
\mathbf x(k + 1) = P_{\mathbf x}(k)
$$
in which $P$ is a stochastic matrix and $p_{ij}$ is the probability that the system will be in state $i$ at time $t = k + 1$ if it is in state $j$ at time $t = k$. The matrix $P$ is called the 转移矩阵 *transition matrix* for the system.

> 行索引 $i$ 对应了新状态，列索引 $j$ 对应了旧状态。

> Example: Wildlife Migration as a Markov Chain
> $$
> P=\begin{bmatrix}0.5&0.4&0.6\\0.2&0.2&0.3\\0.3&0.4&0.1\end{bmatrix}
> $$
> $p_{ij}$ is the probability that the lion will move from reserve $j$ to reserve $i$.
>
> Let 
> $$
> \mathbf x(k)=\begin{bmatrix}x_1(k)\\x_2(k)\\x_3(k)\end{bmatrix}
> $$
> be  the probabilities that the lion is in reserve 1, 2,or 3.
>
> Suppose  the lion is in reserve $2$ at time $t = 0$, the initial state vector is
> $$
> \mathbf x(0)=\begin{bmatrix}0\\1\\0\end{bmatrix}
> $$
> Then 
> $$
> \mathbf x(k)=P^{k}\mathbf x(0)
> $$

**A Markov chain may not be stable at long term.**

### DEFINITION 2

A stochastic matrix $P$ is said to be regular if $P$ or some positive power of P *has all **positive** entries*, and a Markov chain whose transition matrix is regular is said to be a regular Markov chain.

### THEOREM 5.5.1

If $P$ is the transition matrix for a regular Markov chain, then:

1. There is a unique probability vector $\mathbf q$ with positive entries such that $P\mathbf q = \mathbf q$.

2. For any initial probability vector $\mathbf x_0$,
   $$
   \lim_{k\to\infty}P^k\mathbf x_0=\mathbf q
   $$
3. The sequence $P,P^2, P^3,\dots,P^k,\dots$ converges to the matrix $Q$ each of whose column vectors is $\mathbf q$.

The vector $\mathbf q$ is called the **steady-state** vector of the Markov chain.

