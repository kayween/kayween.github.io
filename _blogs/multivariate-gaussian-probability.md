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
where \\(\phi(\xv) \propto \exp(-\frac12 \xv^\top \xv)\\) is the \\(d\\)-dimensional standard Gaussian density and \\(\Lv = (l_{ij}) \in \Rb^{d \times d}\\) is an invertible lower triangular matrix.
At a first glance, this problem seems very restrictive.
<!-- Let \\( \Av = \Lv \Qv \\) be the LQ decomposition of \\( \Av \\). -->

#### **Non-triangular Matrices**
We consider the seemingly more general case \\(\Pr(\vv \leq \Av \xv \leq \uv)\\), where \\(\Av \in \Rb^{d \times d}\\) is not necessarily triangular.
Let \\(\Av = \Lv \Qv\\) be the LQ decomposition.
Then, a change of variable \\(\xv = \Qv^\top \zv\\) reduces the problem to the triangular case \\(\Pr(\vv \leq \Lv \zv \leq \uv)\\), where \\(\zv\\) is a standard Gaussian random variable.

#### **Non-square Wide Matrices**

#### **Non-standard Gaussians**
Let \\(\xv \sim \Nc(\muv, \Sigmav)\\) be a non-standard Gaussian variable.
Let \\(\Lv \Lv^\top = \Sigmav\\) be the Cholesky decomposition.
Then, a change of variable \\(\xv = \Lv \zv + \muv\\) yields
\\[
    \Pr(\vv \leq \Av \xv \leq \uv) = \Pr(\vv - \Av \muv \leq \Av\Lv \cdot \zv \leq \uv + \Av \muv),
\\]
where \\(\zv\\) is a standard Gaussian random variable.

## Monte Carlo Estimate by Separation of Variables
Consider the probability density of the form
\\[
    <!-- p(\xv) = p_1(x_1) p_2(x_2 \mid x_1) \cdots p_d(x_d \mid x_1, x_2, \cdots, x_{d-1}) -->
    p(\xv) = \prod_{i=1}^{d} p_i(x_i \mid x_{1:i-1})
\\]
and let
\\[
    p(x_i \mid x_1, x_2, \cdots, x_{i-1}) = \frac{\phi(x_i) \cdot \mathbb{1}[\tilde v_i \leq x_i \leq \tilde u_i]}{\Phi(\tilde u_i) - \Phi(\tilde v_i)}.
\\]
Using \\(p(\xv)\\) as the importance weight \eqref{eq:fuck} yields
\\[
\int_{\vv \leq \Lv \xv \leq \uv} \phi(\xv) \diff \xv
=
\int \frac{\phi(\xv)}{p(\xv)} \cdot p(\xv) \diff \xv
=
\Eb_{\xv \sim p(\xv)}\left[\prod_{i=1}^{d} \Big(\Phi(\tilde u_i) - \Phi(\tilde v_i)\Big) \right],
\\]

## Discussion
For instance, 
where the right hand side is readily estimated by Monte Carlo samples.

### **Exercises**
