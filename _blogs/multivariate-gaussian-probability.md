---
layout: base
title: Multivariate Normal Probability
---
# {{ page.title }}

Given a multivariate standard normal random variable \\(\xv \sim \Nc(\zero, \Iv)\\), we are interested in estimating the (intractable) probability of the form
\\[
\begin{equation}
\label{eq:normal-prob}
    \Pr(\vv \leq \Av \xv \leq \uv) = \int_{\vv \leq \Lv \xv \leq \uv} \phi(\xv) \diff \xv,
\end{equation}
\\]
where \\(\phi(\xv) \propto \exp(-\frac12 \xv^\top \xv)\\) is the standard normal density and \\(\Av\\) is a \\(d \times d\\) nonsingular lower triangular matrix.
The seemingly more general case where \\(\Av \in \Rb^{m \times d}\\) is row full rank and \\(\xv\\) is non-standard reduces to the form \eqref{eq:normal-prob}.

#### **Non-Triangular Square Matrices: \\(m = d\\)**
Suppose \\(\Av\\) is square.
Let \\(\Av = \Lv \Qv^\top\\) be the LQ decomposition.
Then, a change of variables \\(\xv = \Qv \zv\\) reduces the problem to \\(\Pr(\vv \leq \Lv \zv \leq \uv)\\), where \\(\zv\\) is standard normal and \\(\Lv\\) is lower triangular.

#### **Non-Square Wide Matrices: \\(m < d\\)**
Similar to the previous argument, applying the LQ decomposition and a change of variables reduces the problem to
\\(\Pr(\vv \leq \Lv \zv \leq \uv)\\),
where \\(\Lv \in \Rb^{m \times d}\\) and \\(\Qv \in \Rb^{d \times d}\\).
Note that the last \\(d - m\\) columns of \\(\Lv\\) are zeros, which implies that the last \\(d - m\\) coordinates of \\(\zv\\) is irrelevant.
Thus, marginalizing out those coordinates reduces the problem to \eqref{eq:normal-prob}.

#### **Non-Standard Normal**
Let \\(\xv \sim \Nc(\muv, \Sigmav)\\) be a non-standard normal random variable.
Let \\(\Lv \Lv^\top = \Sigmav\\) be the Cholesky decomposition.
A change of variables \\(\xv = \Lv \zv + \muv\\), where \\(\zv\\) is standard normal, reduces the problem to the previous case.

## Monte Carlo Estimate by Separation of Variables
We are going to use importance sampling to estimate the integral \eqref{eq:normal-prob}.
Separatino of variables 

The linear inequality constraint \\(\vv \leq \Lv \xv \leq \uv\\) can be rewritten as
\\[
    \frac{1}{l_{ii}} \Big(v_i - \sum_{j=1}^{i-1} l_{ij} x_j \Big)
    \leq x_i \leq
    \frac{1}{l_{ii}} \Big(u_i - \sum_{j=1}^{i-1} l_{ij} x_j \Big), \quad i = 1, 2, \cdots, d.
\\]

Let \\(x_{1:i-1}\\) denote the first \\(i-1\\) coordinates in \\(\xv\\).
Consider the probability density of the form
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
    p_i(x_i \mid x_{1:i-1}) = \frac{\phi(x_i) \cdot \mathbb{1}[\tilde v_i \leq x_i \leq \tilde u_i]}{\Phi(\tilde u_i) - \Phi(\tilde v_i)}.
\end{equation}
\\]

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
Estimating the normal probability is of immerse importance in practice.
For instance, the cumulative distribution function (CDF) of a multivariate normal distribution is exactly of the form \eqref{eq:normal-prob} with \\(\Lv = \Iv\\) and \\(\vv = -\infty\\).
When the normal distribution does not have independent coordinates, there is no easy way to compute its CDF, and thus we have to rely on Monte Carlo methods.
Yes, the method we just presented still works when \\(\vv\\) is infinity, but we leave it as an exercise for the readers to verify it is indeed the case.

This fundamental problem of estimating high dimensional normal probability is still under active research.
The method of separation of variables was developed in the 1990s (Genz, 1992).
Recent developments are still based on this idea.

The separation of variables method we presented above is based on univariate conditioning: the conditional distribution \eqref{eq:conditional} predicts one variable at a time.
Bivariate conditioning (Genz and Trinh, 2016) uses conditional distributions that predict two variables at a time: \\(p(x_{2i + 1}, x_{2i + 2} \mid x_{1:2i})\\).

A natural idea is reordering the variables in the conditional distributions \eqref{eq:conditional}.
For instance, Genz and Bretz (2009) propose a heuristic to find a permutation that reduces the variance of the Monte Carlo estimate.

Minimax tilting modifies the conditional distribution \eqref{eq:conditional} by an exponential tilting (Botev, 2017).
Roughly speaking, they replace the standard normal density \\(\phi(x)\\) in the conditional distribution with a translated normal density \\(\phi(x; \mu, 1)\\), and optimize the tiling parameter \\(\mu\\) to find the best conditional distribution.

A careful reader will notice that we have ignored the case when the matrix \\(\Av\\) has more rows than columns, i.e., more constraints than the dimension.
In fact, this is a hard one, which has to rely on Markov chain Monte Carlo methods combined with sophisticated numerical integration methods.

### **Exercises**
1. Show the distribution \eqref{eq:importance-dist} is not the truncated normal distribution \\(\phi(\xv \mid \vv \leq \Lv \xv \leq \uv)\\).

### **References**
Botev, Z. I. (2017). The normal law under linear restrictions: simulation and estimation via minimax tilting. Journal of the Royal Statistical Society Series B: Statistical Methodology, 79(1), 125-148.

Genz, A. (1992). Numerical computation of multivariate normal probabilities. Journal of computational and graphical statistics, 1(2), 141-149.

Genz, A., & Bretz, F. (2009). Computation of multivariate normal and t probabilities (Vol. 195). Springer Science & Business Media.

Genz, A., & Trinh, G. (2016). Numerical computation of multivariate normal probabilities using bivariate conditioning. In Monte Carlo and Quasi-Monte Carlo Methods: MCQMC, Leuven, Belgium, April 2014 (pp. 289-302). Springer International Publishing.
