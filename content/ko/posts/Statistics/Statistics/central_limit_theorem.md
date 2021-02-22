---
collapsible: false
date: "2021-02-21T10:08:56+09:00"
description: null
draft: false
title: 중심극한정리
weight: 1
---

# 중심극한정리(Central Limit Theorem)

`$ X_1, ... X_n $`을 평균이 `$\mu$`이고, 분산이 `$\sigma^2$`인 분포로부터 무작위로 얻은 샘플이라고 하자. (단, `$\sigma^2 < \infty$`)

그러면 `$Y_n = \frac{\sqrt{n}(\bar{X_n} - \mu)}{\sigma}$`은 `$N(0,1)$`을 극한분포로 갖는다.

#### 증명
mgf를 활용하여 증명한다. (추후 추가 예정) 