---
layout: base
title: Matrix Quadratic Equations
---

\\[
\newcommand{\Sb}{\mathbb{S}}
\newcommand{\Rb}{\mathbb{R}}
\newcommand{\zero}{\mathbf{0}}
\newcommand{\Eb}{\mathbb{E}}
\newcommand{\Av}{\mathbf{A}}
\newcommand{\Bv}{\mathbf{B}}
\newcommand{\Cv}{\mathbf{C}}
\newcommand{\Gv}{\mathbf{G}}
\newcommand{\Qv}{\mathbf{Q}}
\newcommand{\Sv}{\mathbf{S}}
\newcommand{\Uv}{\mathbf{U}}
\newcommand{\Vv}{\mathbf{V}}
\newcommand{\Xv}{\mathbf{X}}
\newcommand{\Iv}{\mathbf{I}}
\newcommand{\xv}{\mathbf{x}}
\newcommand{\zv}{\mathbf{z}}
\newcommand{\Sv}{\mathbf{S}}
\newcommand{\mv}{\mathbf{m}}
\newcommand{\Ds}{\mathsf{D}}
\newcommand{\KL}{\mathrm{KL}}
\DeclareMathOperator*{\maxi}{\mathrm{maximize}}
\newcommand{\Qc}{\mathcal{Q}}
\newcommand{\Nc}{\mathcal{N}}
\newcommand{\muv        }{\boldsymbol \mu        }
\newcommand{\Sigmav     }{\boldsymbol \Sigma     }
\newcommand{\lambdav    }{\boldsymbol \lambda    }
\newcommand{\Lambdav    }{\boldsymbol \Lambda    }
\newcommand{\inv}{^{-1}}
\newcommand{\F}{\mathrm{F}}
\\]

\\[
\DeclareMathOperator*{\argmin}{argmin}
\\]

Everyone knows how to solve a quadratic (univariate) equation \\( a x^2 + b x + c = 0 \\).
But it is less obvious how to solve its matrix variant
\\[
    \Xv^\top \Av \Xv + \tfrac12 \Bv^\top \Xv + \tfrac12 \Xv^\top \Bv + \Cv = 0,
\\]
where \\( \Xv, \Bv \in \Rb^{m \times n} \\), \\( \Cv \in \Rb^{n \times n} \\) is symmetric, and \\( \Av \in \Rb^{m \times m} \\) is symmetric positive definite.
All matrices are assumed to be real (we earned this privilege as computer scientists).
In what follows, we present the solution of the quadratic matrix equation developed by Crone (1981).

## A Special Case
The following lemma deals with a special case without the linear term (Crone, 1981, Proposition 3).

**Lemma 1.**
<i>
Let \\( \Sv \\) be a \\( n \times n \\) rank-\\( r \\) symmetric positive semi-definite matrix with a compact SVD \\( \Sv = \Uv \Lambdav \Uv^\top \\).
Then, a matrix \\( \Xv \in \Rb^{m \times n} \\) with \\( m \geq r \\) satisfies the equation \\( \Xv^\top \Xv = \Sv \\) if and only if it is of the form \\( \Xv = \Vv \Lambdav^{\frac12} \Uv^\top = \Vv \Uv^\top \Sv^{\frac12} \\) for some column orthogonal matrix \\( \Vv \in \Rb^{m \times r} \\).
The equation has no solution when \\( m < r \\).
</i>

<i>Proof.</i>
The "if" direction is trivial.
Only the other direction needs a proof.
Let \\( \Xv \\) be a solution and define \\( \Vv = \Xv \Uv \Lambdav^{-\frac12} \\).
It is easy to verify that \\( \Vv \\) is indeed column orthogonal.
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
When \\( m < r\\), the equation's two sides do not have the same rank, and thus has no solution.
**Q.E.D.**

## The General Case

Now we are ready to tackle the general case of the quadratic matrix equation.
The idea is exactly the same as in the univariate case: completing the square.
It is obvious that the equation can be rewritten as
\\[
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)^\top
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)
=
\frac14 \big(\Bv^\top \Av\inv \Bv - 4 \Cv\big).
\\]
Use \\( \Gv \\) to denote \\( \Bv^\top \Av\inv \Bv - 4 \Cv \\) and call it the discriminant.
Apparently, \\( \Gv \\) has to be symmetric positive semi-definite for the solution to exist.
Assuming it is, then invoking Lemma 1 gives the solution:
\\[
    -\frac12 \Av\inv \Bv + \frac12 \Av^{-\frac12} \Vv \Uv^\top \Gv^{\frac12},
\\]
where the columns of \\( \Uv \\) are the eigenvectors of \\( \Gv \\) corresponding to nonzero eigenvalues, and \\( \Vv \\) is an arbitrary column unitary matrix (with an appropriate size).
One can verify that this formula reduces to the usual quadratic formula in the univariate case.
Interestingly, a crucial difference is that the matrix variant has infinitely many solutions whenever the discriminant is positive definite.
However, in the univariate case, the column unitary matrix \\( \Vv \\) has only two options \\( 1 \\) and \\( -1 \\), which yields at most two solutions.

## A Even More General Case

Now consider the following "asymmetric" variant
\\[
    \Xv^\top \Av \Xv + \Bv^\top \Xv + \Cv = 0,
\\]
where \\( \Xv \\) is hit by \\( \Bv \\) from the left side only.

Observe that if \\( \Xv \\) is a solution then \\( \Bv^\top \Xv = \Xv^\top \Bv \\).
This is because the right hand side of the identity
\\[
    \Bv^\top \Xv = -\big(\Xv^\top \Av \Xv + \Cv\big),
\\]
is symmetric.
Then, splitting \\( \Bv^\top \Xv \\) into two part \\( \tfrac12 \Bv^\top \Xv + \tfrac12 \Xv^\top \Bv \\) implies that we can reuse the previous argument!
However, as tempting as it is, this argument has a bug.
Finding the logical bug is left as an exercise to the readers.

It turns out that the asymmetric variant is much harder to solve.
Crone (1981) gave the solution when the discriminant is positive definite, but did not obtain the solution for singular discriminant.
The result, however, is much more messy and thus is omitted here.
It wasn't until recently that a general solution was discovered (Yuan et al., 2021).
I have not checked this paper in detail, but again the solution is complicated.
Interested readers may refer to their papers.

### References
Crone, L. (1981). Second order adjoint matrix equations. Linear Algebra and Its Applications, 39, 61-71.

Yuan, Y., Liu, L., Zhang, H., & Liu, H. (2021). The solutions to the quadratic matrix equation X* AX+ B* X+ D= 0. Applied Mathematics and Computation, 410, 126463.