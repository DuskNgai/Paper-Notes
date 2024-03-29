# Chapter 6 Inner Product Spaces

## 6.1 内积 Inner Products

### DEFINITION 1

An inner product on a **real vector** space $V$ is a function that associates a real number $\langle\mathbf u, \mathbf v\rangle$ with each pair of vectors in $V$ in such a way that the following axioms are satisfied for all vectors $\mathbf u$, $\mathbf v$, and $\mathbf w$ in $V$ and all scalars $k$.

1. $\langle\mathbf u, \mathbf v\rangle=\langle\mathbf v, \mathbf u\rangle$ [Symmetry axiom]
2. $\langle\mathbf u+\mathbf v,\mathbf w\rangle=\langle\mathbf u, \mathbf w\rangle+\langle\mathbf v, \mathbf w\rangle$ [Additivity axiom]
3. $\langle k\mathbf u, \mathbf v\rangle = k\langle\mathbf u, \mathbf v\rangle$ [Homogeneity axiom]
4. $\langle\mathbf v, \mathbf v\rangle \ge 0$ and $\langle\mathbf v, \mathbf v\rangle = 0$ if and only if $\mathbf v = \mathbf 0$ [Positivity axiom]

A real vector space with an inner product is called a 实内积空间 *real inner product space*.

### DEFINITION 2

If $V$ is a real inner product space, then the *norm* of a vector $\mathbf v$ in $V$ is denoted by $\|\mathbf v\|$ and is defined by
$$
\|\mathbf v\|=\sqrt{\langle\mathbf v,\mathbf v\rangle}
$$
and the *distance* between two vectors is denoted by $d(\mathbf u, \mathbf v)$ and is defined by
$$
d(\mathbf u, \mathbf v)=\|\mathbf u-\mathbf v\|=\sqrt{\langle\mathbf u-\mathbf v,\mathbf u-\mathbf v\rangle}
$$
A vector of norm $1$ is called a *unit vector*.

### THEOREM 6.1.1

If $\mathbf u$ and $\mathbf v$ are vectors in a real inner product space $V$, and if $k$ is a scalar, then:

1. $\|\mathbf v\|\ge0$ with equality if and only if $\mathbf v = 0$
2. $\|k\mathbf v\|=|k|\|\mathbf v\|$
3. $d(\mathbf u, \mathbf v) = d(\mathbf v, \mathbf u)$
4. $d(\mathbf u, \mathbf v) \ge 0$ with equality if and only if $\mathbf v=\mathbf u$

### DEFINITION 3

If $V$ is an inner product space, then the set of points in V that satisfy $\|\mathbf u\|=1$ is called the *unit sphere* or sometimes the *unit circle* in $V$.

### 矩阵生成的内积 Inner Products Generated by Matrices

let $\mathbf u$ and $\mathbf v$ be vectors in $\mathbb R^n$, $A$ be an invertible $n\times n$ matrix, define
$$
\langle\mathbf u,\mathbf v\rangle=A\mathbf u\cdot A\mathbf v
$$

1. $$
   \begin{align*}
   \langle\mathbf u,\mathbf v\rangle_i&=(A\mathbf u\cdot A\mathbf v)_i\\
   &=(a_{i1}u_1+\dots+a_{in}u_n)(a_{i1}v_1+\dots+a_{in}v_n)\\
   &=(a_{i1}v_1+\dots+a_{in}v_n)(a_{i1}u_1+\dots+a_{in}u_n)\\
   &=(A\mathbf v\cdot A\mathbf u)_i\\
   &=\langle\mathbf v,\mathbf u\rangle_i
   \end{align*}
   $$

2. $$
   \begin{align*}
   &\quad\langle\mathbf u+\mathbf v,\mathbf w\rangle_i\\
   &=(A(\mathbf u+\mathbf v)\cdot A\mathbf w)_i\\
   &=(a_{i1}(u_1+v_1)+\dots+a_{in}(u_n+v_n))(a_{i1}w_1+\dots+a_{in}w_n)\\
   &=((a_{i1}u_1+\dots+a_{in}u_n)+(a_{i1}v_1+\dots+a_{in}v_n))(a_{i1}w_1+\dots+a_{in}w_n)\\
   &=\langle\mathbf u, \mathbf w\rangle_i+\langle\mathbf v, \mathbf w\rangle_i\\
   \end{align*}
   $$

3. $$
   \langle k\mathbf u, \mathbf v\rangle = A(k\mathbf u)\cdot A\mathbf v = kA\mathbf u\cdot A\mathbf v = k\langle\mathbf u, \mathbf v\rangle
   $$

4. $$
   \begin{align*}
   \langle\mathbf v,\mathbf v\rangle&=(A\mathbf v\cdot A\mathbf v)\\
   &=\sum_{i=1}^{n}(a^2_{i1}v^2_1+\dots+a^2_{in}v^2_n)\\
   &\ge0
   \end{align*}
   $$


### The Standard Inner Product on $M_{nn}$

$$
\langle U,V\rangle=\text{tr}(U^TV)
$$

### The Standard Inner Product on $P_n$

$$
\langle p,q\rangle=\sum_{i=0}^na_ib_i
$$

###  Integral Inner Product on $C[a, b]$

### THEOREM 6.1.2

If $\mathbf u,\mathbf v,\mathbf w$ are vectors in a real inner product space $V$, and if $k$ is a scalar, then:

1. $\langle\mathbf 0, \mathbf v\rangle=\langle\mathbf v, \mathbf 0\rangle=0$
2. $\langle\mathbf u, \mathbf v\pm\mathbf w\rangle=\langle\mathbf u, \mathbf v\rangle\pm\langle\mathbf u, \mathbf w\rangle$
3. $\langle\mathbf u\pm \mathbf v,\mathbf w\rangle=\langle\mathbf u, \mathbf w\rangle\pm\langle\mathbf v, \mathbf w\rangle$
4. $k\langle \mathbf u, \mathbf v\rangle = \langle\mathbf u, k\mathbf v\rangle$

## 6.2 内积空间的角度和正交性 Angle and Orthogonality in Inner Product Spaces

### THEOREM 6.2.1 Cauchy–Schwarz Inequality

### THEOREM 6.2.2 Triangle Inequality

### DEFINITION 1 Orthogonal

### THEOREM 6.2.3 Generalized Theorem of Pythagoras

### DEFINITION 2

If $W$ is a subspace of a real inner product space $V$, then the set of all vectors in $V$ that are orthogonal to every vector in $W$ is called the *orthogonal complement of $W$* and is denoted by the symbol $W^{\perp}$.

### THEOREM 6.2.4

If $W$ is a subspace of a real inner product space $V$, then:

1. $W^{\perp}$ is a subspace of $V$.
2. $W\cap W^{\perp}=\{\mathbf 0\}$

#### Proof

Suppose $\mathbf u, \mathbf w\in W^{\perp}$, $\mathbf v\in W$, $W^\perp$ is closed under addition and scalar multiplication. $W^\perp$ contains $\mathbf 0$. Any vector that is perpendicular to itself is $\mathbf 0$. 

### THEOREM 6.2.5

$(W^\perp)^\perp=W$

## 6.3 Gram–Schmidt 过程；QR 分解 Gram–Schmidt Process; QR-Decomposition

### DEFINITION 1 正交集 Orthogonal Set

A set of two or more vectors in a real inner product space is said to be orthogonal if all pairs of distinct vectors in the set are orthogonal. An **orthogonal set** in which each vector has norm 1 is said to be *orthonormal*.

### THEOREM 6.3.1

If $S = \{v_1, v_2,\dots, v_n\}$ is an <u>orthogonal set</u> of nonzero vectors in an inner product space, then $S$ is linearly independent.

#### Proof

Assume
$$
k_1\mathbf v_1+\dots+k_n\mathbf v_n=\mathbf 0
$$
For each $\mathbf v_i$
$$
\langle k_1\mathbf v_1+\dots+k_n\mathbf v_n,\mathbf v_i\rangle=\langle\mathbf 0,\mathbf v_i\rangle=k_i\langle\mathbf v_i,\mathbf v_i\rangle=0
$$
Since $\mathbf v_i\ne\mathbf 0$, then $k_i=0$, then $S$ is a linear independent set.

### THEOREM 6.3.2

If $S = \{v_1, v_2,\dots, v_n\}$ is an <u>orthogonal basis</u> for an inner product space $V$, and if $\mathbf u$ is any vector in $V$, then
$$
\mathbf u=\frac{\langle \mathbf u,\mathbf v_1\rangle}{\|\mathbf v_1\|^2}\mathbf v_1+\dots+\frac{\langle \mathbf u,\mathbf v_n\rangle}{\|\mathbf v_n\|^2}\mathbf v_n
$$

#### Proof

Suppose 
$$
\mathbf u=c_1\mathbf v_1+\dots+c_n\mathbf v_n
$$
Then
$$
\langle \mathbf u,\mathbf v_i\rangle=\langle c_1\mathbf v_1+\dots+c_n\mathbf v_n,\mathbf v_i\rangle=c_i\langle \mathbf v_i,\mathbf v_i\rangle=c_i\|\mathbf v_i\|^2
$$
So
$$
c_i=\frac{\langle \mathbf u,\mathbf v_i\rangle}{\|\mathbf v_i\|^2}
$$
which completes the proof.

### THEOREM 6.3.3 投影定理 Projection Theorem

If $W$ is a finite-dimensional subspace of an inner product space $V$, then every vector $\mathbf u$ in $V$ can be expressed in exactly one way as
$$
\mathbf u=\mathbf w_1+\mathbf w_2
$$
where $\mathbf w_1\in W, \mathbf w_2\in W^\perp$.

Let $\mathbf w_1=\text{proj}_W\mathbf u,\mathbf w_2=\text{proj}_{W^{\perp}}\mathbf u$.

### THEOREM 6.3.4

Let $W$ be a finite-dimensional subspace of an inner product space $V$.

If $\{v_1, v_2,\dots, v_r\}$ is an orthogonal basis for $W$, and $\mathbf u$ is any vector in $V$, then
$$
\text{proj}_W\mathbf u=\frac{\langle \mathbf u,\mathbf v_1\rangle}{\|\mathbf v_1\|^2}\mathbf v_1+\dots+\frac{\langle \mathbf u,\mathbf v_r\rangle}{\|\mathbf v_r\|^2}\mathbf v_r
$$

### THEOREM 6.3.5 The Gram–Schmidt Process

Every nonzero finite-dimensional inner product space has an orthonormal basis.

> **The Gram–Schmidt Process**
>
> To convert a basis $\{\mathbf u_1, \mathbf u_2,\dots, \mathbf u_r\}$ into an orthogonal basis $\{\mathbf v_1, \mathbf v_2,\dots, \mathbf v_r\}$, perform the following computations:
>
> 1. $\mathbf v_1=\mathbf u_1$
> 2. $\mathbf v_2=\mathbf u_2-\dfrac{\langle \mathbf u_2,\mathbf v_1\rangle}{\|\mathbf v_1\|^2}\mathbf v_1$
> 3. $\mathbf v_3=\mathbf u_3-\dfrac{\langle \mathbf u_3,\mathbf v_1\rangle}{\|\mathbf v_1\|^2}\mathbf v_1-\dfrac{\langle \mathbf u_3,\mathbf v_2\rangle}{\|\mathbf v_2\|^2}\mathbf v_2$
>
> (continue for $r$ steps)
>
> Then normalize them to orthonormal basis.

#### Proof

For the second step:
$$
\begin{align*}
\mathbf u_2&=\text{proj}_{W_1}\mathbf u_2+\text{proj}_{W_1^\perp}\mathbf u_2\\
&=\mathbf v_2+\text{proj}_{W_1}\mathbf u_2\\
&=\mathbf v_2+\dfrac{\langle \mathbf u_2,\mathbf v_1\rangle}{\|\mathbf v_1\|^2}\mathbf v_1
\end{align*}
$$
Where $W_1=\text{span}(\mathbf u_1)$.

### THEOREM 6.3.6

If $W$ is a finite-dimensional inner product space, then every orthogonal set of nonzero vectors in $W$ can be enlarged to an orthogonal basis for $W$.

#### Proof

An orthogonal set is a linear independent set, then it can be extended to a basis of $W$, then apply Grant-Schmidt process, we get an orthogonal basis for $W$.

### QR-Decomposition















