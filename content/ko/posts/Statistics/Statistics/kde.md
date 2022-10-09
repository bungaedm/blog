---
date: "2022-10-07T00:08:29+09:00"
description: null
draft: false
title: 커널밀도추정(KDE)
weight: 1
---

## Kernel Density Estimation

### Definition
커널함수는 원점을 중심으로 대칭이면서 적분값이 1인 non-negative 함수이다. 대표적으로는 Gaussian, Epanechnikov, Uniform 함수가 있다.

$$
\hat{f_h}(x) = \frac{1}{nh}\sum_{i=1}^{n}K(\frac{x_i-x}{h})
$$

### Code

```python
# Estimate with Gaussian kernel
estimator = stats.gaussian_kde(data[target], bw_method=bw)
d = estimator(data[target])
print('max:', np.max(d))
print('min:', np.min(d))
```

```python
# Plot KDE
data[target].plot.kde()
plt.show()
```

![KDE1](images/posts/statistics/kde/kde1.png)

## Reference
[1] https://darkpgmr.tistory.com/147
[2] https://seongkyun.github.io/study/2019/02/03/KDE/