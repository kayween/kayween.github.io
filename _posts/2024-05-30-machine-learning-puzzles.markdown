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

I am compiling a collection of interesting exercises that I came across during my research.
Most problems have short descriptions, can be proved in a few lines, and are hopefully educational.
<!-- Presumably, LLMs would struggle with these problems due to their originality. -->

# Variational Inference

Given a prior \\( p(\zv) \\), a likelihood \\( p(\xv \mid \zv) \\), and a dataset \\( \\{\xv_i\\} _ {i=1}^{n} \\), variational inference approximates the posterior \\( p\big(\zv \mid \\{\xv_i\\} _ {i=1}^{n}\big) \\) by minimizing the Kullback–Leibler divergence
\\[
\DeclareMathOperator*{\mini}{\mathrm{minimize}}
    \mini_{q \in \Qc} \Ds_\KL\big(q, p\big(\zv \mid \\{\xv_i\\} _ {i=1}^{n}\big)\big)
\\]
inside a variational family \\( \Qc \\).
This is equivalent to maximizing the evidence lower bound
\\[
    \maxi_{q \in \Qc} \sum_{i=1}^{n} \Eb _ {q(\zv)} \log p(\xv_i \mid \zv) - \Ds_\KL(q(\zv), p(\zv)).
\\]
We assume the prior \\( p(\zv) \\) is a standard Gaussian \\(\Nc(\zero, \Iv)\\), the variational family \\( \Qc \\) is the collection of all Gaussians, and the log likelihood \\( \log p(\xv_i \mid \zv) \\) is concave in \\(\zv\\) for all \\(i\\).

1. Show that there exists a unique maximizer \\( q^* \\).
2. Let \\( \Nc(\muv_m, \Sigmav_m) = \argmin_{q \in \Qc} \Ds_\KL\big(q, p\big(\zv \mid \\{\xv_i\\} _ {i=1}^{m}\big)\big) \\) be the optimal variational distribution with the first \\( m \\) data points only.
Show that \\( \Iv \succeq \Sigmav_1 \succeq \Sigmav_2 \succeq \cdots \succeq \Sigmav_n \\).
3. Show that the inequalities become strict if \\(\log p(\xv_i \mid \zv) \\) is stictly concave in \\(\zv\\) for all \\(i\\).
That is, the posterior uncertainty contracts with more data. 
4. Can you construct a pathological example where increasing the number of data points inflates the posterior uncertainty?

# Short Questions
- Let \\(f(\Xv) = \Xv\inv\\) be a map defined on \\(\Sb_{++}^d\\).
Show that \\(f\\) is \\(1\\)-Lipschitz in the spectral norm when \\(\Xv \succeq \Iv\\).
Repeat the exercise with the Frobenius norm.
- Show that \\(\frac12 \xv^\top \Av\inv \xv\\) is jointly convex in both \\(\xv\\) and \\(\Av\\), where \\(\Av \in \Sb_{++}^d\\).
- Show that \\(f(\xv, \Av) = -\log\det(\Av - \xv \xv^\top)\\) is jointly convex in both \\(\xv\\) and \\(\Av\\) on its domain \\(\Av - \xv \xv^\top \succ 0\\).
P.S. This is the convex conjugate of the log-partition function of Gaussian distributions, but you have to do the proof without resorting to the conjugacy.