---
layout: post
title:  "Machine Learning Puzzles"
date:   2024-05-22 17:21:02 -0400
categories: jekyll update
---

\\[
\newcommand{\Sb}{\mathbb{S}}
\newcommand{\zero}{\mathbf{0}}
\newcommand{\Eb}{\mathbb{E}}
\newcommand{\Av}{\mathbf{A}}
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

I am compiling a collection of interesting exercises that I came across during my research.
Most problems have short descriptions, can be solved in a few lines, and hopefully inspires some thoughts.
<!-- Presumably, LLMs would struggle with these problems due to their originality. -->

# When the Gaussian MLE Fails to Exist

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
Though, a change of variable \\( \mv = \Sigmav\inv \muv \\) and \\( \Sv = \Sigmav\inv \\) makes it convex:
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

# Variational Inference

Given a dataset \\( \\{\xv_i\\} _ {i=1}^{n} \\), a prior \\( p(\zv) \\), and a likelihood \\( p\big( \\{\xv_i\\} _ {i=1}^{n} \mid \zv\big) = \prod_{i=1}^{n} p(\xv_i \mid \zv) \\), variational inference approximates the posterior \\( p\big(\zv \mid \\{\xv_i\\} _ {i=1}^{n}\big) \\) by minimizing the Kullback–Leibler divergence
\\[
\DeclareMathOperator*{\mini}{\mathrm{minimize}}
    \mini_{q \in \Qc} \Ds_\KL\big(q, p\big(\zv \mid \\{\xv_i\\} _ {i=1}^{n}\big)\big),
\\]
where \\( \Qc \\) is a variational family.
This is equivalent to maximizing the evidence lower bound
\\[
    \maxi_{q \in \Qc} \sum_{i=1}^{n} \Eb _ {q(\zv)} \log p(\xv_i \mid \zv) - \Ds_\KL(q(\zv), p(\zv)).
\\]
We assume the prior \\( p(\zv) \\) is a standard Gaussian \\(\Nc(\zero, \Iv)\\), the variational family \\( \Qc \\) is the collection of all Gaussians, and the log likelihood \\( \log p(\xv_i \mid \zv) \\) is concave in \\(\zv\\) for all \\(i\\).

1. Show that there exists a unique optimal variational distribution \\( q^* \\).

2. Let \\( \Nc(\muv_m, \Sigmav_m) = \argmin_{q \in \Qc} \Ds_\KL\big(q, p\big(\zv \mid \\{\xv_i\\} _ {i=1}^{m}\big)\big) \\) be the optimal variational distribution with the first \\( m \\) data points only.
Show that \\( \Iv \succeq \Sigmav_1 \succeq \Sigmav_2 \succeq \cdots \succeq \Sigmav_n \\).

3. Show that the inequalities become strict if \\(\log p(\xv_i \mid \zv) \\) is stictly concave in \\(\zv\\) for all \\(i\\).
That is, the posterior uncertainty contracts with more data.

4. Can you construct a pathological example where increasing the number of data points inflates the posterior uncertainty?
Necessarily, the likelihood \\( p(\xv \mid \zv) \\) cannot be log-concave in \\( \zv \\).

# Log Determinant

It is well known that \\( f(\Xv) = -\log\det \Xv \\) is a convex function on the positive definite cone \\( \Sb_{+ +}^d \\).

1. Let \\(h(\Xv) = \Xv\inv\\) be a map defined on \\(\Sb_{+ +}^d\\).
Show that \\( h \\) is \\(1\\)-Lipschitz in the spectral norm when \\(\Xv \succeq \Iv\\).
Repeat the exercise with the Frobenius norm.

2. Show that \\( f(\Xv) \\) is \\( 1 \\)-smooth in the Frobenius norm when \\(\Xv \succeq \Iv\\).

3. Show that \\( f(\Xv) \\) is \\( 1 \\)-strongly convex in the Frobenius norm when \\(\Xv \preceq \Iv\\).

4. Show that \\( g(\zv, \Xv) = -\log\det(\Xv - \zv \zv^\top) \\) defined on the domain \\( \Xv - \zv \zv^\top \succ 0\\) is jointly convex in both \\( \zv \\) and \\( \Xv \\).
Note: This is the convex conjugate of the log-partition function of Gaussian distributions, but you have to do the proof without resorting to the conjugacy.
