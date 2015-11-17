---
layout: post
title:  "Projected Development"
date:   2015-11-06 21:49:44 -0800
categories: math
---

Today's post will diverge somewhat from the expected (heh) topic,
which was more about MANOVA. Instead, we will discuss some of the
linear algebraic foundations that we use to discuss linear regression
in higher--dimensional spaces. This post will assume familiarity
with the basic linear algebraic concepts of linear independence
and span, as well as with the derivation of the normal equations
and the SVD.

In particular, we will walk through the definition of projection
and a useful formula for computing the projection of a vector
onto a set of other vectors.

First, what is the definition of projection? There are a few different
ways to define it, but the geometrically most appealing way is as follows:
the projection of a vector \\(y\\) onto a collection of vectors
\\( C = \left{ X^{(i)} ~|~ i = 1, \ldots, n \right}\\), is the vector
\\(\hat{y}\\) which:

1. Lies in the span of \\( C \\), and
2. Is the closest vector to \\(y\\), in the \\(\ell_2\\) sense, among all vectors in the span of \\( C \\).

In other words, to project \\( y \\) onto a collection of vectors \\( C \\) is
to find a linear combination of the vectors in \\( C \\) that gets "as close as
possible" (minimizes the Euclidean distance) to \\( y \\).

Moreover, observe that it suffices to assume that \\( C \\) is linearly
independent.  This is because the projection of a vector into \\( C \\) depends
only on the span of \\( C \\): if \\( C \\) is not linearly independent, then
there exists some strict subset of \\( C \\) (call it \\(C'\\)) whose span is
the same as that of \\( C \\).  Then projecting a vector into \\( C \\) is the
same as projecting it into \\( C' \\).

<div class="lemma" markdown="1"> Suppose that \\( C \subseteq \mathbb{R}^n \\)
is a linearly independent set of \\(p\\) vectors.

Let \\( \mathbf{X} \in \mathbb{R}^{n \times p} \\) be the matrix whose columns
are given by the vectors in \\( C \\).

Then the projection of a vector \\( y \\) onto the vectors of \\(C\\), denoted
\\( \hat{y} \\), is given by

$$ \hat{y} = \mathbf{X} ( \mathbf{X}^T \mathbf{X} )^{-1} \mathbf{X}^T y.$$

</div>

<div class="proof" markdown="1">
Following the definition of the projection as given above,
we can express \\( \hat{y} \\) as the solution to the following optimization problem:

<p style="text-align:center">
    minimize `\|y - u\|_2`
    subject to `u \in \mathrm{col}(\mathbf{X})`,
</p>

where for any matrix \\(A\\), \\( \mathrm{col}(A) \\) denotes the column space of \\(A\\)
(i.e., the span of the columns of \\(A\\)).

But we can reformulate this problem slightly using two facts: firstly,
minimizing a Euclidean distance is the same as minimizing the square of the
Euclidean distance (since Euclidean distance is always nonnegative and squaring
is a monotonic function on the nonnegative reals). Secondly, the constraint
that \\(u\\) lie in the column space of \\(\mathbf{X}\\) just means that
\\(u\\) must be a linear combination of the columns of \\(\mathbf{X}\\), which
in turn means that \\(u = \mathbf{X} v\\) for some vector
\\(v \in \mathbb{R}^p\\). Thus, we can change variables from \\(u\\) to \\(v\\)
and express the problem as:

<p style="text-align:center">
    minimize over `v \in \mathbb{R}^p`: `\|y - \mathbf{X} v\|^2_2`
</p>

If we do this, then the projection of \\( y \\) into the column space of
\\( \mathbf{X} \\) will be \\( \mathbf{X} \hat{\beta} \\), where \\( \hat{\beta} \\)
is the solution to the above, reformulated optimization problem.

But this is simply our old friend linear regression by another name!
The vector \\( v \\) which achieves this minimum is, in the language of linear regression,
the least--squares estimate \\( \hat{\beta} \\), and is given by

$$ \hat{\beta} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T y $$.

This vector \\( \hat{\beta} \in \mathbb{R}^p \\) tells us the coefficients of the
linear combination of the columns of \\( \mathbf{X} \\) needed to recover
the projection of \\( y \\) into the column space of \\( \mathbf{X} \\).
That is,

$$ \hat{y} = \mathbf{X} v = \mathbf{X} (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T y $$.

</div>
