# Chapter 1 Systems of Linear Equations and Matrices

## 1.1 线性方程组介绍 Introduction to Systems of Linear Equations

### 线性方程 linear equation

$$
a_1x_1 + a_2x_2 +\dots+ a_nx_n = b
$$

where $a_1, a_2,\dots,a_n$ and $b$ are constants, and the $a_i$ are not all zero.

### 齐次线性方程 homogeneous linear equation

$$
a_1x_1 + a_2x_2 +\dots+ a_nx_n = 0
$$

### 线性方程组 system of linear equations

A finite set of linear equations. $x_i$ are unknowns.

A general linear system of $m$ equations in the $n$ unknowns can be written as:
$$
a_{11}x_1 + a_{12}x_2 +\dots+ a_{1n}x_n = b_{1}\\
a_{21}x_1 + a_{22}x_2 +\dots+ a_{2n}x_n = b_{1}\\
\vdots\\
a_{m1}x_1 + a_{m2}x_2 +\dots+ a_{mn}x_n = b_{m}
$$
**解 solution:**
$$
x_1 = s_1, x_2 = s_2,\dots, x_n = s_n\qquad(s_1,s_2,\dots,s_n)
$$

### Linear Systems in Two and Three Unknowns

For:
$$
a_1x + b_1y = c_1\\a_2x + b_2y = c_2
$$

1. The lines may be *parallel and distinct*, in which case there is no intersection and consequently *no solution*.
2. The lines may intersect at only *one point*, in which case the system has exactly *one solution*.
3. The lines may *coincide*, in which case there are infinitely many points of intersection(the points on the common line) and consequently *infinitely many solutions*.

相容的 consistent: at least one solution.

不相容的 inconsistent: no solution.

> Every system of linear equations has zero, one, or infinitely many solutions. There are no other possibilities.

### 增广矩阵和基本行操作 Augmented Matrices and Elementary Row Operations

augmented matrix: abbreviation of the linear system

$$
\begin{bmatrix}
a_{11}&a_{12}&\dots&a_{1n}&b_{1}\\
a_{21}&a_{22}&\dots&a_{2n}&b_{2}\\
\vdots&\vdots& &\vdots&\\
a_{m1}&a_{m2}&\dots&a_{mn}&b_{m}\\
\end{bmatrix}
$$
The basic method for solving a linear system: 

1. Multiply an equation through by a nonzero constant.
2. Interchange two equations.
3. Add a constant times one equation to another.

Since the rows (horizontal lines) of an augmented matrix correspond to the equations in the associated system, the **elementary row operations** on a matrix

1. 一行乘以非零常数 Multiply a row through by a nonzero constant.
2. 交换两行 Interchange two rows.
3. 一行的倍数加到令一行上 Add a constant times one row to another

## 1.2 高斯消元法 Gaussian Elimination

### 阶梯型 Echelon Forms

A matrix that is in 简化行阶梯型 *reduced row echelon form*

1. If a row does not consist entirely of zeros, then the first nonzero number in the row is a 1. We call this a *leading 1*.
2. If there are any rows that consist entirely of 0s, then they are grouped together at the bottom of the matrix.
3. In any two successive rows that do not consist entirely of 0s, the leading 1 in the lower row occurs farther to the right than the leading 1 in the higher row.
4. Each column that contains a leading 1 has 0s everywhere else in that column.

A matrix that has the first three properties is said to be in 行阶梯型 *row echelon form*.

$$
\underbrace{\begin{bmatrix}
0&1&*&*&*&*&*&*&*&*\\
0&0&0&1&*&*&*&*&*&*\\
0&0&0&0&1&*&*&*&*&*\\
0&0&0&0&0&1&*&*&*&*\\
0&0&0&0&0&0&0&0&1&*
\end{bmatrix}}_{\text{row echelon form}}\qquad
\underbrace{\begin{bmatrix}
0&1&*&0&0&0&*&*&0&*\\
0&0&0&1&0&0&*&*&0&*\\
0&0&0&0&1&0&*&*&0&*\\
0&0&0&0&0&1&*&*&0&*\\
0&0&0&0&0&0&0&0&1&*
\end{bmatrix}}_{\text{reduced row echelon form}}
$$

> 高斯消元法步骤：
>
> Step 1. Locate the leftmost *column* that does not consist entirely of 0.
> $$
> \begin{bmatrix}
> 0&0&−2&0&7&12\\
> 2&4&−10&6&12&28\\
> 2&4&−5&6&−5&−1
> \end{bmatrix}
> $$
> Step 2. Interchange the top row with another row, if necessary, to bring a nonzero entry to the top of the column found in Step 1.
> $$
> \begin{bmatrix}
> 2&4&−10&6&12&28\\
> 0&0&−2&0&7&12\\
> 2&4&−5&6&−5&−1
> \end{bmatrix}
> $$
> Step 3. If the entry that is now at the top of the column found in Step 1 is *a*, multiply the first row by *1/a* in order to introduce a leading 1.
> $$
> \begin{bmatrix}
> 1&2&−5&3&6&14\\
> 0&0&−2&0&7&12\\
> 2&4&−5&6&−5&−1
> \end{bmatrix}
> $$
> Step 4. Add suitable multiples of the top row to the rows below so that all entries below the leading 1 become 0.
> $$
> \begin{bmatrix}
> 1&2&−5&3&6&14\\
> 0&0&−2&0&7&12\\
> 0&0&5&0&−17&−29
> \end{bmatrix}
> $$
> Step 5. Again with Step 1 applied to the submatrix that remains. Continue in this way until the *entire* matrix is in row echelon form.
> $$
> \begin{bmatrix}
> 1&2&-5&3&6&14\\
> 0&0&1&0&-\frac{7}{2}&-6\\
> 0&0&0&0&1&2
> \end{bmatrix}
> $$
> Step 6. Beginning with the last nonzero row and working upward, add suitable multiples of each row to the rows above to introduce zeros above the leading 1’s.
> $$
> \begin{bmatrix}
> 1&2&0&3&0&7\\
> 0&0&1&0&0&1\\
> 0&0&0&0&1&2
> \end{bmatrix}
> $$

#### 解的情况

1. 如果最后一行存在类似
   $$
   0x+0y+0z=c\qquad(c\ne0)
   $$
   则该方程组无解。

2. 如果最后一行存在类似
   $$
   0x+0y+0z=0
   $$
   则该行可以忽略，对解没有影响。

3. Variable correspond to the leading 1’s in the augmented matrix are the *leading variables*. The remaining variables are called *free variables*.

   > So the solution can be expressed as:
   > $$
   > x_1=7-2x_2-3x_4(x_2,x_4\in\mathbb R)\quad x_3=1\quad x_5=2
   > $$


### 齐次线性方程组 Homogeneous Linear Systems

$$
a_{11}x_1 + a_{12}x_2 +\dots+ a_{1n}x_n = 0\\
a_{21}x_1 + a_{22}x_2 +\dots+ a_{2n}x_n = 0\\
\vdots\\
a_{m1}x_1 + a_{m2}x_2 +\dots+ a_{mn}x_n = 0
$$

$x_1= 0, x_2 = 0,\dots,x_n = 0$ is always a solution, called 平凡解 *trivial solution*. If there are other solutions, they are called 非平凡解 *nontrivial solutions*.

Now consider a general homogeneous linear system with *n* unknowns, and suppose that the reduced row echelon form of the augmented matrix has *r* nonzero rows.
$$
\begin{bmatrix}
x_{k_1}&&&&+ \sum( ) & 0\\
&x_{k_2}&&&+ \sum( ) & 0\\
&&\ddots&&\vdots\\
&&&x_{k_r}&+ \sum( ) & 0
\end{bmatrix}
$$

### THEOREM 1.2.1 Free Variable Theorem for Homogeneous Systems

If a homogeneous linear system has $n$ unknowns, and if the reduced row echelon form of its augmented matrix has $r$ nonzero rows, then the system has $n-r$ free variables.

### THEOREM 1.2.2

A homogeneous linear system with *more* unknowns than equations has infinitely many solutions.



 3 facts about row echelon forms and reduced row echelon forms:

1. Every matrix has a unique reduced row echelon form; that is, regardless of whether you use Gauss–Jordan elimination or some other sequence of elementary row operations, the same reduced row echelon form will result in the end.
2. Row echelon forms are not unique; that is, different sequences of elementary row operations can result in different row echelon forms.
3. Although row echelon forms are not unique, the reduced row echelon form and all row echelon forms of a matrix $A$ have the same number of zero rows, and the leading 1’s always occur in the same positions. Those are called the pivot positions of $A$. A column that contains a pivot position is called a pivot column of $A$.

## 1.3 矩阵和矩阵操作 Matrices and Matrix Operations

### Matrices

Definition1: A matrix is a rectangular array of numbers. The numbers in the array are called the *entries* in the matrix.

Size: number of rows and columns.

A matrix with $n$ rows and $n$ columns is said to be a n阶方阵 square matrix of order $n$.

A matrix with only one row is called a row vector and a matrix with only one column is called a column vector.

### Matrix Operations

*Equal*: $a_{ij}=b_{ij}$

*Addition and Subtraction*: $(a\pm b)_{ij}=a_{ij}\pm b_{ij}$

*Scalar product*: $c(a_{ij})=ca_{ij}$

*Matrix product*: $c_{ij}=\sum_{k}a_{ik}b_{kj}$

*Partition*: divide into submatrices and/or vectors

### 矩阵乘法的行列观点 Matrix Multiplication by Columns and by Rows

$$
AB=A\begin{bmatrix}
\mathbf{b_1}&\mathbf{b_2}&\cdots&\mathbf{b_n}
\end{bmatrix}=\begin{bmatrix}
A\mathbf{b_1}&A\mathbf{b_2}&\cdots&A\mathbf{b_n}
\end{bmatrix}\\
=\begin{bmatrix}
\mathbf{a_1}\\\mathbf{a_2}\\\vdots\\\mathbf{a_m}
\end{bmatrix}B=\begin{bmatrix}
\mathbf{a_1}B\\\mathbf{a_2}B\\\vdots\\\mathbf{a_m}B
\end{bmatrix}
$$

---

### DEFINITION 6

If $A_1, A_2,\dots,A_r$ are matrices of the same size, and if $c_1, c_2,\dots,c_r$ are scalars, then 
$$
c_1A_1+c_2A_2+\dots+c_rA_r
$$
is called a 线性组合 *linear combination* of $A_1, A_2,\dots,A_r$ with 系数 *coeffificients* $c_1, c_2,\dots,c_r$.

This is the same for column vector.

### THEOREM 1.3.1

If $A$ is an $m \times n$ matrix, and if $\mathbf{x}$ is an $n \times 1$ column vector, then the product $A\mathbf{x}$ can be expressed as a linear combination of the column vectors of $A$ in which the coeffificients are the entries of $\vec{x}$.
$$
A\mathbf{x}=
\begin{bmatrix}
a_{11}x_1 + a_{12}x_2 +\dots+ a_{1n}x_n\\
a_{21}x_1 + a_{22}x_2 +\dots+ a_{2n}x_n\\
\vdots\\
a_{m1}x_1 + a_{m2}x_2 +\dots+ a_{mn}x_n
\end{bmatrix}
=x_1\begin{bmatrix}
a_{11}\\
a_{21}\\
\vdots\\
a_{m1}
\end{bmatrix}+
x_2\begin{bmatrix}
a_{12}\\
a_{22}\\
\vdots\\
a_{m2}
\end{bmatrix}+\dots+
x_n\begin{bmatrix}
a_{1n}\\
a_{2n}\\
\vdots\\
a_{mn}
\end{bmatrix}
$$
*column-row expansion* of $AB$: If $A$ is an $m \times r$ matrix that is partitioned into its $r$ column vectors $\mathbf{c_1},\mathbf{c_2},\dots,\mathbf{c_r}$ (each of size $m \times 1$) and $B$ is an $r \times n$ matrix that is partitioned into its $r$ row vectors $\mathbf{r_1},\mathbf{r_2},\dots,\mathbf{r_r}$ (each of size $1 \times n$).
$$
AB=\mathbf{c_1}\mathbf{r_1}+\mathbf{c_2}\mathbf{r_2}+\dots+\mathbf{c_r}\mathbf{r_r}
$$

### 转置 Transpose

If $A$ is any $m \times n$ matrix, then the transpose of $A$, denoted by $A^T$ , is defined to be the $n \times m$ matrix that results by interchanging the rows and columns of $A$.
$$
A =\begin{bmatrix}
a_{11}& a_{12}& a_{13}& a_{14}\\
a_{21}& a_{22}& a_{23}& a_{24}\\
a_{31}& a_{32}& a_{33}& a_{34}
\end{bmatrix}
A^T=\begin{bmatrix}
a_{11}& a_{21}& a_{31}\\
a_{12}& a_{22}& a_{32}\\
a_{13}& a_{23}& a_{33}\\
a_{14}& a_{24}& a_{34}
\end{bmatrix}
$$
*Transpose*: $(A^T)_{ij}=(A)_{ji}=a_{ji}$

## 1.4 逆; 矩阵的代数性质 Inverses; Algebraic Properties of Matrices

### THEOREM 1.4.1 Properties of Matrix Arithmetic

$$
\begin{align*}
(a)&\ A + B = B + A &\text{ [Commutative law for matrix addition]}\\
(b)&\ A + (B + C) = (A + B) + C &\text{ [Associative law for matrix addition]}\\
(c)&\ A(BC) = (AB)C &\text{ [Associative law for matrix multiplication]}\\
(d)&\ A(B \pm C) = AB \pm AC &\text{ [Left distributive law]}\\
(e)&\ (B \pm C)A = BA \pm CA &\text{ [Right distributive law]}\\
(f)&\ \lambda (B \pm C) = \lambda B \pm \lambda C\\
(g)&\ (\lambda + \mu)C = \lambda C + \mu C\\
(h)&\ (\lambda − \mu)C = \lambda C − \mu C\\
(i)&\ \lambda(\mu C) = (\lambda \mu)C\\
(j)&\ \lambda(BC) = (\lambda B)C = B(\lambda C)
\end{align*}
$$

### THEOREM 1.4.2 Properties of Zero Matrices

A matrix whose entries are all zero is called a 零矩阵 *zero matrix*.
$$
\begin{align*}
(a)&\ A + 0 = 0 + A = A\\
(b)&\ A − 0 = A\\
(c)&\ A − A = A + (−A) = 0\\
(d)&\ 0A = 0\\
(e)&\ \text{If }\lambda A = 0\text{, then }\lambda = 0\text{ or }A = 0.
\end{align*}
$$

#### 消去律的失效 Failure of the Cancellation Law

$$
AB=AC\nRightarrow B=C
$$

#### 非0因数的乘积为0 A Zero Product with Nonzero Factors

$$
A=\begin{bmatrix}
0&1\\
0&2
\end{bmatrix}
B=\begin{bmatrix}
3&7\\0&0
\end{bmatrix}
AB=0
$$

### Identity Matrices

A square matrix with 1’s on the *main diagonal* and zeros elsewhere is called an identity matrix. Denoted $I_n$ for the $n\times n$ identity matrix.
$$
\begin{bmatrix}
1&0\\
0&1
\end{bmatrix}
\begin{bmatrix}
1&0&1\\
0&1&0\\
0&0&1
\end{bmatrix}
$$

$$
AI_n = A\text{ and }I_mA = A
$$

### THEOREM 1.4.3

If $R$ is the reduced row echelon form of an $n\times n$ matrix $A$, then either $R$ has a row of zeros or $R$ is the identity matrix $I_n$.

#### Proof

If last row contains all 0s, then we done.

Else, the 1s must lay on the main diagonal, so $R=I_n$. 

### Inverse of a Matrix

If $A$ is a *square matrix*, and if a matrix $B$ of the same size can be found such that $AB=BA=I$ , then $A$ is said to be *invertible* (or *non-singular*) and $B$ is called an *inverse* of $A$. If no such matrix $B$ can be found, then $A$ is said to be *non-invertible*(or *singular*).

### Properties of Inverses

### THEOREM 1.4.4

If $B$ and $C$ are both inverses of the matrix $A$, then $B = C$.

#### Proof

$$
B=BI=B(AC)=(BA)C=IC=C
$$

So: If $A$ is invertible, then its inverse will be denoted by the symbol $A^{−1}$.

### THEOREM 1.4.6

 $A$ and $B$ are invertible matrices with the same size,  $AB$ is invertible and
$$
(AB)^{-1}=B^{-1}A^{-1}
$$

#### Proof

$$
(AB)(B^{-1}A^{-1})=ABB^{-1}A^{-1}=AIA^{-1}=I\\
(B^{-1}A^{-1})(AB)=B^{-1}A^{-1}AB=B^{-1}IB=I
$$

### Powers of a Matrix

If $A$ is a *square* matrix:
$$
A^0=I,A^n=\underbrace{A\cdot A\cdots A}_{n}\\
A^rA^s = A^{r+s}\\
(A^r)^s = A^{rs}
$$
And if $A$ is invertible
$$
A^{-n}=(A^{-1})^n=\underbrace{A^{-1}\cdot A^{-1}\cdots A^{-1}}_{n}
$$

### THEOREM 1.4.7

If $A$ is invertible and $n$ is a nonnegative integer:

1. $A^{-1}$ is invertible and $(A^{−1})^{−1}=A$.
2. $A^n$ is invertible and $(A^n)^{−1}=A^{−n}=(A^{−1})^{n}$.
3. $kA(k\ne0)$ is invertible and $(kA)^{−1}=k^{−1}A^{−1}$.

### The Square of a Matrix Sum

$$
(A + B)^2 = A^2 + AB + BA + B^2
$$

### Matrix Polynomials

If $A$ is a *square* matrix:
$$
p(A) = a_0I + a_1A + a_2A^2 +\dots+ a_mA^m
$$
And
$$
p_1(A)p_2(A)=p_2(A)p_1(A)
$$

### Properties of the Transpose

$$
\begin{align*}
1.&\ (A^T)^T=A\\
2.&\ (A \pm B)^T = A^T \pm B^T\\
3.&\ (kA)^T = kA^T\\
4.&\ (AB)^T = B^TA^T
\end{align*}
$$

### THEOREM 1.4.9

If $A$ is an invertible matrix, then $A^T$ is also invertible and
$$
(A^T)^{−1} = (A^{−1})^T
$$

#### Proof

$$
(A^{T})^{-1}A^{T}=A^T(A^{T})^{-1}=I\\
A^T(A^{-1})^T=(A^{-1}A)^T=I^T=I\\
(A^{-1})^TA^T=(AA^{-1})^T=I^T=I
$$

## 1.5 基本矩阵和求逆的方法 Elementary Matrices and a Method for Finding inverse

**The inverse of elementary row operations** (also row operation)

1. Multiply the same row by $1/c$.
2. Interchange the same two rows.
3. If $B$ resulted by adding $c$ times row $r_i$ of $A$ to row $r_j$ , then add $−c$ times $r_j$ to $r_i$.

### DEFINITION 1

Matrices $A$ and $B$ are said to be **row equivalent** if either (hence each) can be obtained from the other by a sequence of elementary row operations.

### DEFINITION 2

A matrix $E$ is called an **elementary matrix** if it can be obtained from an identity matrix by performing a *single* elementary row operation.
$$
\begin{bmatrix}
1&0\\
0&-3
\end{bmatrix}\text{Multiply the second row of $I_2$ by $-3$.}
$$

$$
\begin{bmatrix}
1&0&0&0\\
0&0&0&1\\
0&0&1&0\\
0&1&0&0
\end{bmatrix}\text{Interchange the second and fourth rows of $I_4$.}
$$

$$
\begin{bmatrix}
1&0&3\\
0&1&0\\
0&0&1\\
\end{bmatrix}\text{Add 3 times the third row of $I_3$ to the first row.}
$$

### THEOREM 1.5.1 Row Operations by Matrix Multiplication

If the elementary matrix $E$ results from performing a certain row operation on $I_m$ and if $A$ is an $m \times n$ matrix, then the product $EA$ is the matrix that results when this same row operation is performed on $A$.

### THEOREM 1.5.2 

Every elementary matrix is invertible, and the inverse is also an elementary matrix.

### THEOREM 1.5.3

[Equivalent Statements](Equivalent Statements.md#Equivalent Statements)

### A Method for Inverting Matrices

Adjoin the identity matrix to the right side of $A$:
$$
[A\ |\ I]
$$
Apply row operations to the matrix above until the left side is reduced to $I$
$$
E_k\dots E_2E_1[A\ |\ I]=A^{-1}[A\ |\ I]=[I\ |\ A^{-1}]
$$

## 1.6 More on Linear Systems and Invertible Matrices

### THEOREM 1.6.1

A system of linear equations has zero, one, or infinitely many solutions.

#### Proof

0 or 1 solution is obvious. And if $A\mathbf x=\mathbf b$ has more than 1 solution, then let $\mathbf x_1, \mathbf x_2$ be the distinct solution. $\mathbf x_0=\mathbf x_1-\mathbf x_2\ne\mathbf 0$, so
$$
A\mathbf x_0=A(\mathbf x_1-\mathbf x_2)=A\mathbf x_1-A\mathbf x_2=\mathbf b-\mathbf b=\mathbf 0
$$
let $k$ be any scalar:
$$
A(\mathbf x_1+k\mathbf x_0)=A\mathbf x_1+A(k\mathbf x_0)=\mathbf b+k\mathbf 0=\mathbf b
$$
So there are infinite solutions.

### THEOREM 1.6.2

If $A$ is an invertible $n \times n$ matrix, then for each $n \times 1$ matrix $\mathbf b$, the system of equations $A\mathbf x=\mathbf b$ has exactly one solution, namely, $\mathbf x=A^{-1}\mathbf b$.

#### Proof

`easy`

### THEOREM 1.6.3

Let $A$ be a square matrix.

(a) If $B$ is a square matrix satisfying $BA = I$, then $B= A^{−1}$. 

(b) If $B$ is a square matrix satisfying $AB = I$, then $B= A^{−1}$.

#### Proof

(a) Let $\mathbf x_0$ be the solution to $A\mathbf x=\mathbf 0$, then $\mathbf x_0=I\mathbf x_0=BA\mathbf x_0=B\mathbf 0=\mathbf 0$ is the only solution. Then $A$ is invertible.

(b) By definition [Inverse of a Matrix](#Inverse of a Matrix). Then $B= A^{−1}$.

### THEOREM 1.6.5

Let $A$ and $B$ be square matrices of the same size. If $AB$ is invertible, then $A$ and $B$ must also be invertible.

#### Proof

Let $\mathbf x_0$ be the solution of $B\mathbf x=\mathbf 0$, then
$$
AB\mathbf x_0=A\mathbf 0=\mathbf 0
$$
Since $AB$ is invertible, then $\mathbf x_0=\mathbf 0$, so $B$ is invertible, then
$$
A=(AB)B^{-1}
$$
is the product of invertible matrices, also invertible.

## 1.7 对角，三角和对称矩阵 Diagonal,Triangular, and Symmetric Matrices

### Diagonal Matrices

A square matrix in which all the entries *off the main diagonal* are zero is called a *diagonal matrix*.
$$
D=\begin{bmatrix}
d_1&0&\cdots&0\\
0&d_2&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&d_n
\end{bmatrix}\\
\exists d_i\ne0
$$
A diagonal matrix is invertible if and only if all of its diagonal entries are nonzero
$$
D^{-1}=\begin{bmatrix}
1/d_1&0&\cdots&0\\
0&1/d_2&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&1/d_n
\end{bmatrix}\\
$$
Powers of diagonal matrices
$$
D^k=\begin{bmatrix}
d_1^k&0&\cdots&0\\
0&d_2^k&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&d_n^k
\end{bmatrix}\\
$$

### Triangular Matrices

A *square* matrix in which all the entries *above* the main diagonal are zero is called *lower triangular*, and a square matrix in which all the entries *below* the main diagonal are zero is called *upper triangular*. A matrix that is either upper triangular or lower triangular is called *triangular*.
$$
L=\begin{bmatrix}
a_{11}&a_{12}&\cdots&a_{1n}\\
0&a_{22}&\cdots&a_{2n}\\
\vdots&\vdots&\ddots&\vdots\\
0&0&\cdots&a_{nn}
\end{bmatrix}\quad 
U=\begin{bmatrix}
a_{11}&0&\cdots&0\\
a_{21}&a_{22}&\cdots&0\\
\vdots&\vdots&\ddots&\vdots\\
a_{n1}&a_{n2}&\cdots&a_{nn}
\end{bmatrix}\
$$

### THEOREM 1.7.1

(a) The transpose of a lower triangular matrix is upper triangular, and the transpose of an upper triangular matrix is lower triangular.

(b) The product of lower triangular matrices is lower triangular, and the product of upper triangular matrices is upper triangular.

(c) A triangular matrix is invertible if and only if its diagonal entries are all nonzero.

(d) The inverse of an invertible lower triangular matrix is lower triangular, and the inverse of an invertible upper triangular matrix is upper triangular

#### Proof

(a) `easy`

(b) Let $A$ and $B$ be the lower triangular matrices, then $a_{ij},b_{ij}=0$ for $i<j$
$$
c_{ij}=\sum_{k=1}^{n}a_{ik}b_{kj}=\sum_{k=1}^{i}a_{ik}b_{kj}
$$
So if $i<j$, then $k\le i<j$, then $b_{kj}=0$, then $c_{kj}=0$.

(c) (d) to be down later.

### Symmetric Matrices

A square matrix $A$ is said to be *symmetric* if $A=A^T$ .

### THEOREM 1.7.2

If $A$ and $B$ are symmetric matrices with the same size, and if $k\in\mathbb C$, then:

(a) $A^T$ is symmetric.

(b) $A + B$ and $A − B$ are symmetric.

(c) $kA$ is symmetric.

`easy to proof`.

### THEOREM 1.7.3

The product of two symmetric matrices is symmetric if and only if the matrices commute.
$$
(AB)^T=B^TA^T=BA=AB
$$

### THEOREM 1.7.4

If $A$ is an *invertible symmetric* matrix, then $A^{-1}$ is symmetric.
$$
I=(AA^{-1})^{T}=(A^{-1})^{T}A^{T}=(A^{-1})^{T}A\\
\Longrightarrow (A^{-1})^{T}=A^{-1}
$$

### Production of $AA^T$ and $A^TA$

$A$ can be any matrix.
$$
(AA^T)^T=(A^T)^TA^T=AA^T\\
(A^TA)^T=A^T(A^T)^T=A^TA
$$
So production of $AA^T$ and $A^TA$ are *sysmetric*.

### THEOREM 1.7.5

If $A$ is an invertible matrix, then $AA^T$ and $A^TA$ are also invertible.

#### Proof

Since $A$ is invertible, so is $A^{T}$. Then the production are invertible.

## 1.8 矩阵变换 Matrix Transformations

Define $\mathbb R^n$ be the $n$ 维空间 $n$ dimension space.

Define 标准基 *standard basis vector*:
$$
\mathbf e_1=\begin{bmatrix}1\\0\\\vdots\\0\end{bmatrix}
\mathbf e_2=\begin{bmatrix}0\\1\\\vdots\\0\end{bmatrix}
\cdots
\mathbf e_n=\begin{bmatrix}0\\0\\\vdots\\1\end{bmatrix}
$$
Then $\forall \mathbf x\in\mathbb R^n$ can be expressed as 
$$
\mathbf x=x_1\mathbf e_1+\cdots+x_n\mathbf e_n
$$

### 函数和变换 Functions and Transformations

*function* is a rule that associates with each element of a set $A$ one and only one element in a set $B$. If $f$ associates the element $b$ with the element $a$, then we write
$$
b=f(a)
$$
$b$ is the 像 *image* of $a$ under $f$. 

$A$ is 定义域 *domain*, $B$ is 上域 *codomain*, $\{b|b=f(a)\}$ is 值域 *range* which is a subset of $B$.

If $f$ is a function with domain $R^n$ and codomain $R^m$, then we say that $f$ is a *transformation* from $R^n$ to $R^m$ or that $f$ *maps* from $R^n$ to $R^m$, which we denote
$$
f: R^n\to R^m
$$
In the special case where $m=n$, a transformation is sometimes called an *operator* on $R^n$.

Usually we use $T$ to denote the transform.

*Matrix multipltication is a kind of transform, we call it matrix transform.*

### THEOREM 1.8.1

For every matrix $A$ the matrix transformation $T_A: R^n\to R^m$ has the following properties for all vectors $\mathbf u$ and $\mathbf v$ and for every scalar $k$ : 

(a) $T_A(\mathbf 0)=\mathbf 0$ 

(b) $T_A(k\mathbf u)=kT_A(\mathbf u)$[Homogeneity property]

(c) $T_A(\mathbf u\pm\mathbf v)=T_A(\mathbf u)\pm T_A(\mathbf v)$[Additivity property]

Then it make sence that 
$$
T_A(\sum_{i=1}^{r}k_i\mathbf u_i)=\sum_{i=1}^{r}k_iT_A(\mathbf u)
$$
