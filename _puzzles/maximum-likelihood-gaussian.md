---
layout: base
title: "Gaussian Maximum Likelihood Estimation: When It Does Not Exist"
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

# {{ page.title }}

In undergraduate statistics classes, we learned that the empirical mean \\( \hat\muv = \frac1n \sum_{i=1}^{n} \xv_i \\) and the empirical covariance \\( \widehat\Sigmav = \frac{1}{n} \sum_{i=1}^{n} (\xv_i - \hat\muv) (\xv_i - \hat \muv)^\top \\) are the maximum likelihood estimators for Gaussian distributions \\( \Nc(\muv, \Sigmav) \\).
Concretely, they "minimize" the negative log likelihood:
\\[
\hat\muv, \hat\Sigmav = \argmin_{\muv, \Sigmav} \ell(\muv, \Sigmav),
\\]
where
\\(
\ell(\muv, \Sigmav) = \frac1n \sum_{i=1}^{n} \big(
    (\xv_i - \muv)^\top \Sigmav\inv (\xv_i - \muv) +
    \log\det \Sigmav
\big)
\\).

1. Show that the negative log likelihood \\( \ell(\muv, \Sigmav) \\) is a non-convex function.
Though, a change of variable \\( \mv = \Sigmav\inv \muv \\) and \\( \Sv = \Sigmav\inv \\) makes it jointly convex:
\\[
\ell(\mv, \Sv) =
\frac1n \sum_{i=1}^{n} \Big(
    \xv_i^\top \Sv \xv_i -
    2 \xv_i^\top \mv +
    \mv^\top \Sv\inv \mv -
    \log\det \Sv
\Big).
\\]
Note: The convexity is not by accident.
Maximum likelihood estimation reduces to a convex optimization problem for all exponential family distributions.

2. Show that \\( \ell \\) is unbounded below when the empirical covariance is singular, in which case the maximum likelihood estimator does not exist.

3. Consider the solution path of the following regularized problem
\\[
<!-- \mv_\*(\gamma), \Sv_\*(\gamma) = \argmin_{\mv, \Sv} \ell(\mv, \Sv) + \gamma \lVert \Sv \rVert_*, -->
\muv^\*(\gamma), \Sigmav^\*(\gamma) = \argmin_{\muv, \Sigmav} \ell(\muv, \Sigmav) + \gamma \big\lVert \Sigmav\inv \big\rVert_*,
\\]
where \\( \lVert \cdot \rVert_\* \\) denotes the nuclear norm.
Show that \\( \lim_{\gamma \to 0} \muv^\*(\gamma) = \hat\muv \\) and \\( \lim_{\gamma \to 0} \Sigmav^\*(\gamma) = \widehat\Sigmav \\), regardless whether \\( \widehat \Sigmav \\) is singular or not.

4. Redo (3) by replacing the nuclear norm with the Frobebius norm square \\( \frac12 \lVert \cdot \rVert_\F^2 \\).
Does it change anything?
What about other norms?

