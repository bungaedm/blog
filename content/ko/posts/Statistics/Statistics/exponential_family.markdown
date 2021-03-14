---
collapsible: false
date: "2021-03-07T10:08:56+09:00"
description: 지수족
draft: false
title: Exponential Family
weight: 3
---

## Exponential Family
한국어로는 지수족 또는 지수류라고도 하지만, 영어로 보는 편이 직관적으로 받아들이는 데에 편할 것이다.

`$f(x;\theta) = \begin{cases}
  exp\big[p(\theta)K(x) + s(x) + q(\theta)\big] & x \in S \\
  0 & o.w
\end{cases} $`

1. S does not depend on `$\theta$`
2. `$p(\theta)$` is a nontrivial continuous function of `$\theta \in \Omega$`
3-1. If X is continuous, `$K'(x) \neq 0$` and `$s(x)$` is continuous function.
3-2. If X is discrete, `$K(x)$` is nontrivial function.

또는
`$ f(y|\phi) = \begin{cases}
  h(y)c(\phi)exp\big[\phi K(y)\big] & y \in S \\
  0 & o.w
\end{cases} $`

1. S does not depend on `$\phi$`
2. `$\phi$` is a nontrivial continuous function of `$\theta \in \Omega$`
3-1. If X is continuous, `$K'(y) \neq 0$` and `$h(y)$` is continuous function.
3-2. If X is discrete, `$K(y)$` is nontrivial function.

첫번째는 Hogg 책을 기준으로 서술한 것이며, 두번째는 FCB 기준으로 서술한 것이다.  
즉 위는 Frequentist 입장, 아래는 Bayesian 입장이라고 보면 된다. 수식에 있어서 큰 차이는 없지만, parameter가 given인지 아닌지가 차이라고 볼 수 있다.

### Sufficient Statistic
우선은 Hogg책 서술을 기준으로 이야기해보자.
`$X_1, ..., X_n \text{ ~ iid } f(x;\theta)$`이고 `$f(x;\theta)$`가 exponential family라고 한다면,  
`$Y_1 = \sum_{i=1}^{n}K(X_i)$`는 `$\theta$`에 대한 완전충분통계량(complete sufficient statistic)이다.

###### 그렇다면 Sufficient 하다는 것의 의미는 무엇일까?
Defintion: `$p(X_1, ..., X_n|Y_1=y_1)$`가 `$\theta$`에 의존하지 않는다.

즉 `$\frac{p(X_1, ..., X_n, Y_1=y_1;\theta)}{p(Y_1=y_1; \theta)}$`를 계산할 때, `$\theta$`에 의해 값이 좌지우지 되지 않는다는 것이다.
이를 풀어서 말하자면, 각각 `$X_1,...,X_n$` 데이터를 직접 알지는 못하더라도 이들에 대한 정보가 `$Y_1$`에 들어가있다는 것이다. 그렇기 때문에 `$Y_1$` 값을 알게 되면 `$X_1,...,X_n$`의 joint probability를 계산할 수 있다. 그래서 **충분(sufficient)**하다는 것이다.
