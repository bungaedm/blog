---
author: 손지우
date: "2021-01-04"
tags: null
title: 빅콘테스트 NS Shop+ 홈쇼핑 실적 예측
---

빅콘테스트 챔피언리그 데이터분석 분야 <!--more-->

배운 점
1. 자연어 전처리
  - 정규표현식
  - 기존에 제시된 칼럼의 수가 굉장히 적었다. 그중에서 그나마 정보를 담고 있는 것은 '상품명' 칼럼이었다.
  - 이 데이터를 예를 들어, 'NIKE 스트라이프 셔츠'와 같은 형식으로 브랜드가 일반적으로 앞에 나오고 뒤에 디테일한 상품 분류가 나왔다.
  - 하지만 식료품과 같은 경우에는 브랜드가 없는 것들이 많았고, 띄어쓰기가 제대로 되어있지 않은 경우도 많았다.
  - (아마도 현실 데이터는 이것보다도 더 정돈되지 않은 경우가 많을 터이다...)
  - 또한 브랜드의 표기 자체가 통일되지 않은 경우가 있었다.(ex. 카사미아=까사미아)
  - NS Shop+ 공식홈페이지에서 나눠놓은 대분류, 중분류, 소분류 체계를 참고해서 나눴다.
  - 너무 많은 상품명이 있었기 때문에, 대분류 3개씩 묶어서 역할분담을 해서 나머지 분류를 채웠는데, 그러다보니 사소하게 통일되지 않은 분류기준이 있어서 추후 약간의 어려움을 겪었다.
  - 기준을 명확하게 정해두거나, 만약 그러지 못할 경우 최소한의 사람이 해당 업무를 했으면 어땠을까라는 생각이 든다.