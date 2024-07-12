---
layout: base 
title: "Variational Inference: Does Adding Data Reduce the Approximate Posterior Uncertainty?"
---
# {{ page.title }}

**This problem has a critical bug and will be updated in the future.**

Given a dataset \\( \Dc = \\{(\xv_i, y_i)\\} _ {i=1}^{n} \\), a prior \\( p(\wv) \\), and a likelihood \\( p(\Dc \mid \wv) = \prod_{i=1}^{n} p((\xv_i, y_i) \mid \wv) \\), variational inference approximates the posterior \\( p(\wv \mid \Dc) \\) by minimizing the Kullback–Leibler divergence
\\(
    <!-- \mini_{q \in \Qc} -->
    \Ds_\KL(q, p(\wv \mid \Dc))
\\)
inside a variational family \\(\Qc \ni q\\).
Equivalently, this is the same as maximizing the evidence lower bound
\\[
\begin{equation}
\label{eq:elbo}
    \maxi_{q \in \Qc} \sum_{i=1}^{n} \Eb _ {q(\wv)} \log p((\xv_i, y_i) \mid \wv) - \Ds_\KL(q(\wv), p(\wv)).
\end{equation}
\\]
We assume the prior \\( p(\wv) \\) is a standard Gaussian \\( \Nc(\zero, \Iv) \\), the variational family \\( \Qc \\) is the collection of all Gaussians, and the likelihood \\( p((\xv_i, y_i) \mid \wv) \\) is log-concave in \\( \wv \\) for all \\(i \in [n]\\).

1. Show that ELBO \eqref{eq:elbo} has a unique maximizer.

1. For each \\(m \in [n]\\), thanks to the existence and uniqueness of the maximizer, define
\\[
    \Nc(\muv_m, \Sigmav_m) = \argmax_{q \in \Qc} \sum_{i=1}^{m} \Eb _ {q(\wv)} \log p((\xv_i, y_i) \mid \wv) - \Ds_\KL(q(\wv), p(\wv))
\\]
as the optimal variational distribution with the first \\( m \\) data points only.
Prove the inequalities
\\[
\begin{equation}
\label{eq:variational-contraction}
    \Iv \succeq \Sigmav_1 \succeq \Sigmav_2 \succeq \cdots \succeq \Sigmav_n.
\end{equation}
\\]

1. Show that the inequalities in \eqref{eq:variational-contraction} become strict if the likelihood \\( p((\xv_i, y_i) \mid \wv) \\) is strictly log-concave in \\(\wv\\) for all \\(i \in [n]\\).
That is, the approximate posterior uncertainty shrinks with more data.

1. Can you construct a pathological example where increasing the number of data points inflates the posterior uncertainty?
Necessarily, the likelihood \\( p((\xv, y) \mid \wv) \\) cannot be log-concave in \\( \wv \\).

Let's appreciate the results for a moment.
The approximate posterior contracts regardless of the label, as long as the likelihood is strictly log-concave.
This might be counter-intuitive in some cases.
For example, consider variational Bayesian logistic regression on a linearly separable dataset.
Adding outliers to the dataset, after which the data becomes non-separable, strictly reduces the posterior uncertainty.
How weird is that!

## Examples
We provide a few common examples that satisfy the assumptions.

**Bayesian Linear Regression.**
The likelihood is
\\[
    p((\xv_i, y_i) \mid \wv) = \Nc(y_i; \wv^\top \xv_i, \sigma^2) = \frac{1}{\sqrt{2 \pi \sigma^2}} \exp\Big(-\frac{1}{2\sigma^2} \lVert y_i - \wv^\top \xv_i \rVert^2\Big),
\\]
where \\(\sigma > 0\\) is the noise standard deviation.
The log-likelihood is a negative quadratic function, which of course is concave.
The maximizer of ELBO \eqref{eq:elbo} has a closed-form
\\[
\Nc\bigg(
    \big(\Xv^\top \Xv + \sigma^2 \Iv\big)\inv \Xv^\top \yv,
    \Big(\Iv + \frac{1}{\sigma^2} \Xv^\top \Xv\Big)\inv
\bigg),
\\]
where \\(\Xv = (\xv_1, \xv_2, \cdots, \xv_n)^\top\\) and \\(\yv = (y_1, y_2, \cdots, y_n)^\top \in \Rb^n\\).

**Bayesian Logistic Regression.**
The likelihood is
\\[
    p((\xv_i, y_i) \mid \wv) = y_i \cdot s(\wv^\top \xv_i) + (1 - y_i) \cdot (1 - s(\wv^\top \xv_i)),
\\]
where \\(s(\cdot)\\) is the sigmoid function and \\(y_i \in \\{0, 1\\}\\).
One can verify the likelihood is indeed strictly log-concave in \\(\wv\\).
The ELBO \eqref{eq:elbo} does not have a closed-form maximizer in this case, and thus we have to resort to stochastic optimization.
