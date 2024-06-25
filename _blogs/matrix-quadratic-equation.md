---
layout: base
title: Matrix Quadratic Equation
---
# {{ page.title }}

Everyone knows how to solve a univariate quadratic equation \\(a x^2 + b x + c = 0\\).
But it is less obvious how to solve \\( \Xv \\) in the matrix quadratic equation 
\\[
\begin{equation}
\label{eq:symmetric-equation}
    \Xv^\top \Av \Xv + \frac12 \Bv^\top \Xv + \frac12 \Xv^\top \Bv + \Cv = 0,
\end{equation}
\\]
where \\( \Xv, \Bv \in \Rb^{m \times n} \\), \\( \Cv \in \Rb^{n \times n} \\) is symmetric, and \\( \Av \in \Rb^{m \times m} \\) is symmetric positive definite.
All matrices are assumed to be real (we earned this privilege as computer scientists).
In what follows, we present the solution of this equation developed by Crone (1981).

## A Special Case
Before dealing with the general case \eqref{eq:symmetric-equation}, we must solve the special case
\\[
\begin{equation}
\label{eq:special-case}
    \Xv^\top \Xv = \Sv,
\end{equation}
\\]
where \\(\Sv\\) is symmetric positive semi-definite.
This special is simpler thanks to the absence of the linear term, and is covered by the following lemma (Crone, 1981, Proposition 3).

**Lemma 1.**
<i>
Let \\(\Xv \in \Rb^{m \times n}\\) and let \\(\Sv\\) be a \\(n \times n\\) symmetric positive semi-definite matrix with rank \\(r\\).
Then, the matrix equation \eqref{eq:special-case} has a root if and only if \\(m \geq r\\).
The roots are of the form
\\[
    \Xv = \Vv \Lambdav^{\frac12} \Uv^\top = \Vv \Uv^\top \Sv^{\frac12},
\\]
where \\(\Lambdav \in \Rb^{r \times r}\\) and \\(\Uv \in \Rb^{n \times r}\\) are matrices in the (compact) spectral decomposition \\(\Sv = \Uv \Lambdav \Uv^\top\\), and \\(\Vv \in \Rb^{m \times r}\\) is an arbitrary column orthonormal matrix.
</i>

<i>Proof.</i>
If \\(m < r\\), the equation's two sides do not have the same rank, and thus cannot be equal.
When \\(m > r\\), it is trivial to verify \\(\Vv \Lambdav^{\frac12} \Uv^\top\\) is indeed a root as long as \\(\Vv\\) is column orthonormal.
It remains to prove all roots can be written in this form.
Let \\( \Xv \\) be an arbitrary root and define \\(\Vv = \Xv \Uv \Lambdav^{-\frac12}\\).
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
**Q.E.D.**

## The General Case

Now we are ready to tackle the general form of the matrix quadratic equation \eqref{eq:symmetric-equation}.
The idea is exactly the same as in the univariate case: completing the square.
Observe that the equation \eqref{eq:symmetric-equation} can be rewritten as
\\[
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)^\top
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)
=
\frac14 \big(\Bv^\top \Av\inv \Bv - 4 \Cv\big).
\\]
Let \\(\Gv = \Bv^\top \Av\inv \Bv - 4 \Cv\\) and call it the discriminant.
Apparently, \\(\Gv\\) has to be symmetric positive semi-definite for the solution to exist, since the left hand side is symmetric positive semi-definite.
Assuming it is, then invoking Lemma 1 gives a general formula for all roots:
\\[
    -\frac12 \Av\inv \Bv + \frac12 \Av^{-\frac12} \Vv \Uv^\top \Gv^{\frac12},
\\]
where \\( \Uv \\) is a column orthonormal matrix consisting of the eigenvectors of \\( \Gv \\), and \\( \Vv \\) is an arbitrary column orthonormal matrix (with an appropriate size).
One can verify that this formula reduces to the usual univariate quadratic formula when all matrices are \\( 1 \times 1 \\).
The matrix quadratic equation has infinitely many roots in most cases, particularly when the discriminant is (strictly) positive definite, because there are infinitely many choices for the column orthonormal matrix \\(\Vv\\).
In the univariate case, however, \\(\Vv\\) is either \\(1\\) or \\( -1 \\), which yields at most two roots.

## An Asymmetric Variant

Now consider the following "asymmetric" variant
\\[
\begin{equation}
\label{eq:asymmetric-equation}
    \Xv^\top \Av \Xv + \Bv^\top \Xv + \Cv = 0,
\end{equation}
\\]
where \\( \Xv \\) is hit by \\( \Bv \\) from the left side only.
Again, we assume all matrices are real, \\( \Av \\) is symmetric positive definite, and \\( \Cv \\) is symmetric.[^is_it_more_general]

If \\( \Xv \\) is a root of the asymmetric equation \eqref{eq:asymmetric-equation}, then \\(\Bv^\top \Xv\\) has to be symmetric.
This is because the right hand side of the identity
\\[
    \Bv^\top \Xv = -\big(\Xv^\top \Av \Xv + \Cv\big)
\\]
is symmetric.
Then, splitting \\( \Bv^\top \Xv \\) into two parts \\( \tfrac12 \Bv^\top \Xv + \tfrac12 \Xv^\top \Bv \\) suggests that the asymmetric equation reduces to the symmetric one and we are ready to apply the quadratic formula we just derived!
However, as tempting as it is, this argument has a bug.
Finding the logical bug is left as an exercise to the readers.

It turns out that the asymmetric matrix quadratic equation is much harder to solve.
Crone (1981) did not obtain a complete solution when the discriminant is singular,
and only gave a solution when the discriminant is positive definite.
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

Yuan, Y., Liu, L., Zhang, H., & Liu, H. (2021). The solutions to the quadratic matrix equation X* AX + B* X + D = 0. Applied Mathematics and Computation, 410, 126463.

### **Notes**
[^is_it_more_general]: It is not clear to me if the asymmetric equation \eqref{eq:asymmetric-equation} is more general than the symmetric one \eqref{eq:symmetric-equation}.
