---
layout: base
title: Owen's T Function
---
# {{ page.title }}

In this post, we introduce Owen's T function
\\[
    T(h, a) = \frac{1}{2\pi} \int_{0}^{a} \frac{\exp(-\frac12 h^2 (1 + x^2))}{1 + x^2} \diff x.
\\]
Owen's T function arises naturally from bivariate normal probabilities.
Indeed, in his original paper, Owen BLAH BLAH.

## Basic Properties


## Numerical Estimate


## Derivatives
The derivatives of Owen's T function are available in closed-form.
\\[
    \frac{\partial}{\partial h} T(h, a) = \frac{1}{2\pi} \int_{0}^{a} \exp\bigg(-\frac12 h^2 (1 + x^2)\bigg) \cdot (-h) \diff x = BLAH.
\\]
and
\\[
    \frac{\partial}{\partial a} T(h, a) = \frac{1}{2\pi} \cdot \frac{\exp(-\frac12 h^2 (1 + a^2))}{1 + a^2}
\\]
