---
layout: base
title: Multivariate Normal Probability
---
# {{ page.title }}

Given a Gaussian random variable \\(\xv \sim \Nc(\muv, \Sigmav)\\) and a constant vector \\(\uv \in \Rb^d\\), we are interested in computing the probability
\\[
    \Pr(\xv \leq \uv),
\\]
where the inequality is element-wise.

This probability is exactly the CDF of multivariate normal distributions---a fundamental quantity that characterizes the behavior of the distribution.
Yet, perhaps surprisingly, it is intractable (no analytical expression) to compute except in a few special cases.
Thus, one has to resort to Monte Carlo methods to numerically estimate the probability.

## A Slightly More General Problem

We introduce a slightly more general formulation with the following two modifications:
1. We restrict \\(\xv \sim \Nc(\zero, \Iv)\\) to be a standard normal random variable;
1. But we allow a more general linear inequality \\(\vv \leq \Av \xv \leq \uv\\).

Namely, we arrive at the integral
\\[
\begin{equation}
\label{eq:normal-prob}
    \Pr(\vv \leq \Av \xv \leq \uv) = \int_{\vv \leq \Av \xv \leq \uv} \phi(\xv) \diff \xv,
\end{equation}
\\]
where \\(\phi(\xv) \propto \exp(-\frac12 \xv^\top \xv)\\) is the standard normal density.

For the sake of presentation, we assume \\(\Av \in \Rb^{d \times d}\\) is a full rank lower triangular matrix.
The seemingly more general case where \\(\Av \in \Rb^{m \times d}\\) is non-square (but still row full rank) and \\(\xv\\) is non-standard normal can be reduced to \eqref{eq:normal-prob} by a change of variables.

#### **Non-Triangular Square Matrices**
Suppose \\(\Av\\) is square and full rank but not triangular.
Let \\(\Av = \Lv \Qv\\) be its LQ decomposition.
Then, a change of variables \\(\zv = \Qv \xv\\) reduces the problem to \\(\Pr(\vv \leq \Lv \zv \leq \uv)\\), where \\(\zv\\) is standard normal and \\(\Lv\\) is lower triangular.

#### **Non-Square Wide Matrices**
Suppose \\(\Av \in \Rb^{m \times d}\\) is rectangle with \\(m < d\\) and row full rank.
Similar to the previous argument, applying a LQ decomposition and a change of variables reduces the problem to
\\(\Pr(\vv \leq \Lv \zv \leq \uv)\\),
where \\(\Lv \in \Rb^{m \times d}\\) and \\(\Qv \in \Rb^{d \times d}\\).
Note that the last \\((d - m)\\) columns of \\(\Lv\\) are zeros, which implies that the last \\((d - m)\\) coordinates of \\(\zv\\) are irrelevant.
Thus, marginalizing out those coordinates reduces the problem to \eqref{eq:normal-prob}.

#### **Non-Standard Normal**
Suppose \\(\xv \sim \Nc(\muv, \Sigmav)\\) is a non-standard normal random variable.
Let \\(\Lv \Lv^\top = \Sigmav\\) be the Cholesky decomposition.
A change of variables \\(\xv = \Lv \zv + \muv\\) yields \\(\Pr(\vv \leq \Av(\Lv \zv + \muv) \leq \uv)\\), where \\(\zv\\) is standard normal.
Reuse the previous argument completes the reduction.

## Monte Carlo Estimate by Separation of Variables
We will estimate the integral \eqref{eq:normal-prob} by importance sampling.
The basic idea is to construct a different distribution that is easy to sample from and whose support is exactly the domain defined by the inequality constraints.

Since \\(\Av\\) is lower triangular, the linear inequality constraints \\(\vv \leq \Av \xv \leq \uv\\) can be rewritten as
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
We have removed the inequality constraints because samples from \\(p(\xv)\\) automatically satisfy the constraints.

## Discussion
Perhaps surprisingly, this fundamental problem of estimating high dimensional normal probability is still under active research.
The method of separation of variables in this post was developed in the 1990s (Genz, 1992).
Recent developments are still based on this idea, which we briefly mention below.

The method of separation of variables we presented above is based on univariate conditioning: The conditional distribution \eqref{eq:conditional} is a univariate distribution.
Bivariate conditioning (Genz and Trinh, 2016) instead uses bivariate conditional distributions \\(p(x_{2i + 1}, x_{2i + 2} \mid x_{1:2i})\\) for \\(i = 0, 1, \cdots, \frac12 (n - 2)\\).

A natural idea is tweaking the order of the variables in the conditional distributions \eqref{eq:conditional}.
For instance, Genz and Bretz (2009) propose a heuristic to find a permutation of the coordinates that reduces the Monte Carlo error.

Minimax tilting modifies the conditional distribution \eqref{eq:conditional} by an exponential tilting (Botev, 2017).
Very roughly speaking, they replace the standard normal density \\(\phi(x)\\) in the conditional distribution \eqref{eq:conditional} with a shifted normal density \\(\phi(x; \mu, 1)\\), where the shift parameter \\(\mu\\) is chosen carefully.

A careful reader will notice that we have ignored the case when the matrix \\(\Av\\) in \eqref{eq:normal-prob} has more rows than columns, i.e., more constraints than dimensions.
In fact, this is a hard one, which has to rely on Markov chain Monte Carlo methods combined with sophisticated numerical integration methods.
We have intentionally skipped this case.

### **Exercises**
1. Show that separation of variables still works when some entries of \\(\vv\\) and \\(\uv\\) in  \eqref{eq:normal-prob} are infinite.
Hence, the method of separation of variables indeed applies to the normal CDF as well.

1. Show the distribution \eqref{eq:importance-dist} is ***not*** the truncated normal distribution \\(\phi(\xv \mid \vv \leq \Av \xv \leq \uv)\\).

1. What are the advantages of the importance distribution \eqref{eq:importance-dist} compared to the uniform distribution over the polytope \\(\\{\xv \in \Rb^d: \vv \leq \Av \xv \leq \uv\\}\\)?

### **References**
Botev, Z. I. (2017). The normal law under linear restrictions: simulation and estimation via minimax tilting. Journal of the Royal Statistical Society Series B: Statistical Methodology, 79(1), 125-148.

Genz, A. (1992). Numerical computation of multivariate normal probabilities. Journal of computational and graphical statistics, 1(2), 141-149.

Genz, A., & Bretz, F. (2009). Computation of multivariate normal and t probabilities (Vol. 195). Springer Science & Business Media.

Genz, A., & Trinh, G. (2016). Numerical computation of multivariate normal probabilities using bivariate conditioning. In Monte Carlo and Quasi-Monte Carlo Methods: MCQMC, Leuven, Belgium, April 2014 (pp. 289-302). Springer International Publishing.
