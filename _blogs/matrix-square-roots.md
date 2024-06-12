---
layout: base
title: Matrix Square Roots
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

This blog post discusses matrix square roots.
A matrix square root of a \\( n \times n \\) square matrix \\( \Av \\) is defined as any solution to the equation
\\[
    \Xv \Xv = \Av.
\\]
The term "matrix square root" is often overloaded.
For example, the solution to \\( \Xv^\top \Xv = \Av \\) is sometimes called matrix square roots as well, but will not be the focus of this post.

For a general square matrix \\( \Av \\), there might be zero, finitely many, or even infinitely many matrix square roots.
We give an example for each case:

1. $$
\left(
\begin{matrix}
0 & 1 \\
0 & 0 
\end{matrix}
\right)
$$
has no matrix square root at all (see Exercise 1 for a more general construction);

1. $$
\left(
\begin{matrix}
1 & 0 \\
0 & 0
\end{matrix}
\right)
$$
has two square roots 
$$
\left(
\begin{matrix}
1 & 0 \\
0 & 0
\end{matrix}
\right)
$$
and
$$
\left(
\begin{matrix}
-1 & 0 \\
0 & 0
\end{matrix}
\right)
$$;

1. $$
\left(
\begin{matrix}
1 & 0 \\
0 & 1 
\end{matrix}
\right)
$$
has infinitely many square roots (not surprising),
some of which cannot even be found by the spectral decomposition (surprise?).

Our goal is to give a complete characterization of the number of square roots, and give a general formula.

See the excellent note by Gallier (2008).

### **Exercises**
1. Let \\( \Av \\) be a \\( n \times n \\) square matrix with \\( n \geq 2 \\).
Show that \\( \Av \\) does not have a square root if \\( n \\) is the smallest number such that \\( \Av^n = 0 \\).

### **References**
Some examples are taken freely from math stack exchange, which we greatly appreciate.

Gallier, J. (2008). Logarithms and square roots of real matrices. arXiv preprint arXiv:0805.0245.