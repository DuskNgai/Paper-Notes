# Chapter 3 Euclidean Vector Spaces

## 3.1 二维三维 n 维空间的向量 Vectors in 2-Space, 3-Space, and n-Space

Vector is a directed arrow.

$\mathbf v + \mathbf w$ is the translation of $\mathbf v$ by $\mathbf w$ or the translation of $\mathbf w$ by $\mathbf v$.
$$
\mathbf v+\mathbf w=\mathbf w+\mathbf v
$$

$$
\mathbf w − \mathbf v = \mathbf w + (−\mathbf v)
$$

### THEOREM 3.1.1

If $\mathbf u$, $\mathbf v$, and $\mathbf w$ are vectors in $\mathbb R^n$, and if $k$ and $m$ are scalars, then:

1. $\mathbf u + \mathbf v = \mathbf v + \mathbf u$
2. $(\mathbf u + \mathbf v) + \mathbf w = \mathbf u + (\mathbf v + \mathbf w)$
3. $\mathbf u + \mathbf 0 = \mathbf 0 + \mathbf u = \mathbf u$
4. $\mathbf u + (−\mathbf u) = \mathbf 0$
5. $k(\mathbf u + \mathbf v) = k\mathbf u + k\mathbf v$
6. $(k + m)\mathbf u = k\mathbf u + m\mathbf u$
7. $k(m\mathbf u) = (km)\mathbf u$
8. $1\mathbf u = \mathbf u$

### THEOREM 3.1.2

If $\mathbf v$ is a vector in $\mathbb R^n$ and $k$ is a scalar, then:

1. $0\mathbf v = \mathbf 0$
2. $k\mathbf 0 = \mathbf 0$
3. $(−1)\mathbf v = −\mathbf v$

### DEFINITION 4

If $\mathbf w$ is a vector in $\mathbb R^n$, then $\mathbf w$ is said to be a linear combination of the vectors $\mathbf v1, \mathbf v2,\dots, \mathbf vr$ in $\mathbb R^n$ if it can be expressed in the form $\mathbf w = k_1\mathbf v_1 + k_2\mathbf v_2 +\dots+k_r \mathbf v_r$ where $k_1, k_2,\dots, k_r$ are scalars. These scalars are called the coefficients of the linear combination.

## 3.2 范数，点乘和距离 Norm, Dot Product, and Distance in $\mathbb R^n$

### DEFINITION 1

If $\mathbf v = (\mathbf v_1, \mathbf v_2,\dots, \mathbf v_n)$ is a vector in $\mathbb R^n$, then the norm of $\mathbf v$ is denoted by $\|\mathbf v\|$, and is defined by the formula 
$$
\|\mathbf v\|=\sqrt{\sum_{i=1}^nv_i^2}
$$

### THEOREM 3.2.1

If $\mathbf v$ is a vector in $\mathbb R^n$, and if $k$ is any scalar, then:

1. $\|\mathbf v\|\ge0$
2. $\|\mathbf v\|=0$ if and only if $\mathbf v=\mathbf 0$
3. $\|k\mathbf v\|=|k|\|\mathbf v\|$

### 单位向量 Unit Vectors

A vector of norm 1 is called a unit vector.
$$
\mathbf u=\frac{1}{\|\mathbf v\|}\mathbf v
$$

### 标准单位向量 The Standard Unit Vectors

The unit vectors in the positive directions of the coordinate axes are called the standard unit vectors. The standard unit vectors in $\mathbb R^n$ are
$$
\mathbf e_1=(1, 0,\dots,0),\mathbf e_2=(0, 1,\dots,0),\dots,\mathbf e_n=(0, 0,\dots,1),
$$

### DEFINITION 2

If $\mathbf u = (u_1, u_2,\dots, u_n)$ and $\mathbf v = (v_1, v_2,\dots, v_n)$ are points in $\mathbb R^n$, then we denote the distance between $\mathbf u$ and $\mathbf v$ by $d(\mathbf u, \mathbf v)$ and define it to be
$d(\mathbf u, \mathbf v) = \|\mathbf u − \mathbf v\| =\sqrt{(u_1 − v_1)^2 + (u_2 − v_2)^2 +\dots+(u_n − v_n)^2}$

### Dot Product

If $\mathbf u$ and $\mathbf v$ are nonzero vectors in $\mathbb R^2$ or $\mathbb R^3$, and if $\theta$ is the angle between $\mathbf u$ and $\mathbf v$, then the dot product (also called the Euclidean inner product) of $\mathbf u$ and $\mathbf v$ is denoted by $\mathbf u\cdot \mathbf v$ and is defined as
$$
\mathbf u\cdot\mathbf v=\|\mathbf u\|\|\mathbf v\|\cos\theta=\sum_{i=1}^n u_iv_i
$$
If $\mathbf u = 0$ or $\mathbf v = 0$, then we define $\mathbf u\cdot \mathbf v$ to be $0$.

$$
\cos\theta=\frac{\mathbf u\cdot\mathbf v}{\|\mathbf u\|\|\mathbf v\|}
$$

### THEOREM 3.2.2

If $\mathbf u$, $\mathbf v$, and $\mathbf w$ are vectors in $\mathbb R^n$, and if $k$ is a scalar, then:

1. $\mathbf u\cdot\mathbf v = \mathbf v\cdot\mathbf u$ [Symmetry property]
2. $\mathbf u \cdot (\mathbf v + \mathbf w) = \mathbf u \cdot \mathbf v + \mathbf u \cdot \mathbf w$ [Distributive property]
3. $k(\mathbf u \cdot \mathbf v) = (k\mathbf u) \cdot \mathbf v$ [Homogeneity property]
4. $\mathbf v \cdot \mathbf v ≥ 0$ and $\mathbf v \cdot \mathbf v = 0$ if and only if $\mathbf v = \mathbf 0$ [Positivity property]

### THEOREM 3.2.3

If $\mathbf u$, $\mathbf v$, and $\mathbf w$ are vectors in $\mathbb R^n$, and if $k$ is a scalar, then:

1. $\mathbf 0 \cdot \mathbf v = \mathbf v \cdot \mathbf 0 = 0$
2. $(\mathbf u + \mathbf v)  \cdot \mathbf w = \mathbf u  \cdot \mathbf w + \mathbf v  \cdot \mathbf w$
3. $\mathbf u \cdot (\mathbf v − \mathbf w) = \mathbf u \cdot \mathbf v − \mathbf u \cdot \mathbf w$
4. $(\mathbf u − \mathbf v) \cdot \mathbf w = \mathbf u \cdot \mathbf w − \mathbf v \cdot \mathbf w$
5. $k(\mathbf u \cdot \mathbf v) = \mathbf u \cdot (k\mathbf v)$

### Cauchy–Schwarz Inequality and Angles in $\mathbb R^n$

Since
$$
-1\le\cos\theta=\frac{\mathbf u\cdot\mathbf v}{\|\mathbf u\|\|\mathbf v\|}\le1
$$

### THEOREM 3.2.4 Cauchy–Schwarz Inequality

If $\mathbf u = (u_1, u_2,\dots, u_n)$ and $\mathbf v = (v_1, v_2,\dots, v_n)$ are points in $\mathbb R^n$, then
$$
|\mathbf u\cdot\mathbf v|\le\|\mathbf u\|\|\mathbf v\|\\
(\sum_{i=1}^nu_iv_i)^2\le(\sum_{i=1}^nu_i^2)(\sum_{i=1}^nv_i^2)
$$

### THEOREM 3.2.5

If $\mathbf u$, $\mathbf v$, and $\mathbf w$ are vectors in $\mathbb R^n$, then:

1. $\|\mathbf u+\mathbf v\|\le\|\mathbf u\|+\|\mathbf v\|$ [Triangle inequality for vectors]
2. $d(\mathbf u, \mathbf v) ≤ d(\mathbf u, \mathbf w) + d(\mathbf w, \mathbf v)$ [Triangle inequality for distances]

### THEOREM 3.2.6 Parallelogram Equation for Vectors

If $\mathbf u$and $\mathbf v$ are vectors in $\mathbb R^n$, then:
$$
\|\mathbf u+\mathbf v\|^2+\|\mathbf u-\mathbf v\|^2=2(\|\mathbf u\|^2+\|\mathbf v\|^2)
$$

### THEOREM 3.2.7

If $\mathbf u$and $\mathbf v$ are vectors in $\mathbb R^n$ with the Euclidean inner product, then
$$
\mathbf u\cdot\mathbf v=\frac{1}{4}(\|\mathbf u+\mathbf v\|^2-\|\mathbf u-\mathbf v\|^2)
$$
If $A$ is an $n \times n$ matrix and $\mathbf u$ and $\mathbf v$ are $n \times 1$ matrices, then:
$$
A\mathbf u\cdot\mathbf v=\mathbf v^T(A\mathbf u)=(\mathbf v^TA)\mathbf u=(A^T\mathbf v)^T\mathbf u=\mathbf u\cdot A^T\mathbf v\\
\mathbf u\cdot A\mathbf v=(A\mathbf v)^T\mathbf u=(\mathbf v^TA^T)\mathbf u=\mathbf v^T(A^T\mathbf u)=A^T\mathbf u\cdot\mathbf v
$$
If the row vectors of $A$ are $\mathbf r_1, \mathbf r_2,\dots, \mathbf r_m$ and the column vectors of $B$ are $\mathbf c_1,\mathbf c_2,\dots, \mathbf c_n$, then the matrix product $AB$ can be expressed as
$$
AB=\begin{bmatrix}
\mathbf r_1\cdot\mathbf c_1&\mathbf r_1\cdot\mathbf c_2&\cdots&\mathbf r_1\cdot\mathbf c_n\\
\mathbf r_2\cdot\mathbf c_1&\mathbf r_2\cdot\mathbf c_2&\cdots&\mathbf r_2\cdot\mathbf c_n\\
\vdots&\vdots&&\vdots\\
\mathbf r_m\cdot\mathbf c_1&\mathbf r_m\cdot\mathbf c_2&\cdots&\mathbf r_m\cdot\mathbf c_n
\end{bmatrix}
$$

## 3.3 正交性 Orthogonality

### DEFINITION 1

Two nonzero vectors $u$ and $v$ in $\mathbb R^n$ are said to be *orthogonal* (or perpendicular) if $\mathbf u \cdot \mathbf v = 0$. We will also agree that the zero vector in $\mathbb R^n$ is orthogonal to every vector in $\mathbb R^n$.

### 直线和平面的点法向式 Lines and Planes Determined by Points and Normals

Consider a line in $\mathbb R^2$ that passes $(x_0, y_0)$. An arbitary point on the line $(x,y)$, 
$$
a(x-x_0)+b(y-y_0)=\begin{bmatrix}a\\b\end{bmatrix}\cdot\begin{bmatrix}x-x_0\\y-y_0\end{bmatrix}=0
$$
where $(a, b)$ is the normal of the line.

The same for plane in $\mathbb R^3$
$$
\begin{bmatrix}a\\b\\c\end{bmatrix}\cdot\begin{bmatrix}x-x_0\\y-y_0\\z-z_0\end{bmatrix}=0
$$
Vector form of the line.

### 正投影 Orthogonal Projections

### Projection Theorem

If $\mathbf u$ and $\mathbf a$ are vectors in $\mathbb R^n$, and if $\mathbf a \ne\mathbf 0$, then $\mathbf u$ can be expressed in exactly one way in the form $\mathbf u = \mathbf w_1 + \mathbf w_2$, where $\mathbf w_1$ is a scalar multiple of $\mathbf a$ and $\mathbf w_2$ is orthogonal to $\mathbf a$.

#### Proof:

Since
$$
\mathbf w_1=k\mathbf a
$$
Then
$$
\mathbf u=k\mathbf a+\mathbf w_2\\
\mathbf u\cdot\mathbf a=k\mathbf a\cdot\mathbf a+\mathbf w_2\cdot\mathbf a\\
\mathbf u\cdot\mathbf a=k\|\mathbf a\|^2\\
k=\frac{\mathbf u\cdot\mathbf a}{\|\mathbf a\|^2}\\
\mathbf w_2=\mathbf u-\frac{\mathbf u\cdot\mathbf a}{\|\mathbf a\|^2}\mathbf a
$$
We call the $\mathbf w_1$ the orthogonal projection of $\mathbf u$ on $\mathbf a$, we denote it as $\text{proj}_{\mathbf a}\mathbf u$, the $\mathbf w_2$ the vector component of $\mathbf u$ orthogonal to $\mathbf a$.
$$
\text{proj}_{\mathbf a}\mathbf u=\frac{\mathbf u\cdot\mathbf a}{\|\mathbf a\|^2}\mathbf a\\
\mathbf u-\text{proj}_{\mathbf a}\mathbf u=\mathbf u-\frac{\mathbf u\cdot\mathbf a}{\|\mathbf a\|^2}\mathbf a
$$


### THEOREM 3.3.3 Theorem of Pythagoras in $\mathbb R^n$

If $\mathbf u$ and $\mathbf v$ are orthogonal vectors in $\mathbb R^n$ with the Euclidean inner product, then
$$
\|\mathbf u+\mathbf v\|^2=\|\mathbf u\|^2+\|\mathbf v\|^2
$$

### THEOREM 3.3.4

$$
D=\frac{|ax_0+by_0+cz_0+d|}{\sqrt{a^2+b^2+c^2}}=\frac{\mathbf n\cdot\mathbf x}{\|\mathbf n\|}
$$

## 3.4 线性方程组的几何意义 The Geometry of Linear Systems

### 直线的参数方程 (点向式) Vector and Parametric Equations of Lines in $\mathbb R^2$ and $\mathbb R^3$

Let L be the line in $\mathbb R^2$ or $\mathbb R^3$ that contains the point $\mathbf x_0$ and is parallel to the nonzero vector $\mathbf v$. Then the equation of the line through $\mathbf x_0$ that is parallel to $\mathbf v$ is $\mathbf x = \mathbf x0 + t\mathbf v$. If $\mathbf x_0 = \mathbf 0$, then the line passes through the origin and the equation has the form $\mathbf x = t\mathbf v$.

Let W be the plane in $\mathbf R^3$ that contains the point $\mathbf x_0$ and is parallel to the noncollinear vectors $\mathbf v_1$ and $\mathbf v_2$. Then an equation of the plane through $\mathbf x_0$ that is parallel to $\mathbf v_1$ and $\mathbf v_2$ is given by $\mathbf x = \mathbf x_0 + t_1\mathbf v_1 + t_2\mathbf v_2$. If $\mathbf x_0 = \mathbf 0$, then the plane passes through the origin and the equation has the form $\mathbf x = t_1\mathbf v_1 + t_2\mathbf v_2$.

### DEFINITION 1

If $\mathbf x0$ and $\mathbf v$ are vectors in $\mathbb R^n$, and if $\mathbf v$ is nonzero, then the equation $\mathbf x = \mathbf x0 + t\mathbf v$ defines the line through x0 that is parallel to $\mathbf v$. In the special case where $\mathbf x_0 = \mathbf 0$, the line is said to pass through the origin.

### DEFINITION 2

If $\mathbf x_0$, $\mathbf v_1$, and $\mathbf v_2$ are vectors in $\mathbb R^n$, and if $\mathbf v_1$ and $\mathbf v_2$ are not collinear, then the equation $\mathbf x = \mathbf x_0 + t_1\mathbf v_1 + t_2\mathbf v_2$ defines the plane through x0 that is parallel to v1 and v2. In the special case where $\mathbf x_0 = \mathbf 0$, the plane is said to pass through the origin.

### 两点式 Lines Through Two Points in $\mathbb R^n$

$$
\mathbf x = \mathbf x_0 + t(\mathbf x_1 − \mathbf x_0)\Longrightarrow\mathbf x = (1 − t)\mathbf x_0 + t\mathbf x_1
$$

### THEOREM 3.4.3

If $A$ is an $m \times n$ matrix, then the solution set of the homogeneous linear system $A\mathbf x = \mathbf 0$ consists of all vectors in $\mathbb R^n$ that are orthogonal to every row vector of $A$.

### THEOREM 3.4.4

The general solution of a consistent linear system $A\mathbf x = \mathbf b$ can be obtained by adding any specific solution of $A\mathbf x = \mathbf b$ to the general solution of $A\mathbf x = \mathbf 0$.

## 3.5 叉乘 Cross Product

### DEFINITION 1

If $\mathbf u = (u_1, u_2, u_3)$ and $\mathbf v = (v_1, v_2, v_3)$ are vectors in $\mathbb R^3$, then the cross product $\mathbf u \times \mathbf v$ is the vector defined by
$$
\mathbf u \times \mathbf v=(u_2v_3 − u_3v_2, u_3v_1 − u_1v_3, u_1v_2 − u_2v_1)
$$
or
$$
\mathbf u \times \mathbf v=(\begin{vmatrix}u_2&u_3\\v_2&v_3\end{vmatrix},-\begin{vmatrix}u_1&u_3\\v_1&v_3\end{vmatrix},\begin{vmatrix}u_1&u_2\\v_1&v_2\end{vmatrix})
$$
or
$$
\mathbf u \times \mathbf v=\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\v_1&v_2&v_3\end{vmatrix}
$$

### THEOREM 3.5.1 Relationships Involving Cross Product and Dot Product

If $\mathbf u$, $\mathbf v$, and w are vectors in $\mathbb R^3$, then

1. $\mathbf u \cdot (\mathbf u \times \mathbf v) = \mathbf 0$ [$\mathbf u \times \mathbf v$ is orthogonal to $\mathbf u$]
2. $\mathbf v \cdot (\mathbf u \times \mathbf v) = \mathbf 0$ [$\mathbf u \times \mathbf v$ is orthogonal to $\mathbf v$]
3. $\|\mathbf u \times \mathbf v\|^2 = \|\mathbf u\|^2\|\mathbf v\|^2 − (\mathbf u \cdot \mathbf v)^2$ [ Lagrange’s identity ]
4. $\mathbf u \times (\mathbf v \times \mathbf w) = (\mathbf u \cdot \mathbf w)\mathbf v − (\mathbf u \cdot \mathbf v)\mathbf w$ [ vector triple product ]
5. $(\mathbf u \times \mathbf v) \times \mathbf w = (\mathbf u \cdot \mathbf w)\mathbf v − (\mathbf v \cdot \mathbf w)\mathbf u$ [ vector triple product ]

#### Proof

1. $$
   \mathbf u \cdot (\mathbf u \times \mathbf v)=\begin{vmatrix}u_1&u_2&u_3\\ u_1&u_2&u_3\\v_1&v_2&v_3\end{vmatrix}=0
   $$

2. $$
   \mathbf v \cdot (\mathbf u \times \mathbf v)=\begin{vmatrix}v_1&v_2&v_3\\ u_1&u_2&u_3\\v_1&v_2&v_3\end{vmatrix}=0
   $$

3. 
   $$
   \begin{align*}
   \text{LHS}&=(u_2v_3 − u_3v_2)^2+(u_3v_1 − u_1v_3)^2+(u_1v_2 − u_2v_1)^2\\
   &=u^2_1v^2_2+u^2_2v^2_1+u^2_1v^2_3+u^2_3v^2_1+u^2_2v^2_3+u^2_3v^2_2\\
   &\quad-2u_1u_2v_1v_2-2u_1u_3v_1v_3-2u_2u_3v_2v_3\\
   &=u^2_1v^2_2+u^2_2v^2_1+u^2_1v^2_3+u^2_3v^2_1+u^2_2v^2_3+u^2_3v^2_2+u^2_1v^2_1+u^2_2v^2_2+u^2_3v^2_2\\
   &\quad-u^2_1v^2_1-u^2_2v^2_2-u^2_3v^2_3-2u_1u_2v_1v_2-2u_1u_3v_1v_3-2u_2u_3v_2v_3\\
   \text{RHS}&=(u_1+u_2+u_3)^2(v_1+v_2+v_3)^2-(u_1v_1+u_2v_2+u_3v_3)^2\\
   \end{align*}
   $$
   
4. $$
   \begin{align*}
   &\quad\mathbf u \times (\mathbf v \times \mathbf w)\\
   &=\begin{vmatrix}i&j&k\\
   u_1&u_2&u_3\\
   v_2w_3 − v_3w_2&v_3w_1 − v_1w_3&v_1w_2 − v_2w_1
   \end{vmatrix}\\
   &=\begin{vmatrix}i&j&k\\
   u_1&u_2&u_3\\
   v_2w_3&v_3w_1&v_1w_2
   \end{vmatrix}-\begin{vmatrix}i&j&k\\
   u_1&u_2&u_3\\
   v_3w_2&v_1w_3&v_2w_1
   \end{vmatrix}\\
   &=(u_2v_1w_2-u_3v_3w_1,u_3v_2w_3-u_1v_1w_2,u_1v_3w_1-u_2v_2w_3)\\
   &\quad-(u_2v_2w_1-u_3v_1w_3,u_3v_3w_2-u_1v_2w_1,u_1v_1w_3-u_2v_3w_2)\\
   &=(u_2v_1w_2+u_3v_1w_3,u_3v_2w_3+u_1v_2w_1,u_1v_3w_1+u_2v_3w_2)\\
   &\quad-(u_2v_2w_1+u_3v_3w_1,u_3v_3w_2+u_1v_1w_2,u_1v_1w_3+u_2v_2w_3)\\
   &=(u_2w_2+u_3w_3,u_3w_3+u_1w_1,u_1w_1+u_2w_2)\cdot(v_1,v_2,v_3)\\
   &\quad-(u_2v_2+u_3v_3,u_3v_3+u_1v_1,u_1v_1+u_2v_2)\cdot(w_1,w_2,w_3)\\
   &=\text{adding terms and subtract terms}\\
   &=(\mathbf u \cdot \mathbf w)\mathbf v − (\mathbf u \cdot \mathbf v)\mathbf w
   \end{align*}
   $$

5. Similar as 4.

### THEOREM 3.5.2 Properties of Cross Product

If $\mathbf u$, $\mathbf v$, and $\mathbf w$ are any vectors in $\mathbb 3$ and $k$ is any scalar, then:

1. $\mathbf u \times \mathbf v = −(\mathbf v \times \mathbf u)$
2. $\mathbf u \times (\mathbf v + \mathbf w) = (\mathbf u \times \mathbf v) + (\mathbf u \times \mathbf w)$
3. $(\mathbf u + \mathbf v) \times \mathbf w = (\mathbf u \times \mathbf w) + (\mathbf v \times \mathbf w)$
4. $k(\mathbf u \times \mathbf v) = (k\mathbf u) \times \mathbf v = \mathbf u \times (k\mathbf v)$
5. $\mathbf u \times \mathbf 0 = \mathbf 0 \times \mathbf u = \mathbf 0$
6. $\mathbf u \times \mathbf u = \mathbf 0$

#### Proof

1. $$
   \mathbf u \times \mathbf v =\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\v_1&v_2&v_3\end{vmatrix}=-\begin{vmatrix}\hat i&\hat j&\hat k\\v_1&v_2&v_3\\ u_1&u_2&u_3\end{vmatrix}= −(\mathbf v \times \mathbf u)
   $$

2. $$
   \mathbf u \times (\mathbf v + \mathbf w) =\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\v_1+w_1&v_2+w_2&v_3+w_3\end{vmatrix}=\\\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\v_1&v_2&v_3\end{vmatrix}+\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\w_1&w_2&w_3\end{vmatrix}= (\mathbf u \times \mathbf v) + (\mathbf u \times \mathbf w)
   $$

3. $$
   (\mathbf u + \mathbf v) \times \mathbf w =\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1+v_1&u_2+v_2&u_3+v_3\\w_1&w_2&w_3\end{vmatrix}
   =\\\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\w_1&w_2&w_3\end{vmatrix}+\begin{vmatrix}\hat i&\hat j&\hat k\\ v_1&v_2&v_3\\w_1&w_2&w_3\end{vmatrix}=(\mathbf u \times \mathbf w) + (\mathbf v \times \mathbf w)
   $$

4. $$
   k(\mathbf u \times \mathbf v) = k\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\v_1&v_2&v_3\end{vmatrix}=\\
   (k\mathbf u) \times \mathbf v = \begin{vmatrix}\hat i&\hat j&\hat k\\ ku_1&ku_2&ku_3\\v_1&v_2&v_3\end{vmatrix}=\\
   \mathbf u \times (k\mathbf v)=\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\kv_1&kv_2&kv_3\end{vmatrix}
   $$

5. $$
   \mathbf u \times \mathbf 0 =\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\0&0&0\end{vmatrix}= \mathbf 0 \times \mathbf u = \begin{vmatrix}\hat i&\hat j&\hat k\\ 0&0&0\\v_1&v_2&v_3\end{vmatrix}=\mathbf 0
   $$

6. $$
   \mathbf u \times \mathbf u =\begin{vmatrix}\hat i&\hat j&\hat k\\ u_1&u_2&u_3\\u_1&u_2&u_3\end{vmatrix}= \mathbf 0
   $$

### 叉乘的几何解释 Geometric Interpretation of Cross Product

$$
\begin{align*}
\|\mathbf u \times \mathbf v\|^2&=\|\mathbf u\|^2\|\mathbf v\|^2 − (\mathbf u \cdot \mathbf v)^2\\
&=\|\mathbf u\|^2\|\mathbf v\|^2 − \|\mathbf u\|^2\|\mathbf v\|^2\cos^2\theta\\
&=\|\mathbf u\|^2\|\mathbf v\|^2\sin^2\theta
\end{align*}\\
\|\mathbf u \times \mathbf v\|=\|\mathbf u\|\|\mathbf v\|\sin\theta
$$

### THEOREM 3.5.3 Area of a Parallelogram

If $\mathbf u$ and $\mathbf v$ are vectors in $\mathbb R^3$, then $\mathbf u \times \mathbf v$ is equal to the area of the parallelogram determined by $\mathbf u$ and $\mathbf v$.
