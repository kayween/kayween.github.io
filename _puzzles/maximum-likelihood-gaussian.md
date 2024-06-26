---
layout: base
title: "Gaussian Maximum Likelihood: When the Estimate Does Not Exist"
---
# {{ page.title }}

In undergraduate statistics classes, we learned that the empirical mean \\( \hat\muv = \frac1n \sum_{i=1}^{n} \xv_i \\) and the empirical covariance \\( \widehat\Sigmav = \frac{1}{n} \sum_{i=1}^{n} (\xv_i - \hat\muv) (\xv_i - \hat \muv)^\top \\) are the maximum likelihood estimates for Gaussian distributions \\( \Nc(\muv, \Sigmav) \\),
because they "minimize" the negative log likelihood
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

1. Show that \\( \ell \\) is unbounded below when the empirical covariance \\( \widehat\Sigmav \\) is singular, in which case the maximum likelihood estimate does not exist.

1. Show that \\( \ell \\) is non-convex.
Though, a change of variable \\( \mv = \Sigmav\inv \muv \\) and \\( \Sv = \Sigmav\inv \\) makes it jointly convex in \\( \mv \\) and \\( \Sv \\):
\\[
\ell(\mv, \Sv) =
\frac1n \sum_{i=1}^{n} \Big(
    \xv_i^\top \Sv \xv_i -
    2 \xv_i^\top \mv +
    \mv^\top \Sv\inv \mv -
    \log\det \Sv
\Big).
\\]
Show that the maximum likelihood estimate exists if and only if \\(\widehat\Sigmav\\) is nonsingular.[^convexity]

1. Consider the solution path of the regularized problem
\\[
    \muv^\*(\gamma), \Sigmav^\*(\gamma) = \argmin_{\muv, \Sigmav} \ell(\muv, \Sigmav) + \gamma \big\lVert \Sigmav\inv \big\rVert_*,
\\]
where \\( \lVert \cdot \rVert_\* \\) is the nuclear norm and \\(\gamma > 0\\).
Show that the limits
\\[
    \lim_{\gamma \to 0} \muv^\*(\gamma) = \hat\muv
    \quad \text{and} \quad
    \lim_{\gamma \to 0} \Sigmav^\*(\gamma) = \widehat\Sigmav
\\]
are precisely the empirical mean and the empirical covariance, regardless of the singularity of \\( \widehat \Sigmav \\).

1. Redo Q3 with the squared Frobebius norm \\(\lVert \cdot \rVert_\F^2\\).
Does it change anything?
<!-- What about other norms? -->

Maximum likelihood is arguably the most popular method for parameter estimation in machine learning.
However, as we have just seen, the maximum likelihood estimate does not always exist.
Another well-known example of non-existence in machine learning is training binary logistic regression on linearly separable datasets.
In these cases, regularization is often needed to ensure the existence (and uniqueness) of the maximum likelihood estimate.
Taking a broader perspective, keep in mind that there are numerous parameter estimation methods beyond maximum likelihood, e.g., the method of moments and maximum entropy estimation (cool stuff that uses convex optimization).

#### **Notes**
[^convexity]: The convexity of the negative log likelihood is not by accident. Maximum likelihood estimation reduces to a convex optimization problem for all exponential family distributions, which include Gaussian distributions.
