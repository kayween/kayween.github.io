---
layout: base
title: Matrix Quadratic Equations
---
# {{ page.title }}

Everyone knows how to solve a univariate quadratic equation \\( a x^2 + b x + c = 0 \\).
But it is less obvious how to solve its matrix variant
\\[
    \Xv^\top \Av \Xv + \tfrac12 \Bv^\top \Xv + \tfrac12 \Xv^\top \Bv + \Cv = 0,
\\]
where \\( \Xv, \Bv \in \Rb^{m \times n} \\), \\( \Cv \in \Rb^{n \times n} \\) is symmetric, and \\( \Av \in \Rb^{m \times m} \\) is symmetric positive definite.
All matrices are assumed to be real (we earned this privilege as computer scientists).
In what follows, we present the solution of the matrix quadratic equation developed by Crone (1981).

## A Special Case
The following lemma deals with a special case without the linear term (Crone, 1981, Proposition 3).

**Lemma 1.**
<i>
Let \\( \Sv \\) be a \\( n \times n \\) rank-\\( r \\) symmetric positive semi-definite matrix with a compact (only keeping the nonzero eigenvalues) spectral decomposition \\( \Sv = \Uv \Lambdav \Uv^\top \\).
Then, \\( \Xv \in \Rb^{m \times n} \\) with \\( m \geq r \\) satisfies the equation \\( \Xv^\top \Xv = \Sv \\) if and only if it is of the form \\( \Xv = \Vv \Lambdav^{\frac12} \Uv^\top = \Vv \Uv^\top \Sv^{\frac12} \\) for some column orthonormal matrix \\( \Vv \in \Rb^{m \times r} \\).
No solution exists when \\( m < r \\).
</i>

<i>Proof.</i>
The "if" direction is trivial.
Only the other direction needs a proof.
Let \\( \Xv \\) be a root and define \\( \Vv = \Xv \Uv \Lambdav^{-\frac12} \\).
It is easy to verify that \\( \Vv \\) indeed has orthonormal columns.
Then, it follows that
\\[
\Vv \Lambdav^{\frac12} \Uv^\top
=
\big(\Xv \Uv \Lambdav^{-\frac12}\big) \Lambdav^{\frac12} \Uv^\top
=
\Xv \Uv \Uv^\top
=
\Xv.
\\]
It's worth noting that the last equality does not assume \\( \Uv \Uv^\top \\) is identity---it is not when \\( \Sv \\) is rank deficient.
Instead, the last equality uses
\\(
\Xv \Uv_{\perp} = 0
\\)
since
\\(
\big(\Xv \Uv_{\perp}\big)^\top
\big(\Xv \Uv_{\perp}\big)
= 0
\\).
When \\( m < r\\), the equation's two sides do not have the same rank, and thus cannot be equal.
**Q.E.D.**

## The General Case

Now we are ready to tackle the general case of the matrix quadratic equation.
The idea is exactly the same as in the univariate case: completing the square.
It is obvious that the equation can be rewritten as
\\[
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)^\top
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)
=
\frac14 \big(\Bv^\top \Av\inv \Bv - 4 \Cv\big).
\\]
Use \\( \Gv \\) to denote \\( \Bv^\top \Av\inv \Bv - 4 \Cv \\) and call it the discriminant.
\\( \Gv \\) has to be symmetric positive semi-definite for the solution to exist, since the left hand side is symmetric positive semi-definite.
Assuming it is, then invoking Lemma 1 gives the formula for all roots:
\\[
    -\frac12 \Av\inv \Bv + \frac12 \Av^{-\frac12} \Vv \Uv^\top \Gv^{\frac12},
\\]
where \\( \Uv \\) is a column orthonormal matrix consisting of the eigenvectors of \\( \Gv \\), and \\( \Vv \\) is an arbitrary column orthonormal matrix (with an appropriate size).
One can verify that this formula reduces to the usual univariate quadratic formula when all matrices are \\( 1 \times 1 \\).
A crucial difference is that the matrix quadratic equation has infinitely many roots in most cases, particularly when the discriminant is positive definite.
However, in the univariate case, the column orthonormal matrix \\( \Vv \\) has only two options \\( 1 \\) and \\( -1 \\), which yields at most two roots.

## An Asymmetric Variant

Now consider the following "asymmetric" variant
\\[
    \Xv^\top \Av \Xv + \Bv^\top \Xv + \Cv = 0,
\\]
where \\( \Xv \\) is hit by \\( \Bv \\) from the left side only.
Again, we assume all matrices are real, \\( \Av \\) is symmetric positive definite, and \\( \Cv \\) is symmetric.
Note: It is not clear to me if this asymmetric variant is more general than the symmetric version.

If \\( \Xv \\) is a root then \\( \Bv^\top \Xv = \Xv^\top \Bv \\).
Because the right hand side of the following identity
\\[
    \Bv^\top \Xv = -\big(\Xv^\top \Av \Xv + \Cv\big),
\\]
is symmetric.
Then, splitting \\( \Bv^\top \Xv \\) into two parts \\( \tfrac12 \Bv^\top \Xv + \tfrac12 \Xv^\top \Bv \\) suggesting that we can apply the quadratic formula we just derived!
However, as tempting as it is, this argument has a bug.
Finding the logical bug is left as an exercise to the readers.

It turns out that the asymmetric matrix quadratic equation is much harder to solve.
Crone (1981) did not obtain a complete solution, particularly when the discriminant is singular,
but did give a solution when the discriminant is positive definite.
The result, however, is much messier and hence is omitted here.
It wasn't until recently that a general solution was discovered (Yuan et al., 2021).
I have not yet come across the asymmetric variant in applications and thus I am less motivated to read these results.
Interested readers may refer to their original papers.

### **Exercises**
1. Solve the matrix quadratic equation \\( \Xv^\top \Av \Xv + 2 \Xv + \Cv = 0 \\) with the following two conditions:
all matrices \\( \Av, \Cv, \Xv \\) are symmetric;
\\( \Av \\) is positive definite and satisfies \\( \Av\inv \succeq \Cv \\).
Note that we need to solve the equation with the symmetric constraint \\( \Xv = \Xv^\top \\).
How many roots are there?

### **References**
Crone, L. (1981). Second order adjoint matrix equations. Linear Algebra and Its Applications, 39, 61-71.

Yuan, Y., Liu, L., Zhang, H., & Liu, H. (2021). The solutions to the quadratic matrix equation X* AX+ B* X+ D= 0. Applied Mathematics and Computation, 410, 126463.
