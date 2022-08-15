---
collapsible: false
date: "2022-03-15T10:08:56+09:00"
description: Partial Least Regression
title: Partial Least Regression
weight: 2
---

# Partial Leaset Regression

Feature Extraction 방법 중 하나로, PCA처럼 공분산을 최대화하는 방법이다. 종속변수가 여러 개일 때도, 범주형 변수일 때에도 유연하게 활용이 가능하다는 장점이 있다..

Gradient Boosting처럼 잔차에 대해 계속 학습해나가는 모델이지만, 독립변수들을 일종의 PCA처럼 차원축소를 한 후에 회귀분석의 틀에서 분석한다는 특징이 있다.

![PLS1](images/posts/machine_learning/partial_least_regression1.jpg)
![PLS2](images/posts/machine_learning/partial_least_regression2.jpg)
