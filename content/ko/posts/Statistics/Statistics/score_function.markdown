---
collapsible: false
date: "2021-03-07T10:08:56+09:00"
title: score function
weight: 99
---

## Score Function
`$X \text{ ~ } f(x;\theta)$`일 때, score function `$s(\theta;x)$`은 다음과 같이 정의한다.
$$\frac{\partial}{\partial\theta}logf(x;\theta) $$
`\begin{align}
E\big[s(\theta;x) \big] &= \int\frac{\partial}{\partial\theta}logf(x;\theta)f(x;\theta)\partial x \\
&= \int\frac{f'(x;\theta)}{f(x;\theta)}f(x;\theta)\partial x \\
&= \frac{\partial}{\partial\theta}\int f'(x;\theta) \\
&= \frac{\partial}{\partial\theta}1 = 0
\end{align}`

score function은 log likelihood의 기울기를 나타낸다는 데에서 의미가 있다.  
score function의 평균이 0이라는 것의 의미를 그래프를 likelihood 그래프를 상상하여 생각해보자.
