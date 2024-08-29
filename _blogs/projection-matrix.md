---
layout: base
title: Projection Matrix
---
# {{ page.title }}

Given a dataset \\((\\Xv, \yv)\\) where \\(\Xv \in \Rb^{n \times d}\\) and \\(\yv \in \Rb^d\\), the optimal weight of (ordinary) linear regression is \\(\wv^* = \big(\Xv^\top \Xv\big)\inv \Xv^\top \yv\\).
The prediction on the training set is a linear transformation of the training labels:
\\[
    \hat\yv = \Pv \yv = \Xv \big(\Xv^\top \Xv\big)\inv \Xv^\top \yv,
\\]
where we call \\(\Pv \in \Rb^{n \times n}\\) the projection matrix.

The projection matrix \\(\Pv \in \Rb^{n \times n}\\) has many interesting properties.

connections with projection matrices and pseudo inverse
