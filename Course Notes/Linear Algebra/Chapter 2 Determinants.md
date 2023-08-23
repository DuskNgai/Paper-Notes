# Chapter 2 Determinants

## 2.1 余子式展开的行列式 Determinants by Cofactor Expansion

### DEFINITION 1

If $A$ is a square matrix, then the *minor of entry* $a_{ij}$ is denoted by $M_{ij}$ and is defined to be the determinant of the submatrix that remains after the $i^{th}$ row and $j^{th}$ th column are deleted from $A$. The number $(-1)^{i+j}M_{ij}$ is denoted by $C_{ij}$ and is called the *cofactor of entry* $a_{ij}$.

Sign patter checkerboard
$$
\begin{bmatrix}
+&-&+&\cdots\\
-&+&-&\cdots\\
+&-&+&\cdots\\
\vdots&\vdots&\vdots\\
\end{bmatrix}
$$

> Eg. For $2\times2$ matrix $A$
> $$
> \begin{align*}
> &C_{11}=M_{11}=a_{22}&&C_{12}=-M_{12}=-a_{21}\\
> &C_{21}=-M_{21}=-a_{12}&&C_{22}=M_{22}=a_{11}
> \end{align*}\\
> $$
> Then we define the *cofactor expansion of $\det(A)$*:
> $$
> \begin{align*}
> |A|&=\begin{vmatrix}a_{11}&a_{12}\\a_{21}&a_{22}\end{vmatrix}\\
> &=a_{11}C_{11}+a_{12}C_{12}\\
> &=a_{11}C_{11}+a_{22}C_{22}\\
> &=a_{11}C_{11}+a_{21}C_{21}\\
> &=a_{12}C_{12}+a_{22}C_{22}\\
> \end{align*}\\
> $$

### DEFINITION 2

If $A$ is an $n\times n$ matrix, then the number obtained by multiplying the entries in any row or column of $A$ by the corresponding cofactors and adding the resulting products is called the *determinant of A*, and the sums themselves are called *cofactor expansions of A*. That is,
$$
\det(A)=\sum_{i=1}^{n}a_{ij}C_{ij}=a_{1j}C_{1j}+a_{2j}C_{2j}+\cdots+a_{nj}C_{nj}\\
\text{[cofactor expansion along the jth column]}\\
\det(A)=\sum_{j=1}^{n}a_{ij}C_{ij}=a_{i1}C_{i1}+a_{i2}C_{i2}+\cdots+a_{in}C_{in}\\
\text{[cofactor expansion along the jth column]}
$$

### THEOREM 2.1.2

If $A$ is an $n\times n$ triangular matrix (upper triangular, lower triangular, or diagonal), then $\det(A)$ is the product of the entries on the main diagonal of the matrix; that is, $\det(A)=\prod_{i=1}^{n}a_{ii}$.

## 2.2 通过行简化计算行列式 Evaluating Determinants by Row Reduction

### THEOREM 2.2.1

Let $A$ be a square matrix. If $A$ has a row of 0s or a column of 0s, then $\det(A) = 0$.

#### Proof:

Definition 2

### THEOREM 2.2.2

Let A be a square matrix. Then $\det(A) = \det(A^T )$.

#### Proof:

Definition 2

### THEOREM 2.2.3 Elementary Row Operations

Let $A$ be an $n\times n$ matrix.

1. If $B$ is the matrix that results when a single row or single column of $A$ is multiplied by a scalar $k$, then $\det(B) = k \det(A)$.
2. If $B$ is the matrix that results when two rows or two columns of $A$ are interchanged, then $\det(B) = −\det(A)$.
3. If $B$ is the matrix that results when a multiple of one row of $A$ is added to another or when a multiple of one column is added to another, then $\det(B) = \det(A)$.

#### Proof:

definition 2

### Elementary Matrix

Let $E$ be an $n\times n$ elementary matrix.

1. If $E$ results from multiplying a row of $I_n$ by a scalar $k$, then $\det(E) = k$.
2. If $E$ results from interchanging two rows of $I_n$, then $\det(E) = −1$.
3. If $E$ results from adding a multiple of one row of $I_n$ to another, then $\det(E) = 1$.

$$
\begin{vmatrix}
1&0&0&0\\
0&3&0&0\\
0&0&1&0\\
0&0&0&1
\end{vmatrix}=3\quad
\begin{vmatrix}
0&0&0&1\\
0&1&0&0\\
0&0&1&0\\
1&0&0&0
\end{vmatrix}=-1\quad
\begin{vmatrix}
1&0&0&1\\
0&1&0&0\\
0&0&1&0\\
0&0&0&1
\end{vmatrix}=1
$$

### THEOREM 2.2.5 

If A is a square matrix with two proportional rows or two proportional columns, then $\det(A)=0$.

## 2.3 行列式性质；克莱姆法则 Properties of Determinants; Cramer's Rule

### Basic Properties of Determinants

$$
\det(kA)=k^n\det(A)
$$

#### Proof

$$
\det(kA)=\sum_{i=1}^nka_{i1}C_{i1}=\cdots=k^n\det(A)
$$

$$
\det(A+B)\ne\det(A)+\det(B)
$$

Let $A$, $B$, and $C$ be $n\times n$ matrices that differ only in a single row, say the $r^{th}$, and assume that the $r^{th}$ row of $C$ can be obtained by adding corresponding entries in the $r^{th}$ rows of $A$ and $B$. Then $\det(C) = \det(A) + \det(B)$. The same result holds for columns.

#### Proof

$$
\begin{align*}
\det(C)&=\sum_{i=1}^nc_{i1}C_{i1}\sum_{i=1}^nc_{i1}C_{i1}\\
&=\sum_{i=1}^na_{i1}C_{i1}+\sum_{i=1}^nb_{i1}C_{i1}\\
&=\det(A)+\det(B)
\end{align*}
$$

$$
   \det(AB)=\det(A)\det(B)
$$

### LEMMA 2.3.2

If $B$ is an $n \times n$ matrix and $E$ is an $n \times n$ elementary matrix, then $\det(EB) = \det(E)\det(B)$.

#### Proof:

If $E$ results from multiplying a row of $I_n$ by $k$, then $EB$ results from $B$ by multiplying the corresponding row by $k$, then $\det(EB) = \det(E)\det(B)$.

Other cases are similar.

Then $\det(E_1E_2\dots E_rB) = \det(E_1)\det(E_2)\dots\det(E_r)\det(B)$

### THEOREM 2.3.3

A square matrix $A$ is invertible if and only if $\det(A) = 0$.

#### Proof

$A$ can be reduced to row reduction echelon form $R$:
$$
R=E_1E_2\dots E_rA
$$
Then
$$
\det(R)=\det(E_1)\det(E_2)\dots\det(E_r)\det(A)
$$
Then $\det(R)=\det(A)=0$ only when $A$ is invertible.

### THEOREM 2.3.4

If $A$ and $B$ are square matrices of the same size, then $\det(AB) = \det(A) \det(B)$.

#### Proof

If $A$ or $B$ is invertible, then $AB$ is invertible. Then $\det(AB) = \det(A) \det(B)=0$.

Otherwise, $A$ can be represents as a multiplication of elementary matrices, then $\det(AB) = \det(A) \det(B)$.

### THEOREM 2.3.5

If $A$ is invertible, then $\det(A^{-1}) = \frac{1}{\det(A)}$

#### Proof

$$
\det(A^{-1})\det(A)=\det(A^{-1}A)=\det(I)=1
$$

### Adjoint of a Matrix

If $A$ is any $n \times n$ matrix and $C_{ij}$ is the cofactor of $a_{ij}$ , then the matrix

$$
\begin{bmatrix}
C_{11}&C_{21}&\cdots&C_{n1}\\
C_{12}&C_{22}&\cdots&C_{n2}\\
\vdots&\vdots&&\vdots\\
C_{1n}&C_{2n}&\cdots&C_{nn}
\end{bmatrix}
$$
is called the adjoint of $A$ and is denoted by $\text{adj}(A)$.

### THEOREM 2.3.6

If $A$ is an invertible matrix, then $A^{-1}=\frac{1}{\det(A)}\text{adj}(A)$

#### Proof

$$
\begin{align*}
A\ \text{adj}(A)_{(ij)}&=\sum_{k=1}^{n}A_{(ik)}\text{adj}(A)_{(kj)}\\
&=\sum_{k=1}^{n}A_{(ik)}C_{(jk)}\\
&=\begin{cases}
\det(A)&i=j\\
0&i\ne j
\end{cases}\\
\end{align*}\\
A\ \text{adj}(A)=\det(A)I
$$

---

Consider a matrix $B$ that replace the $j^{th}$ row to the $i^{th}$ row in $A$. Then 
$$
\begin{align*}
\sum_{k=1}^{n}A_{(ik)}C_{(jk)}&=\sum_{k=1}^{n}A_{(jk)}C_{(jk)}=\det(B)=0
\end{align*}\\
$$

---

Easy to get $\det(\text{adj}(A))=[\det(A)]^{n-1}$.

### THEOREM 2.3.7 Cramer's Rule

If $A\mathbf x =\mathbf b$ is a system of $n$ linear equations in $n$ unknowns such that $\det(A) = 0$, then the system has a unique solution. This solution is
$$
x_j=\frac{\det(A_j)}{\det(A)}
$$
where $A_j$ is the matrix obtained by replacing the entries in the $j^{th}$ column of $A$ by the
entries in the vector $\mathbf b$.

#### Proof

$$
\begin{align*}
\mathbf x&=A^{-1}\mathbf b\\
&=\frac{1}{\det(A)}\text{adj}(A)\mathbf b\\
&=\frac{1}{\det(A)}
\begin{bmatrix}
C_{11}&C_{21}&\cdots&C_{n1}\\
C_{12}&C_{22}&\cdots&C_{n2}\\
\vdots&\vdots&&\vdots\\
C_{1n}&C_{2n}&\cdots&C_{nn}
\end{bmatrix}
\begin{bmatrix}
b_1\\
b_2\\
\vdots\\
b_n
\end{bmatrix}\\
&=\frac{1}{\det(A)}
\begin{bmatrix}
b_1C_{11}+b_2C_{21}+\cdots+b_nC_{n1}\\
b_1C_{12}+b_2C_{22}+\cdots+b_nC_{n2}\\
\vdots\\
b_1C_{1n}+b_2C_{2n}+\cdots+b_nC_{nn}
\end{bmatrix}
\end{align*}\\
x_j=\frac{\sum_{i=1}^nb_{ij}C_{ij}}{\det(A)}=\frac{\det(A_j)}{\det(A)}
$$

