---
layout: base
title: Multivariate Normal Probability
---
# {{ page.title }}

The cumulative distribution function (CDF) is arguably the most basic property of a distribution.
The multivariate normal distribution is probably the most basic and frequently used distribution in practice.
Yet, perhaps surprisingly, the CDF of a multivariate normal distribution is intractable in general.
That is, there is no "easy" way to compute the probability
\\[
    \Pr(\xv \leq \uv), \; \text{where} \; \xv \sim \Nc(\muv, \Sigmav).
\\]
Closed-form expressions do exist for special cases, e.g., \\(\xv\\) is univariate or has independent coordinates.
However, no analytical expressions exist in general.
The arising question is how to numerically estimate the probability, which will be the focus of this post.

## Problem
We are going to formulate the problem slightly more general than the normal CDF.

Given a multivariate standard normal random variable \\(\xv \sim \Nc(\zero, \Iv)\\), we are interested in estimating the probability of the form
\\[
\begin{equation}
\label{eq:normal-prob}
    \Pr(\vv \leq \Av \xv \leq \uv) = \int_{\vv \leq \Lv \xv \leq \uv} \phi(\xv) \diff \xv,
\end{equation}
\\]
where \\(\phi(\xv) \propto \exp(-\frac12 \xv^\top \xv)\\) is the standard normal density and \\(\Av\\) is a \\(d \times d\\) nonsingular lower triangular matrix.
The seemingly more general case where \\(\Av \in \Rb^{m \times d}\\) is not square (but still row full rank) and \\(\xv\\) is non-standard normal reduces to \eqref{eq:normal-prob} by a change of variables.

#### **Non-Triangular Square Matrices**
Suppose \\(\Av\\) is square.
Let \\(\Av = \Lv \Qv^\top\\) be the LQ decomposition.
Then, a change of variables \\(\xv = \Qv \zv\\) reduces the problem to \\(\Pr(\vv \leq \Lv \zv \leq \uv)\\), where \\(\zv\\) is standard normal and \\(\Lv\\) is lower triangular.

#### **Non-Square Wide Matrices**
We have \\(\Av \in \Rb^{m \times d}\\) with \\(m < d\\).
Similar to the previous argument, applying a LQ decomposition and a change of variables reduces the problem to
\\(\Pr(\vv \leq \Lv \zv \leq \uv)\\),
where \\(\Lv \in \Rb^{m \times d}\\) and \\(\Qv \in \Rb^{d \times d}\\).
Note that the last \\((d - m)\\) columns of \\(\Lv\\) are zeros, which implies that the last \\((d - m)\\) coordinates of \\(\zv\\) are irrelevant.
Thus, marginalizing out those coordinates reduces the problem to \eqref{eq:normal-prob}.

#### **Non-Standard Normal**
Let \\(\xv \sim \Nc(\muv, \Sigmav)\\) be a non-standard normal random variable.
Let \\(\Lv \Lv^\top = \Sigmav\\) be the Cholesky decomposition.
A change of variables \\(\xv = \Lv \zv + \muv\\) yields \\(\Pr(\vv \leq \Av(\Lv \zv + \muv) \leq \uv)\\), where \\(\zv\\) is standard normal.
Reuse the previous argument completes the reduction.

## Monte Carlo Estimate by Separation of Variables
We are going to use importance sampling to estimate the integral \eqref{eq:normal-prob}.
Since \\(\Av\\) is lower triangular, the linear inequality constraint \\(\vv \leq \Av \xv \leq \uv\\) can be rewritten as
\\[
    \tilde v_i \leq x_i \leq \tilde u_i, \quad i = 1, 2, \cdots, d,
\\]
where \\(\tilde v_i = \frac{1}{a_{ii}} \Big(v_i - \sum_{j=1}^{i-1} a_{ij} x_j \Big)\\)  and \\(\tilde u_i = \frac{1}{a_{ii}} \Big(u_i - \sum_{j=1}^{i-1} a_{ij} x_j \Big)\\).
Note that \\(\tilde v_i\\) and \\(\tilde u_i\\) are functions of the first \\(i-1\\) coordinates only.

Let \\(x_{1:i-1}\\) denote the first \\(i-1\\) coordinates in \\(\xv\\).
Consider the probability distribution of the form
\\[
\begin{equation}
\label{eq:importance-dist}
    p(\xv) = \prod_{i=1}^{d} p_i(x_i \mid x_{1:i-1}),
\end{equation}
\\]
where
\\[
\begin{equation}
\label{eq:conditional}
    p_i(x_i \mid x_{1:i-1}) = \frac{\phi(x_i) \cdot \mathbf{1}[\tilde v_i \leq x_i \leq \tilde u_i]}{\Phi(\tilde u_i) - \Phi(\tilde v_i)}.
\end{equation}
\\]

Sampling from \eqref{eq:importance-dist} is easy, because each \\(p_i\\) is a univariate truncated normal distribution.
Using \\(p(\xv)\\) as the importance weight to estimate \eqref{eq:normal-prob} yields
\\[
\eqref{eq:normal-prob}
=
\int \frac{\phi(\xv)}{p(\xv)} \cdot p(\xv) \diff \xv
=
\Eb_{\xv \sim p(\xv)}\left[\prod_{i=1}^{d} \Big(\Phi(\tilde u_i) - \Phi(\tilde v_i)\Big) \right],
\\]
where the right hand side is readily estimated by Monte Carlo samples.

## Discussion
This fundamental problem of estimating high dimensional normal probability is still under active research.
The method of separation of variables was developed in the 1990s (Genz, 1992).
Recent developments are still based on this idea.

The separation of variables method we presented above is based on univariate conditioning: the conditional distribution \eqref{eq:conditional} predicts one variable at a time.
Bivariate conditioning (Genz and Trinh, 2016) uses conditional distributions that predict two variables at a time: \\(p(x_{2i + 1}, x_{2i + 2} \mid x_{1:2i})\\).

A natural idea is reordering the variables in the conditional distributions \eqref{eq:conditional}.
For instance, Genz and Bretz (2009) propose a heuristic to find a permutation that reduces the variance of the Monte Carlo estimate.

Minimax tilting modifies the conditional distribution \eqref{eq:conditional} by an exponential tilting (Botev, 2017).
Roughly speaking, they replace the standard normal density \\(\phi(x)\\) in the conditional distribution with a translated normal density \\(\phi(x; \mu, 1)\\), where the tilting parameter \\(\mu\\) is optimized.

A careful reader will notice that we have ignored the case when the matrix \\(\Av\\) has more rows than columns, i.e., more constraints than the dimension.
In fact, this is a hard one, which has to rely on Markov chain Monte Carlo methods combined with sophisticated numerical integration methods.

### **Exercises**
1. Show that the separation of variables estimator still works when \\(\vv = -\infty\\).

1. Show the distribution \eqref{eq:importance-dist} is **not** the truncated normal distribution \\(\phi(\xv \mid \vv \leq \Av \xv \leq \uv)\\).

1. Compared to \eqref{eq:importance-dist}, is it sensible to use instead the uniform distribution over the polytope \\(\\{\xv \in \Rb^d: \vv \leq \Av \xv \leq \uv\\}\\) as the importance distribution? Why or why not?

### **References**
Botev, Z. I. (2017). The normal law under linear restrictions: simulation and estimation via minimax tilting. Journal of the Royal Statistical Society Series B: Statistical Methodology, 79(1), 125-148.

Genz, A. (1992). Numerical computation of multivariate normal probabilities. Journal of computational and graphical statistics, 1(2), 141-149.

Genz, A., & Bretz, F. (2009). Computation of multivariate normal and t probabilities (Vol. 195). Springer Science & Business Media.

Genz, A., & Trinh, G. (2016). Numerical computation of multivariate normal probabilities using bivariate conditioning. In Monte Carlo and Quasi-Monte Carlo Methods: MCQMC, Leuven, Belgium, April 2014 (pp. 289-302). Springer International Publishing.
