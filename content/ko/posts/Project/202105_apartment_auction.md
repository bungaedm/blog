---
date: "2021-05-03T10:08:56+09:00"
description: null
draft: false
title: 아파트 경매가격 예측
weight: 1
---

# ESC 2021 SPRING Final Project
- 분석기간: 2021 May ~ 2021 June
- 연세대학교 통계학회 ESC 2021년 봄학기에 진행한 파이널 프로젝트 내용이다. 베이지안 회귀분석 코딩 파트를 맡았다.

## My Role
1. 베이지안 회귀분석 코딩
  - BMA(Bayesian Model Averaging)을 통해 최종 변수 선택
  - `conditional probability`, `g-prior`, `BMA` 과정을 모두 직접 코딩하였다. 

2. 명목형 변수 전처리
  - 심리학 논문에서 접한 Platt's Probability 개념 활용

## What I Have Learned
1. 타겟 변수 변경
  - 낙찰가(Hammer_price)가 타겟 변수였는데, 아파트마다 가격 편차가 크고 최저경매액(경매 시작 금액)이라는 변수와 큰 상관관계가 있어서 다른 변수의 설명력을 다 잡아먹는 문제가 있었다.
  - 그래서 낙찰가를 최저경매액으로 나눈 '최저경매액 대비 상승률(y2)'를 타겟 변수로 변경하여 예측 task에 활용하였다.
  - 그 결과, 성능이 크게 향상되었다.
  - 타겟 변수를 유의미하게 처리한 후 예측을 시도한 첫 번째 프로젝트여서 개인적으로 큰 배움을 얻을 수 있었다.

2. Platt's Probability
  - 흔하게 사용되는 `One-hot Encoding`이나, CatBoost에서 활용하는 `Greedy target statistics`가 아닌 새로운 방법을 시도해보았다는 데에 의의가 있다.
  - 추후 다양한 데이터에 적용해봄으로써 명목형 변수를 전처리하는 법에 대해서 연구해보고자 한다.

3. Bayesian Linear Regression
  - 빈도주의자 관점에서 MSE를 최소화하는 최적의 `$\beta$`를 찾는 것이 아니라, 베이지안 관점에서 `$\beta$`가 fixed value가 아니라 분포를 갖는 모수로서 보고 최적의 값을 찾아나가는 코딩을 할 수 있었다.
  - 해당 과정을 R이 아니라 python으로 함에 따라, `conditional probability`, `g-prior`, `BMA` 과정을 모두 직접 코딩하면서 python 코딩 실력도 적당히 향상시킬 수 있었다.

---

## Presentation

![slide1](images/posts/project/apartment_auction/Slide1.PNG)
![slide2](images/posts/project/apartment_auction/Slide2.PNG)
![slide3](images/posts/project/apartment_auction/Slide3.PNG)
![slide4](images/posts/project/apartment_auction/Slide4.PNG)
![slide5](images/posts/project/apartment_auction/Slide5.PNG)
![slide6](images/posts/project/apartment_auction/Slide6.PNG)
![slide7](images/posts/project/apartment_auction/Slide7.PNG)
![slide8](images/posts/project/apartment_auction/Slide8.PNG)
![slide9](images/posts/project/apartment_auction/Slide9.PNG)
![slide10](images/posts/project/apartment_auction/Slide10.PNG)
![slide11](images/posts/project/apartment_auction/Slide11.PNG)
![slide12](images/posts/project/apartment_auction/Slide12.PNG)
![slide13](images/posts/project/apartment_auction/Slide13.PNG)
![slide14](images/posts/project/apartment_auction/Slide14.PNG)
![slide15](images/posts/project/apartment_auction/Slide15.PNG)
![slide16](images/posts/project/apartment_auction/Slide16.PNG)
![slide17](images/posts/project/apartment_auction/Slide17.PNG)
![slide18](images/posts/project/apartment_auction/Slide18.PNG)
![slide19](images/posts/project/apartment_auction/Slide19.PNG)
![slide20](images/posts/project/apartment_auction/Slide20.PNG)
![slide21](images/posts/project/apartment_auction/Slide21.PNG)
![slide22](images/posts/project/apartment_auction/Slide22.PNG)
![slide23](images/posts/project/apartment_auction/Slide23.PNG)
![slide24](images/posts/project/apartment_auction/Slide24.PNG)
![slide25](images/posts/project/apartment_auction/Slide25.PNG)
![slide26](images/posts/project/apartment_auction/Slide26.PNG)
![slide27](images/posts/project/apartment_auction/Slide27.PNG)
![slide28](images/posts/project/apartment_auction/Slide28.PNG)
![slide29](images/posts/project/apartment_auction/Slide29.PNG)
![slide30](images/posts/project/apartment_auction/Slide30.PNG)
![slide31](images/posts/project/apartment_auction/Slide31.PNG)
![slide32](images/posts/project/apartment_auction/Slide32.PNG)
![slide33](images/posts/project/apartment_auction/Slide33.PNG)
![slide34](images/posts/project/apartment_auction/Slide34.PNG)
![slide35](images/posts/project/apartment_auction/Slide35.PNG)
![slide36](images/posts/project/apartment_auction/Slide36.PNG)
![slide37](images/posts/project/apartment_auction/Slide37.PNG)
![slide38](images/posts/project/apartment_auction/Slide38.PNG)
![slide39](images/posts/project/apartment_auction/Slide39.PNG)
![slide40](images/posts/project/apartment_auction/Slide40.PNG)

---

<br>
<br>