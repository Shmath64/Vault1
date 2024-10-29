Review Practice Midterm Question 2 Part b at some point.

Given a system of linear equations, we can express finding the solution set / unique solution of that equation as:
$C(parameters)^T = (constants)^T$
$(parameters)^T = C^{-1}(constants)^T$

A system of linear equations is:
**over-determined** if $m>n$ (more equations than variables)
**under-determined** if $m<n$ (more variables than equations)

A **basic solution** occurs in a homogenous system of linear equations. Let the free-variable(s) = 1

#### Properties of the Inverse
1. $I$ is invertible and $I^{-1} = I$
2. If A is invertible then so is $A^{-1}$
3. If A is invertible then so is $A^k$ and $(A^K)^{-1} = (A^{-1})^k$
5. If A and B are invertible matrices then AB is invertible and $(AB)^{-1} = B^{-1}A^{-1}$ 
		^Note the change of order when inverting a matrix product!
6. If $A_1, A_2, ..., A_k$ are invertible, then the product $A_1A_2...A_k$ is invertible and $(A_1A_2...A_k)^{-1} = A_k^{-1}A_{k-1}^{-1}...A_1^{-1}$ 
7. If A is invertible, then $(A^T)^{-1} = (A^{-1})^T$



$\frac{1}{det(a)} = det(a^{-1})$ <-- Check this!

**Minors** (denoted $minor(A)_{ij}$ ) are the determinants after eliminating the $i^{th}$ row and $j^{th}$ column.
"The $ij^{th}$ minor of $A$" is the minor of the $(n-1)*(n-1)$ matrix left over after "deleting" 

**Cofactor** (denoted $cof(A)_{ij}$ ) is defined to be:

	$cof(A)_{ij} = (-1)^{i+j}minor(A)_{ij}$
	
Like the minor, but negative if $i+j$ is odd

If the determinant is zero, it isn't invertible! ; A is invertible IFF $det(a)\neq0$

$det(A)=det(A^T)$
$det(AB)=det(A)det(B)$
$det(I)=1$
$det(A^{-1})=\frac{1}{det(A)}$

$A^{-1} = \frac{1}{det(A)}adj(A)$
