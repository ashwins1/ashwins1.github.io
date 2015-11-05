---
layout: post
title:  "MANOVA Board"
date:   2015-11-06 21:49:44 -0800
categories: math
---

Today's post will explain the statistical theory behind MANOVA (multivariate
analysis of variance). We will begin with the more familiar univariate method
called ANOVA and its attendant statistical test, the F-test. Then we will
generalize our findings to the multivariate case; in particular, we will
find out how to determine the significance of the decomposition of variance
in the multivariate case.

First, let us recap single--variable ANOVA. Given a vector of observations
\\(y \in \mathbb{R}^n\\), we often want to know whether a particular regression
is "significant", i.e., explains the data to a degree greater than would be
expected by chance.

Concretely, suppose that we have a particular model \\(y = X \beta + \epsilon\\),
for some known matrix of predictors \\(X\\), some given observation vector \\(y\\),
and some unknown parameter vector \\(\beta\\). This is the standard linear regression
model, and we will later show how the "standard" ANOVA with \\(k\\) groups is a
special case of this. Under this model, the fitted parameter vector \\(\hat{\beta}\\)
which minimizes the squared norm of the vector \\(\epsilon\\)
is $$\hat{\beta} = (X^T X)^{-1} X^T y$$

This matrix equation is sometimes known as the *normal equations*.

Having found the least--squares parameter vector \\(\beta\\), we can compute
the vector of residuals: $$\hat{\epsilon} = y - X \hat{\beta} = y - X (X^T
X)^{-1} X^T y$$.  Assuming that one of the predictors (i.e., one of the columns
of \\(X\\)) was a constant, we know that the mean of \\(\hat{\epsilon}\\) is 0,
and hence the variance of the residuals is precisely \\(\hat{\epsilon}^T
\hat{\epsilon}\\). This is, intuitively, the variance that was unexplained by
the regression of \\(y\\) on \\(X\\). The question we now want to answer is,
did that regression explain a "significant" amount of the variance in \\(y\\)?
That is, is the remaining variance which is unexplained by the regression
(namely \\(\hat{\epsilon}^T \hat{\epsilon}\\)) small in comparison to the
variance of the observations \\(y\\)?
