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
for some known matrix of predictors \\(X \in \mathbb{R}^{n \times (p+1)} \\)
and some unknown parameter vector \\(\beta\\). We assume that the matrix \\(X\\)
has a column corresponding to the intercept term, along with \\(p\\) actual predictors,
hence the \\(p+1\\) columns of the matrix. This is the standard linear regression
model, and we will later show how the "standard" ANOVA with \\(k\\) groups is a
special case of this. Under this model, the fitted parameter vector \\(\hat{\beta}\\)
which minimizes the squared norm of the vector \\(\epsilon\\)
is

\\[ \hat{\beta} = (X^T X)^{-1} X^T y \\]

This matrix equation is sometimes known as the *normal equations*.

Having found the least--squares parameter vector \\(\beta\\), we can compute
the vector of residuals:

\\[ \hat{\epsilon} = y - X \hat{\beta} = y - X (X^T X)^{-1} X^T y \\]

Assuming that one of the predictors (i.e., one of the columns of \\(X\\)) was a
constant, we know that the mean of \\(\hat{\epsilon}\\) is 0, and hence the
variance of the residuals is precisely \\(\hat{\epsilon}^T \hat{\epsilon}\\).
This is, intuitively, the variance that was unexplained by the regression of
\\(y\\) on \\(X\\). The question we now want to answer is, did that regression
explain a "significant" amount of the variance in \\(y\\)?  That is, is the
remaining variance which is unexplained by the regression (namely
\\(\hat{\epsilon}^T \hat{\epsilon}\\)) small in comparison to the variance of
the observations \\(y\\)?

As usual with significance tests, we will first construct a null hypothesis
which says, essentially, that the regression is useless and does not contributed
anything to our understanding of the output \\(y\\). This hypothesis will
predict, then, that some test statistic of interest has a certain distribution.
If the actual value of the test statistic is "extreme" (i.e., would not
occur under the assumptions of the null hypothesis, except with probability
\\(p\\) less than some desired significance level \\(\alpha\\)), then we reject the null hypothesis
at the \\(\alpha\\) significance level.

So, what are the assumptions of the null hypothesis in this case? Again, we want
to formalize the hypothesis that the regression is useless. So, we assume that
each \\(y_i = \mu + \epsilon_i\\), for some mean \\(\mu\\) and Gaussian noise
\\(\epsilon_i \sim N(0, \sigma^2)\\). Under these assumptions, the variance
of the residuals from the regression will satisfy

\\[ \frac{1}{\sigma^2} \hat{\epsilon}^T \hat{\epsilon} \sim \chi^2_{n - p - 1} \\]

Note that the scaling factor of \\(1 / \sigma^2\\) is unknown; we will deal
with this issue shortly.

Why does this statistic have \\(n - p - 1\\) as opposed to \\(n\\) degrees of
freedom? The answer is a little too involved to explain in this post, but
remember this question because it will be answered in tomorrow's post.
Essentially, we can construct \\(n - p - 1\\) independent,
identically--distributed standard normal variables whose squared sum is equal
to that of the squared residuals (after they are scaled down by a factor of
\\(1 / \sigma\\)).

Similarly, we can show that the difference between the residual sum of squares
and the variance in the initial dataset, i.e.,

$$ \left( \frac{1}{\sigma^2} \left[ \sum\limits_{i=1}^n (y_i - \bar{y})^2 - \hat{\epsilon}^T \hat{\epsilon} \right] \right) \sim \chi^2_{p} $$

This brings us to the definition of the \\(F\\) distribution (named, like most
objects in statistics, after the British statistician R. A. Fisher).  Unlike
most well--known distributions, the \\(F\\) distribution is parametrized by not
one but two degrees of freedom.  \\(F\\) distribution with \\( (d_1, d_2) \\)
degrees of freedom, denoted \\(F_{d_1, d_2}\\), can be characterized as the
distribution of the ratio

$$ \frac{\frac{1}{d_1} \sum\limits_{i=1}^{d_1} U_i^2} {\frac{1}{d_2} \sum\limits_{j=1}^{d_2} V_j^2}, $$

where the \\(U_i, i = 1, \ldots, d_1, V_j, j = 1, \ldots, d_2\\) are i.i.d.
standard normal variables. Equivalently, we can express the distribution as
that of the ratio

\\[ \frac{\frac{1}{d_1} X_1}{\frac{1}{d_2} X_2}, \\]

where \\(X_1\\) and \\(X_2\\) are distributed as \\(\chi^2\\) with \\(d_1\\)
and \\(d_2\\) degrees of freedom respectively.  But these are precisely the
distributions of the two statistics from above! Moreover, when we take their
ratio, the unknown scale factor of \\(1 / \sigma^2\\) cancels out from both the
numerator and the denominator, so we are left with the following fact: under
the assumptions of the null hypothesis,

$$ \frac{\left( \sum\limits_{i=1}^n (y_i - \bar{y})^2 - \hat{\epsilon}^T \hat{\epsilon} \right)}{\hat{\epsilon}^T \hat{\epsilon}} \sim F_{p, n - p - 1}. $$

All that remains to do is compute the value of the test statistic using the
variance of our dataset and the sum of squared residuals from our regression
and find its quantile under the \\(F_{p, n - p - 1}\\) distribution. If the
probability of a value drawn from the \\(F\\) distribution being larger than
the \\(F\\) statistic is smaller than some threshold \\(\alpha\\), then we can
reject the null hypothesis at the \\(\alpha\\) significance level.

So we have explained how ANOVA works in the single--variable case. What about
in the multivariate case (MANOVA)? We will answer this, and some other questions
that came up in this post, next time.
