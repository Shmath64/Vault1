$cof(A)_{ij} = (-1)^{i+j}minor(A)_{ij}$

To find the determinant of a matrix, use "Laplace Expansion" / "Cofactor Expansion".
We can use elementary row or column operations to simplify the matrix, HOWEVER, these can change the determinant!
- Row swapping changes the sign of the determinant
- Scaling a row by a constant scales the determinant by that constant (so to keep the determinant the same, multiple by one over that constant)
- Adding a multiple of one row to another does NOT change the determinant!

### 3.2
- The only invertible matrices are square matrices.

The cofactor matrix of a square matrix A, is where each entry is the the cofactor of the corresponding element in A.

#### Cramer's Rule
Solve AX=B when A is invertible
$X = A^{-1}B = \frac{1}{det(A)}adj(B)$

## 4: $\mathbb{R}^n$
##### 4.1 Vectors in $\mathbb{R}^n$ Algebra
- Just tip-to-tail addition (it's commutative), scaling 
##### 4.2 Vectors in $\mathbb{R}^n$ Algebra
$\vec{PQ} = \vec{Q}-\vec{P}$
- Properties of scalar multiplication:
	- Distributive
	- Associative $s(c\vec{u}) = (sc)\vec u$
- Vector addition is just adding the components (like matrix addition)
- Linear Combination (the existence of scalars to make a sum)
##### 4.3 Length of a Vector
- Just "extend" the Pythagorean theorem, take root of squared differences.

##### 4.4 The Dot Product
Returns a scalar, the sum of the products of each component. (AKA Scalar Product)
$||\vec{u}||^2 = \vec u \cdot \vec u$

##### 4.5 The Cross Product
##### 4.6 Parametric Lines
Parametric equations mean separate equations for each component:
e.g.
$x = 1 + 2t$
$y = 7 +t$ 
##### 4.7 Planes in $\mathbb{R}^3$
Normal vector of a plane iff $\vec n \cdot \vec v=0, \forall \vec v$ in the plane.
**Vector Equation:** $\vec n \cdot (\vec{0P} - \vec{0P_0})=0$
**Scalar Equation:** $ax+by+cy=d$
To get shortest distance from a point to a plane (point on the plane, $Q$) in $\mathbb{R}^3$, pick an arbitrary point $P_0$ on the plane, then $\vec{QP} = proj_{\vec n}\vec{P_0P}$. And ||QP|| is shortest distance

##### 4.8 Spanning and Linear Independence in $\mathbb{R}^n$
To check if $\vec w$ is in $span\{\vec u, \vec v\}$, means to find scalars a,b, $w = au + bv$
A set of vectors is linearly *dependent* iff there is a non trivial solution to the homogenous system. i.e. one vector can be written as a linear combination of the others


##### 4.9 Subspaces, Bases, and Dimension
**Subspace:**
- Exactly the span of finitely many vectors.
Subspaces are closed under scalar multiplication and scalar addition and are a subset of $\mathbb{R}^n$. Linear combinations of vectors within a subspace remain in a subspace. Need not be more than one vector. 
- **Subspaces MUST contain the zero vector**
**Bases:**
A set of vectors is a basis of a subspace iff:
1. span of the vectors = the subspace
2. the vectors are linearly independent 
"Every subspace of $\mathbb{R}^n$ has a basis consisting of $n$ or fewer vectors."

$proj_{\vec w}(\vec v) = (\frac{\vec{w} \cdot \vec v}{\vec w \cdot \vec w})\vec w$
