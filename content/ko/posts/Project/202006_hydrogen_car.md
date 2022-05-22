---
date: "2020-06-03T10:08:56+09:00"
description: null
draft: false
title: 수소차 충전소 입지 추천
weight: 1
---

## 수소차 충전소 입지 추천
- 분석기간: 2020 April ~ 2020 June
- 연세대학교 데이터사이언스입문 수업에서 진행한 프로젝트

## My Role
마침 지난 2019년 겨울방학 때 머신러닝을 공부해보기 시작해서, 주로 모델링과 관련된 역할을 많이 맡았다.
다양한 방법론을 시도하였는데, 의미 해석을 도출하기 위해 Random Forest를 채택하게 되었다.
하지만 데이터캠프를 통해서 shiny dashboard를 포함한 다양한 것들을 공부할 수 있었다.

## What I Have Learned
**1. 데이터 수집 및 전처리**
  - 기존에 데이터가 주어지는 것이 아니기 때문에, 필요한 데이터를 직접 일일이 찾아나서는 노력을 했다.
  - 그 과정에서 거리 변수를 직접 계산하여 파생변수를 만들었다.
  - 지역 단위를 최대한 동 단위로 나눠서 계산을 했다.

**2. tidyverse**
  - 이전에는 tidyverse 문법을 잘 모르고 기본 base 문법만 썼다.
  - tidyverse의 편리함을 잘 모르고 python만 겨울방학동안 썼었는데, R의 %>% pipe line과 ggplot의 편리함을 알 수 있었다.

**3. 다양한 R 활용법**
  - blogdown: R로 블로그 만들기
  - shiny: R로 대쉬보드 만들기
  - xaringan: R로 프렌젠테이션 자료 만들기


## Difficulty
1. 수소차 보유 대수 자체가 애초에 충분하지 못했기 때문에, 포아송회귀분석을 한번 해봤으면 어떨까 하는 아쉬움이 있다.
2. xaringan 패키지를 너무 늦게 알게 되어서, 숙련도가 높지 못했다.

## Result
[블로그](https://rhinoblog.netlify.app/)
![rhino_blog](images/posts/blog/rhino_blog.png)

[대쉬보드](https://ysuks.shinyapps.io/dashboard/)
![rhino_dashboard](images/posts/blog/rhino_dashboard.png)