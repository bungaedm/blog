---
collapsible: false
date: "2021-01-07T10:08:56+09:00"
description: Introduction and Examples
draft: false
title: What is Bayesian
weight: 1
---

## Chapter 01. <br> Introduction and Examples
본 포스팅은 **First Course in Bayesian Statistical Methods**를 참고하였다.
이번 장을 통해서는 Likelihood and Prior를 살펴보고 Full probability model의 의미를 보는 데에 주목해보쟈.

### 베이지안 추론의 목적
우리는 데이터 획득을 통해, 모집단 특성에 대한 불확실성을 줄여나가고자 한다. 이때, 불확실성 정도의 변화 수준을 계량화하는 것이 베이지안 추론통계의 목적이라고 할 수 있다.

### 핵심 개념
1. prior distribution `$p(\theta)$`
    - 사전확률
    - 모수에 대해 기존에 갖고 있던 믿음의 정도
2. sampling model `$p(y|\theta)$` 
    - 일종의 가능도 함수(likelihood)
    - 사전확률이 참이라는 가정 하에, 특정 데이터가 관찰된 확률
3. posterior distribution `$p(\theta|y)$`
    - 데이터가 관찰되었을 때, 이를 바탕으로 수정된 모수에 대한 믿음의 정도
    
### Bayes' Rule
$$p(\theta|y) = \frac{p(y|\theta)p(\theta)}{\int_{\Theta}p(y|\tilde{\theta})p(\tilde{\theta})d\tilde{\theta}}$$

이는 사후분포가 사전분포와 가능도 함수에 의해 어떻게 업데이트 되는지를 수식적으로 나타난 것이다.
베이즈 통계의 전부라고 해도 무방하다.

### 활용예시
1. 희소사건 확률 추정(Estimation)
    - 감염 확률(infectious probability)
    - 확률론자(frequentist)는 sample이 적을 때 확률추정을 합리적으로 하는 데에 있어서 취약할 수 있다. 예를 들어, 20명만을 대상으로 감염 여부를 확인하고 감염 확률을 추론한다면, 감염확률을 0%라고 제안하는 것은 통계적으로는 그럴 듯하게 계산될 수 있다. 하지만 이는 현실과는 다소 거리가 있을 수 있다.
    - 이에 반해, 베이지안은 감염 확률을 분포로서 제시할 뿐더러 기존의 믿음을 사전확률로서 제안하기 때문에 이러한 부분에 있어서 덜 취약할 수 있다.
2. 예측 모델 구축(Prediction)
    - 당뇨병(diabetes progression)
    - 50% 확률로 변수의 coefficient가 0라고 사전확률을 제안한다면, 변수선택의 효과를 얻을 수 있다.
    - 이와 관련된 자세한 내용은 [FCB chapter 09](/posts/statistics/bayesian/fcb09/)서 Bayesian Linear Regression과 관련하여 설명될 예정이다.

### ETC
- 'Adjusted' Wald interval
흔히 알려진 신뢰구간을 베이지안적으로 바꾼 형태이다. 
<p style='text-align: center'>`\hat{\theta} \pm 1.96\sqrt{\hat{\theta}(1-\hat{\theta})//n}` , where </p>
<p style='text-align: center'>`\hat{\theta} = \frac{n}{n+4}\bar{y} + \frac{4}{n+4}\frac{1}{2}`</p>

- Lasso
변수 선택의 한 방법이다. 아래 제시된 SSR를 최소화하는 것을 목표로 한다. 베이지안의 맥락에서 처음 연구된 방법론은 아니지만, 특정 사전확률을 적용한다면 베이지안의 관점과 일치한다.
$$SSR(\beta:\lambda) = \sum_{i=1}^{n}(y_i-\boldsymbol{x_i}^T\boldsymbol{\beta})^2 + \lambda\sum_{j=1}^{n}|\beta_j|$$ 



### Conclusion
<p style='text-align: center'> "All models are wrong, but some are useful" </p>
<p style='text-align: center'> - Box and Draper, 1987 </p> <br>

---
<br> 
<p style='text-align: center; color:gray'> 혹시 궁금한 점이나 잘못된 내용이 있다면, 댓글로 알려주시면 적극 반영하도록 하겠습니다. </p>

<br>
<br>