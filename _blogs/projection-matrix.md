---
layout: base
title: Projection Matrix
---
# {{ page.title }}

Consider a linear regression problem with a given dataset \\((\\Xv, \yv)\\), where \\(\Xv \in \Rb^{n \times d}\\) and \\(\yv \in \Rb^n\\).
The optimal weight of linear regression is \\(\wv^* = \big(\Xv^\top \Xv\big)\inv \Xv^\top \yv\\).
It is easy to see that the prediction on the training set is a linear transformation of the training labels:
\\[
    \hat\yv = \Xv \wv^* = \Xv \big(\Xv^\top \Xv\big)\inv \Xv^\top \yv,
\\]
where we call \\(\Pv = \Xv \big(\Xv^\top \Xv\big)\inv \Xv^\top \in \Rb^{n \times n}\\) the projection matrix.

The projection matrix \\(\Pv \in \Rb^{n \times n}\\) has the following properties (taken directly from [Wikipedia](https://en.wikipedia.org/wiki/Projection_matrix#Properties)).
1. The residual \\(\rv = \yv - \hat\yv\\) is orthogonal to the column space of \\(\Xv\\).
1. \\(\Pv\\) is idempotent, i.e., \\(\Pv^2 = \Pv\\).
1. The eigenvalues of \\(\Pv\\) are either zeros or ones.
1. \\(\Xv\\) is invariant under \\(\Pv\\), i.e., \\(\Pv \Xv = \Xv\\).

All of the above can be proved by brute-force calculation.
But I prefer the following argument, which explains where the name "projection" comes from.

It's not hard to see that \\(\hat \yv\\) is a Euclidean projection of \\(\yv\\) onto the column space of \\(\Xv\\):
\\[
    \hat \yv = \argmin_{\zv \in \operatorname{range}(\Xv)} \lVert \zv - \yv \rVert,
\\]
which implies that \\(\Pv\\) implements this projection operator.
By the first-order optimality condition, we immediately have \\((\hat \yv - \yv) \perp \operatorname{range}(\Xv)\\), which proves (1).
For an arbitrary \\(\yv \in \Rb^d\\), applying the projection more than once is the same as applying the projection once, which proves (2).
The eigenvalues of \\(\Pv\\) are immediately available due to idempotence.
The last statement (4) is immediate because each column of \\(\Xv\\) is trivially inside \\(\operatorname{range}(\Xv)\\) and thus is invariant under \\(\Pv\\).

<!-- connections with projection matrices and pseudo inverse -->

### **Exercises**
1. Let \\(V \subseteq \Hc\\) be a closed linear subspace of a Hilbert space.
Show that the projection operator onto \\(V\\) is a linear function, where the projection operator is defined as
\\[
    \xv \mapsto \argmin_{\zv \in V} \; \lVert \zv - \xv \rVert_\Hc.
\\]
