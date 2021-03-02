---
collapsible: false
date: "2021-01-07T10:08:56+09:00"
description: One-parameter Models
draft: false
title: Conjugacy
weight: 3
---

## Chapter 03. <br> One-parameter Models
본 포스팅은 **First Course in Bayesian Statistical Methods**를 참고하였다.

### Binomial Model
Prior: `$\theta \text{ ~ } beta(a,b)$`
Likelihood: `$Y|\theta \text{ ~ } binomial(n, \theta) $`
Posterior: `$\theta|y \text{ ~ } beta(a+y, b+n-y) $`  

`$ E[\theta|y] = \frac{a+y}{a+b+n} = \frac{n}{a+b+n}\times\frac{y}{n} + \frac{a+b}{a+b+n}\times\frac{a}{a+b} $` where `$\frac{y}{n}$` = sample mean, `$\frac{a}{a+b}$` = prior expectation

### Poisson Model
Prior: `$\theta \text{ ~ } gamma(a,b) $`
Likelihood: `$Y_1, ..., Y_n \text{ ~ iid. } Poisson(\theta)$`
Posterior: `$\theta|Y_1, ..., Y_n \text{ ~ } gamma(a+\sum_{i=1}^{n}{Y_i}, b+n) $`

### Exponential Family
exponential family(지수족)의 pdf 또는 pmf는 다음과 같은 형식으로 표현될 수 있어야 한다. `$ p(y_i|\theta) = f(y_i) \ g(\theta) \ exp(\phi(\theta)^Ts(y_i)) $`

Prior: `$ p(\theta) \propto g(\theta)^\eta \ exp(\phi(\theta)^T \ \nu) $`
Likelihood: `$ p(y|\theta) = \prod_{i=1}^{N} f(y_i) \ g(\theta)^N \ exp(\phi(\theta)^T \ \sum_{i=1}^{N}s(y_i)) $` where `$\sum_{i=1}^{N}s(y_i))$` is sufficient statistics `$t(y)$`
Posterior: `$ p(\theta|y) \propto g(\theta)^{\eta+N} \ exp(\phi(\theta)^T \ (\nu + t(y)) $`

### Conjugate Prior
prior와 posterior의 확률분포형태가 같을 수 있도록 prior을 설정하면 이를 conjugate prior라고 한다.  
위의 예시 외에도 Normal model 등이 있는데, 이들에 대해서는 다음에 이어서 살펴보도록 하겠다.

### Conclusion
<p style='text-align: center'> Conjugacy를 잘 알아두자. </p>

---
<br> 
<p style='text-align: center; color:gray'> 혹시 궁금한 점이나 잘못된 내용이 있다면, 댓글로 알려주시면 적극 반영하도록 하겠습니다. </p>

<br>
<br>