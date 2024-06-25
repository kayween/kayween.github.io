---
layout: base
title: Multivariate Gaussian Probability
---
# {{ page.title }}

Given a multivariate standard Gaussian random variable \\(\xv \sim \Nc(\zero, \Iv)\\), we are interested in the probability of the form
\\[
\begin{equation}
\label{eq:fuck}
    \Pr(\vv \leq \Lv \xv \leq \uv) = \int_{\vv \leq \Lv \xv \leq \uv} \phi(\xv) \diff \xv,
\end{equation}
\\]
where \\(\phi(\xv) \propto \exp(-\frac12 \xv^\top \xv)\\) is the \\(d\\)-dimensional standard Gaussian density and \\(\Lv = (l_{ij}) \in \Rb^{d \times d}\\) is a nonsingular lower triangular matrix.

<!-- Let \\( \Av = \Lv \Qv \\) be the LQ decomposition of \\( \Av \\). -->

## Separation of Variables

Consider the probability density of the form
\\[
    <!-- p(\xv) = p_1(x_1) p_2(x_2 \mid x_1) \cdots p_d(x_d \mid x_1, x_2, \cdots, x_{d-1}) -->
    p(\xv) = \prod_{i=1}^{d} p_i(x_i \mid x_{1:i-1})
\\]
and let
\\[
    p(x_i \mid x_1, x_2, \cdots, x_{i-1}) = \frac{\phi(x_i) \cdot \mathbb{1}[\tilde v_i \leq x_i \leq \tilde u_i]}{\Phi(\tilde u_i) - \Phi(\tilde v_i)}.
\\]
We estimate it with importance sampling \ref{eq:fuck}
\\[
    \int_adsf 
\\]
