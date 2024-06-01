---
layout: post
title:  "Machine Learning Puzzles"
date:   2024-05-22 17:21:02 -0400
categories: jekyll update
---

\\[
\newcommand{\Eb}{\mathbb{E}}
\newcommand{\Av}{\mathbf{A}}
\newcommand{\xv}{\mathbf{x}}
\newcommand{\zv}{\mathbf{z}}
\newcommand{\Ds}{\mathsf{D}}
\newcommand{\KL}{\mathrm{KL}}
\DeclareMathOperator*{\maxi}{\mathrm{maximize}}
\newcommand{\Qc}{\mathcal{Q}}
\newcommand{\Nc}{\mathcal{N}}
\newcommand{\muv        }{\boldsymbol \mu        }
\newcommand{\Sigmav     }{\boldsymbol \Sigma     }
\newcommand{\inv}{^{-1}}
\\]

\\[
\DeclareMathOperator*{\argmin}{argmin}
\\]

# Variational Inference

Given a prior \\( p(\zv) \\), a likelihood \\( p(\xv \mid \zv) \\), and a dataset \\( \\{\xv_i\\} _ {i=1}^{n} \\), variational inference approximates the posterior \\( p(\zv \mid \xv_1, \xv_2, \cdots, \xv_n) \\) by minimizing the Kullback–Leibler divergence
\\[
\DeclareMathOperator*{\mini}{\mathrm{minimize}}
    \mini_{q \in \Qc} \Ds_\KL\big(q, p\big(\zv \mid \\{\xv_i\\} _ {i=1}^{n}\big)\big)
\\]
inside a variational family \\( \Qc \\).
Up to a constant, it is equivalent to maximizing the evidence lower bound
\\[
    \maxi_{q \in \Qc} \sum_{i=1}^{n} \Eb _ {q(\zv)} \log p(\xv_i \mid \zv) - \Ds_\KL(q(\zv), p(\zv)).
\\]
We assume the prior \\( p(\zv) \\) is a Gaussian, the variational family \\( \Qc \\) is the set of all Gaussians, and the likelihood \\( p(\xv_i \mid \zv) \\) is log-concave for \\( i = 1, 2, \cdots, n \\).

1. Show that there exists a unique optimum \\( q^* \\).
2. Let \\( \Nc(\muv_m, \Sigmav_m) = \argmin_{q \in \Qc} \Ds_\KL\big(q, p\big(\zv \mid \\{\xv_i\\} _ {i=1}^{m}\big)\big) \\) be the optimal variational approximation with the first \\( m \\) data points only.
Show that \\( \Sigmav_1 \succeq \Sigmav_2 \succeq \cdots \succeq \Sigmav_n \\).
That is, the approximate posterior uncertainty contracts with more data.


# Short Questions
1. If \\( \Av \\) is symmetric positive definite, then \\(\frac12 \xv^\top \Av \xv\\) is convex in \\(\xv\\).
However, is it jointly convex in both \\(\xv\\) and \\(\Av\\)? What about \\(\frac12 \xv^\top \Av\inv \xv\\)?

2. Show that \\(f(\xv, \Av) = -\log\det(\Av - \xv \xv^\top)\\) is jointly convex in both \\(\xv\\) and \\(\Av\\) on its domain \\(\Av - \xv \xv^\top \succ 0\\).
P.S. This is the convex conjugate of the log-partition function of Gaussian distributions, but you have to do the proof without resorting the conjugacy.