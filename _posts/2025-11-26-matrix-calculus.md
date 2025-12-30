---
layout: base
title: Matrix Calculus
---

# {{ page.title }}

This post is concerned with computing the derivatives of matrix functions with a particular focus on reverse-mode automatic differentiation (AD).

To set up the stage, let us consider the following computational graph
\\[
    x \to y \leadsto \cdots \leadsto f,
\\]
which starts from \\(x\\), computes \\(y\\), and then eventually arrives at the final output \\(f(x)\\).
We assume the output \\(f(x)\\) is a scalar (which is ubiquitous in machine learning, e.g., the training loss of deep neural networks).
We are interested in computing the derivative
\\[
    \bar x = \frac{\partial}{\partial x} f
\\]
by reverse mode AD (a.k.a. the backward pass in deep learning).
That is, we wish to construct the above derivative using \\(\bar y = \frac{\partial}{\partial y} f\\).

For the time being, let us assume both \\(x\\) and \\(y\\) are vectors.
Let \\(J(x)\\) be the Jacobian matrix of \\(y\\) with respect to \\(x\\).
Then, we can write
\\[
    \diff y = J(x) \diff x.
\\]
It is not hard to see that \\(\bar x\\) and \\(\bar y\\) are related via the Jacobian:
\\[
    \bar x = J(x)^\top \bar y.
\\]
Despite its simplicity, the above process reveals two key steps for reverse mode AD:
1. Calculate the Jacobian (derivative) of \\(y\\) with respect to \\(x\\);
1. Take the transpose (adjoint) of the Jacobian and apply it to \\(\bar y\\).

In what follows, we will use the same two steps to derive reverse mode AD for matrix operations, where both \\(x\\) and \\(y\\) become matrices.
What makes it challenging is that the derivative of \\(y\\) with respect to \\(x\\) cannot be easily written as a Jacobian matrix anymore; this derivative becomes a linear operator.
As a result, we will have to take the adjoint of the linear operator, which is often more complicated than simply taking the transpose of a Jacobian.

## Warm Up: Matrix Multiplication

Let \\(Y = A X B\\), where \\(A, X, B\\) are matrices with appropriate sizes.
Differentiating both sides of the equation gives the derivative:
\\[
    \diff Y = A \diff X B.
\\]
Unlike the vector setting, we cannot write the derivative in a simple matrix form.
(Technically, we still can using Kronecker products. But it's strongly discouraged.)
This derivative is best viewed as a linear operator applied to \\(\diff X\\).

Next, we need to take the adjoint (transpose) of the derivative.
Some simple calculation yields
\\[
\diff f
=
\langle \bar Y, \diff Y \rangle
=
\langle \bar Y, A \diff X B \rangle
=
\langle A^\top \bar Y B^\top, \diff X \rangle,
\\]
which implies that \\(\bar X = A^\top \bar Y B^\top\\).

## Inverse Matrix Multiplication

Let \\(Y = A X\inv B\\).
Differentiating both sides of the equation gives
\\[
\diff Y
=
A \diff \big(X\inv\big) B
= - A X\inv \diff X X\inv B,
\\]
where the second equality is from the matrix cookbook (Petersen & Pedersen 2008; Eq (59)).
Next, taking the adjoint of the derivative yields
\\[
\diff f
=
\langle \bar{Y}, \diff Y \rangle
=
-\langle \bar{Y}, A X\inv \diff X X\inv B \rangle
=
-\langle X^{-\top} A^\top \bar{Y} B^\top X^{-\top}, \diff X \rangle.
\\]
The last expression implies that the backward pass is
\\[
    \bar{X} = - X^{-\top} A^\top \bar{Y} B^\top X^{-\top}.
\\]

## Cholesky Decomposition

Let \\(L L^\top = \Sigma\\) be the Cholesky decomposition of a symmetric positive semi-definite matrix \\(\Sigma\\).
Now we compute the derivative
\\(
    \bar \Sigma = \frac{\partial}{\partial \Sigma} f
\\)
by reverse mode AD.

Differentiating both sides of the equation \\(\Sigma = L L^\top\\) gives
\\[
    \diff \Sigma = \diff L L^\top + L \diff L^\top,
\\]
where we emphasize that \\(\diff \Sigma\\) is symmetric and \\(\diff L\\) is lower triangular.
To get the derivative, we need to express \\(\diff L\\) in terms of \\(\diff \Sigma\\), which requires some algebra.
We skip the step-by-step derivation here and directly give the final identity by Murray (2016, Section 3):
\\[
\begin{equation}
\label{eq:cholesky_derivative}
    \diff L = L \Phi\big(L\inv \diff \Sigma L^{-\top}\big),
\end{equation}
\\]
where \\(\Phi\\) is an element-wise function that zeros out the upper triangular part and halves the diagonal entries, i.e., \\(\Phi(Z) = \operatorname{tril}(Z) - \frac12 \operatorname{diag}(Z)\\) for any symmetric \\(Z\\).

Next, we need to take the adjoint (i.e., transpose) of the derivative in \eqref{eq:cholesky_derivative}.
Recall that the derivative \eqref{eq:cholesky_derivative} is a linear operator that maps from the space of symmetric matrices to the space of lower triangular matrices.
Hence, its adjoint is a linear operator that takes a lower triangular matrix and returns a symmetric matrix.

Some algebra gives
\\[
\diff f
=
\langle \bar{L}, \diff L \rangle
=
\Big\langle \bar{L}, L \Phi\big(L\inv \diff \Sigma L^{-\top}\big) \Big\rangle
=
\Big\langle \operatorname{tril}\big(L^\top \bar{L}\big), \Phi\big(L\inv \diff \Sigma L^{-\top}\big) \Big\rangle,
\\]
where we stress that the linear function \\(\operatorname{tril}(\cdot)\\) taking the lower triangular part (including the diagonal) cannot be omitted because the above inner product is defined in the space lower triangular matrices.

Let \\(\Phi^*\\) be the adjoint of the linear function \\(\Phi\\).
It is not hard to see that
\\[
    \Phi^ *(Z) = \frac12\big(Z + Z^\top - \operatorname{diag}(Z)\big)
\\]
for any lower triangular \\(Z\\).
The remaining is straightforward calculation:
\\[
\diff f
=
\Big\langle \Phi^ *\big(\operatorname{tril}\big(L^\top \bar{L}\big)\big), L\inv \diff \Sigma L^{-\top} \Big\rangle
=
\Big\langle L^{-\top} \Phi^ *\big(\operatorname{tril}\big(L^\top \bar{L}\big)\big) L\inv, \diff \Sigma \Big\rangle,
\\]
where we emphasize that both inner products are defined in the space of symmetric matrices.
From the last expression, we obtain \\(\bar\Sigma = L^{-\top} \Phi^ *\big(\operatorname{tril}\big(L^\top \bar{L}\big)\big) L\inv\\).
This expression is [what's implemented in PyTorch](https://github.com/pytorch/pytorch/blob/4a0693682a8574bdc36e1ca2ea7bd2ddf5c19340/torch/csrc/autograd/FunctionsManual.cpp#L1984-L2010), different from the one in Murray (2016, Section 3).

## Final Remarks

The matrix cookbook by Petersen & Pedersen (2008) is always a good reference when it comes to linear algebra.
But it does not cover enough material for matrix calculus.
The MIT IAP course (Matrix Calculus for Machine Learning and Beyond) by Edelman and Johnson is an excellent resource for matrix calculus and automatic differentiation.

I always feel reverse mode AD is more complicated to implement compared to forward mode AD.
One of the reasons is that we need to take the adjoint of the derivative operator in reverse mode, which is not always straightforward when the derivative is complicated.

Throughout this post, we have used \\(\bar X\\) to denote the derivative of the final output w.r.t. \\(X\\).
The usual derivative notation \\(\dot X\\) is typically reserved for the derivative of \\(X\\) w.r.t. the very first input, which is commonly used in forward mode AD.

These two identities are worth remembering:
\\(
    \langle \Av, \Bv \Cv \rangle = \langle \Av \Cv^\top, \Bv \rangle
\\)
and
\\(
    \langle \Av, \Bv \Cv \rangle = \langle \Bv^\top \Av, \Cv \rangle
\\),
which arise frequently when doing matrix calculus.

## References

1. Murray, I. (2016). Differentiation of the Cholesky decomposition. arXiv preprint arXiv:1602.07527.

