---
layout: post
title:  Maximum likelihood estimation of normal distribution
categories: [notes]
tags: [statistics, home work]
---

The probability density function of normal distribution is:
`\[
f(x)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^{2}}{2\sigma^{2}}}
\]`


Support we have the following *n i.i.d* observations: `\(x_{1},x_{2},\dots,x_{n}\)`.
Because they are independent, the probability that we have observed
these data are:
`\[
f(x_{1},x_{2},\dots,x_{n}|\sigma,\mu)=\prod_{i=1}^{n}\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x_{i}-\mu)^{2}}{2\sigma^{2}}}=(\frac{1}{\sigma\sqrt{2\pi}})^{n}e^{-\frac{1}{2\sigma^{2}}\sum_{i=1}^{n}(x_{i}-\mu)^{2}}
\]`


`\[\begin{array}{cl}
\log(f(x_{1},x_{2},\dots,x_{n}|\sigma,\mu)) & =\log((\frac{1}{\sigma\sqrt{2\pi}})^{n}e^{-\frac{1}{2\sigma^{2}}\sum_{i=1}^{n}(x_{i}-\mu)^{2}})\\
 & =n\log\frac{1}{\sigma\sqrt{2\pi}}-\frac{1}{2\sigma^{2}}\sum_{i=1}^{n}(x_{i}-\mu)^{2}\\
 & =-\frac{n}{2}\log(2\pi)-n\log\sigma-\frac{1}{2\sigma^{2}}\sum_{i=1}^{n}(x_{i}-\mu)^{2}
\end{array}\]`

Let's call `\(\log(f(x_{1},x_{2},\dots,x_{n}|\sigma,\mu))\)` as `\(\mathcal{L},\)`
then let:
`\[
\frac{d\mathcal{L}}{d\mu}=-\frac{1}{2\sigma^{2}}\sum_{i=1}^{n}(x_{i}-\mu)^{2}\mid_{\mu}=0
\]`
 solve this equation, we get 
`\[
\frac{1}{2\sigma^{2}}\sum_{i=1}^{n}(2\hat{\mu}-2x_{i})=0
\]`

Because `\(\sigma^{2}\)` should be larger than zero,
`\[
\hat{\mu}=\frac{\sum_{i=1}^{n}x_{i}}{n}
\]`


Similarly, let
`\[
\frac{d\mathcal{L}}{d\sigma}=-\frac{n}{\sigma}+\sum_{i=1}^{n}(x_{i}-\mu)^{2}\sigma^{-3}=0
\]`


I realized that it would be better to get the maximum likelihood estimator
of `\(\sigma^{2}\)` instead of `\(\sigma\)`. Thus

`\[
\hat{\sigma}^{2}=\frac{\sum_{i=1}^{n}(x_{i}-\hat{\mu})^{2}}{n}
\]`
