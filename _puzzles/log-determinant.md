---
layout: base
title:  "Properties of the Log Determinant"
---

\\[
\newcommand{\Sb}{\mathbb{S}}
\newcommand{\zero}{\mathbf{0}}
\newcommand{\Eb}{\mathbb{E}}
\newcommand{\Av}{\mathbf{A}}
\newcommand{\Xv}{\mathbf{X}}
\newcommand{\Iv}{\mathbf{I}}
\newcommand{\wv}{\mathbf{w}}
\newcommand{\xv}{\mathbf{x}}
\newcommand{\zv}{\mathbf{z}}
\newcommand{\Sv}{\mathbf{S}}
\newcommand{\mv}{\mathbf{m}}
\newcommand{\Ds}{\mathsf{D}}
\newcommand{\KL}{\mathrm{KL}}
\DeclareMathOperator*{\maxi}{\mathrm{maximize}}
\newcommand{\Qc}{\mathcal{Q}}
\newcommand{\Nc}{\mathcal{N}}
\newcommand{\Dc}{\mathcal{D}}
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

# Log Determinant

It is well known that \\( f(\Xv) = -\log\det \Xv \\) is a convex function on the positive definite cone \\( \Sb_{+ +}^d \\).

1. Show that \\(h(\Xv) = \Xv\inv\\) defined on \\(\Sb_{+ +}^d\\) is \\(1\\)-Lipschitz in the spectral norm when \\(\Xv \succeq \Iv\\).

1. Redo (1) with the Frobenius norm.

1. Show that \\( f(\Xv) \\) is \\( 1 \\)-smooth in the Frobenius norm when \\(\Xv \succeq \Iv\\).

1. Show that \\( f(\Xv) \\) is \\( 1 \\)-strongly convex in the Frobenius norm when \\(\Xv \preceq \Iv\\).

1. Show that \\( g(\zv, \Xv) = -\log\det(\Xv - \zv \zv^\top) \\) is jointly convex on the domain \\( \Xv - \zv \zv^\top \succ 0 \\).  
Note: This is the convex conjugate of the log-partition function of Gaussian distributions, but you have to do the proof without resorting to the conjugacy.
