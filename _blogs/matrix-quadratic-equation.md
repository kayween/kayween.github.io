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

The following lemma deals with a special case when the linear term is absent (Crone, 1981, Proposition 3).

<b>Lemma 1.</b>
<i>
Let \\( \Cv \\) be a symmetric positive semi-definite matrix with a spectral decomposition \\( \Cv = \Uv \Lambdav \Uv^\top \\).
Then \\( \Xv \in \Rb^{m \times n} \\) with \\( m \geq n \\) satisfies \\( \Xv^\top \Xv = \Cv \\) if and only if it is of the form \\( \Xv = \Uv \Qv^\top \Cv^{\frac12} \\) for some column orthogonal matrix \\( \Uv \in \Rb^{m \times n} \\).
</i>

<i>Proof.</i>
The "if" part is trivial.
For the other direction, let \\( \Xv \\) be a matrix that satisfies the equation and define \\( \Uv = \Xv \Av^{-\frac12} \Qv \\).
It is trivial to verify that \\( \Uv \\) is indeed column orthogonal.
Q.E.D.

The heavy lifting is done, and we are ready to tackle the general case.
The idea is exactly the same as the univariate case: completing the square.
It is obvious that the equation can be rewritten as
\\[
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)^\top
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)
=
\frac14 \big(\Bv^\top \Av\inv \Bv - 4 \Cv\big).
\\]
Apparently, \\( \Gv \\) has to be symmetric positive definite for the solution to exist.
If it is, then invoking Lemma 1 solves the equation: \\( \Xv \in \Rb^{m \times n} \\) is a solution if and only if it is of the form
\\[
    -\frac12 \Av\inv \Bv + \frac12 \Av^{-\frac12} \Uv \Qv^\top \Gv^{\frac12}.
\\]

The idea appears to generalize to the asymmetric equation

Observe that if \\( \Xv \\) is a solution of the equation then \\( \Bv^\top \Xv = \Xv^\top \Bv \\) (why?).
Then, it is obvious that the equation can be equivalently rewritten as
\\[
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)^\top
\big(\Av^{\frac12} \Xv + \tfrac12 \Av^{-\frac12} \Bv\big)
=
\frac14 \big(\Bv^\top \Av\inv \Bv - 4 \Cv\big).
\\]
We use \\( \Gv \\) to denote \\( \Bv^\top \Av\inv \Bv - 4 \Cv \\) and call it the discriminant.
Apparently, \\( \Gv \\) has to be symmetric positive definite for the solution to exist.
If it is, then invoking Lemma 1 solves the equation: \\( \Xv \in \Rb^{m \times n} \\) is a solution if and only if it is of the form
\\[
    -\frac12 \Av\inv \Bv + \frac12 \Av^{-\frac12} \Uv \Qv^\top \Gv^{\frac12}.
\\]

### References
Crone, L. (1981). Second order adjoint matrix equations. Linear Algebra and its Applications, 39, 61-71.