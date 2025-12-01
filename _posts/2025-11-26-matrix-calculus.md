---
layout: base
title: Matrix Calculus
---
# {{ page.title }}

I have been doing matrix calculus recently and I thought it's quite interesting.

## Basic Setup
Let \\(f(\Xv) = \Av \Xv \Bv\\), where \\(\Av, \Xv, \Bv\\) are matrices with appropriate sizes.

The following two identities are extremely useful and worth remembering.
\\[
    \langle \Av, \Bv \Cv \rangle = \langle \Av \Cv^\top, \Bv \rangle
\\]
\\[
    \langle \Av, \Bv \Cv \rangle = \langle \Bv^\top \Av, \Cv \rangle
\\]
The proof is trivial by invoking the cyclic property of matrix traces.

## Differentiate Through Inverse
TBD

## Differentiate Through Cholesky Decomposition
This section largely follows the note by Murray (2008).

$$
\begin{aligned}
\diff f
&=
\langle \bar{\Lv}, \diff \Lv \rangle \\\\
&=
\Big\langle \bar{\Lv}, \Lv \Phi\big(\Lv\inv \diff \Sigmav \Lv^{-\top}\big) \Big\rangle \\\\
& =
\Big\langle L^\top \dot L, \Phi\big(L\inv \diff \Sigma L^{-\top}\big) \Big\rangle
\\\\
& =
\Big\langle \Phi\big(L^\top \dot L\big), L\inv \diff \Sigma L^{-\top} \Big\rangle
\\\\
& =
\Big\langle L^{-\top} \Phi\big(L^\top \dot L\big) L\inv, \diff \Sigma \Big\rangle
\end{aligned}
$$

Thus, we obtain \\(\dot\Sigma = L^{-\top} \Phi\big(L^\top \dot L\big) L\inv\\).

## Closing Remarks
The matrix cookbook by Petersen & Pedersen (2008) is always a good reference when it comes to linear algebra.
But it does not cover enough material for matrix calculus.
The MIT IAP course (Matrix Calculus for Machine Learning and Beyond) by Edelman and Johnson is an exellence resource for matrix calculus and automatic differentiation.

## References
1. Murray, I. (2016). Differentiation of the Cholesky decomposition. arXiv preprint arXiv:1602.07527.