---
layout: base 
title: "Variational Inference: Adding Data Reduces the Approximate Uncertainty?"
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

Given a dataset \\( \Dc = \\{(\xv_i, y_i)\\} _ {i=1}^{n} \\), a prior \\( p(\wv) \\), and a likelihood \\( p(\Dc \mid \wv) = \prod_{i=1}^{n} p((\xv_i, y_i) \mid \wv) \\), variational inference approximates the posterior distribution \\( p(\wv \mid \Dc) \\) by minimizing the Kullback–Leibler divergence
\\[
\DeclareMathOperator*{\mini}{\mathrm{minimize}}
    \mini_{q \in \Qc} \Ds_\KL(q, p(\wv \mid \Dc)),
\\]
where \\( \Qc \\) is a variational family.
This is equivalent to maximizing the evidence lower bound
\\[
    \maxi_{q \in \Qc} \sum_{i=1}^{n} \Eb _ {q(\wv)} \log p((\xv_i, y_i) \mid \wv) - \Ds_\KL(q(\wv), p(\wv)).
\\]
We assume the prior \\( p(\wv) \\) is a standard Gaussian \\( \Nc(\zero, \Iv) \\), the variational family \\( \Qc \\) is the collection of all Gaussians, and the likelihood \\( p((\xv_i, y_i) \mid \wv) \\) is log-concave in \\( \wv \\) for all \\(i\\).

1. Show that there exists a unique optimal variational distribution \\( q^* \\).

1. Let \\( \Nc(\muv_m, \Sigmav_m) = \argmin_{q \in \Qc} \Ds_\KL\big(q, p(\wv \mid \Dc)) \\) be the optimal variational distribution with the first \\( m \\) data points only.
Show that \\( \Iv \succeq \Sigmav_1 \succeq \Sigmav_2 \succeq \cdots \succeq \Sigmav_n \\).

1. Show that the above inequalities become strict if the likelihood \\( p((\xv_i, y_i) \mid \wv) \\) is strictly log-concave in \\( \wv \\) for all \\( i \\).
That is, the posterior uncertainty contracts with more data.

1. Can you construct a pathological example where increasing the number of data points inflates the posterior uncertainty?
Necessarily, the likelihood \\( p((\xv, y) \mid \wv) \\) cannot be log-concave in \\( \wv \\).


Let's appreciate the results so far for a moment.
In the above setting, the approximate posterior contracts regardless of the label \\( y \\), as long as the likelihood is strictly log-concave.
This might be counter-intuitive in some cases.
For example, consider variational Bayesian logistic regression on a linearly separable dataset.
Adding outliers to the dataset, after which the data becomes non-separable, strictly reduces the posterior uncertainty!