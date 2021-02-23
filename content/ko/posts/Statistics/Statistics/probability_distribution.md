---
collapsible: false
date: "2021-02-21T10:08:56+09:00"
description: null
draft: false
title: 확률분포
weight: 1
---

# 확률분포(Probability Distribution) 
![확률분포 관계도](images/posts/statistics/prob_dist_relation.png)
<div style="text-align: center"> 출처: https://artificialnetworkforstarters.readthedocs.io/en/latest/_post/chap6.html </div>

### 연속형
- 정규 분포
- T-분포
- 감마 분포
- 베타 분포
- 카이제곱 분포
- F-분포
- 균일 분포

### 이산형
- 이항 분포
- 베르누이 분포
- 포아송 분포
- 기하 분포
- 음이항 분포
- 초기하 분포

---

#### 정규 분포(Normal Distribution) 
$$ \text{X~} N(\mu, \sigma^2) \rightarrow f(x) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp(-\frac{(x-\mu)}{2\sigma^2}^2) $$

$$ E(X) = \mu, Var(X) = \sigma^2$$


#### 다변수 정규분포(Multivariate Normal Distribution)

[관련 포스팅](https://jiwooblog.netlify.app/posts/statistics/statistics/mvn/) 참고

#### T-분포(Student's t-Distribution)
$$ \text{X~} t(n) \rightarrow f(x) = \frac{\Gamma(\frac{n+1}{2})}{\Gamma(\frac{n}{2})\cdot\sqrt{\pi n}}x $$

$$ T = \frac{Z}{\sqrt{\frac{V}{\nu}}} \text{~ } t(df)$$
where `$Z\text{~ }N(0,1), V\text{~ } \chi^2(\nu)$`

`$ $`

#### 이항 분포(Binomial Distribution)

#### 감마 분포(Gamma Distribution)
$$ \text{X~} Gamma(\alpha, \beta) \rightarrow f(x) = \frac{1}{\beta^\alpha\cdot\Gamma(\alpha)}x^{\alpha-1}e^{-\frac{x}{\beta}} $$
`$\text{for } x>0, \ \alpha>0, \ \beta>0 $`
$$ E(X)=\alpha\beta, \ Var(X)=\alpha\beta^2 $$

#### 베타 분포(Beta Distribution)

