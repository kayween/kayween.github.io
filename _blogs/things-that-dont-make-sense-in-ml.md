---
layout: base
title: Things That Don't Make Sense in Machine Learning
---
# {{ page.title }}

More than once, I have seen papers confusing "subject to" with "such that".
Here is an example
> We formulate the following optimization problem
\\[
    \min_{\xv} f(\xv) \;\;\text{such that}\;\; g(\xv) = 0,
\\]
whose optimal solution can be found by ...

There are three issues with the above writing.
1. It's illegal to use "\\(\min\\)" defining an optimization problem.
Should use "\\(\mini\\)" instead.
2. It's illegal to use "such that" defining an optimization problem.
Should use "subject to" instead.
3. It makes no sense to say "optimal solution"---the word "solution" already implies it's optimal.

Try to solve a minimization problem by taking derivative, but do not verify convexity.

Lagrange multiplier
