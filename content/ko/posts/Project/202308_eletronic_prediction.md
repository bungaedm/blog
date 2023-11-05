---
date: "2023-08-31T10:08:56+09:00"
description: null
draft: false
title: 전력사용량 예측 AI 경진대회
weight: 1
---

## 2023 전력사용량 예측 AI 경진대회
- 분석기간: 2023 June ~ 2023 July
- 데이콘에서 진행한 대회였습니다.

## 1. What I Have Learned

### 1-1. Prophet
![prophet](images/posts/project/202308_electronic/prophet.png)

`Prophet`은 Meta(구 Facebook)에서 배포한 시계열 예측 모델이다. 세부적으로 여러 argument를 지정해줄 수 있지만, 기본적으로 weekly, monthly trend 등을 잘 파악해줄 수 있는 모델이다. 유의미한 변수가 없는 경우 잘 활용해볼 수 있는 모델이라고 생각한다.

### 1-2. XGBoost
XGBoost는 당연히 원래 알고 있었지만, 해당 대회의 수상작이 XGBoost를 활용했다는 것을 확인할 수 있었다. 유의미한 변수가 있는 경우, 시계열 모델인 Prophet을 쓰는 것보다 XGBoost를 쓰는 것이 더 효율적일 수 있다는 것을 알 수 있었다.

## 2. Conclusion

해당 대회에서 수상을 하지는 못했으나, Prophet을 알 수 있는 좋은 기회였다. GLMM과 같은 모델과 더불어서 성능 향상을 시도해보았으나, 여러 제한점이 있어서 최적화를 시도할 여유가 없었다.