---
date: "2020-09-04T10:08:56+09:00"
description: null
draft: false
title: NS Shop+ 홈쇼핑
weight: 1
---

## [BigContest] NS Shop+ 홈쇼핑 실적 예측
- 분석기간: 2020 August ~ 2020 September
- 본 대회는 `BigContest`에서 주최한 대회로, NS Shop+의 `2020년 6월 실적을 예측`하는 것이 주목적이었다.
- 그리고 이를 바탕으로 최적 수익을 고려한 요일별/시간대별/카테고리별 `편성 최적안 방안(모형)`을 제시하는 것까지가 대회 요구사항이었다.
- 평가방법은 `MAPE`(Mean Abosolute Percentage Error) 평균절대비율오차였다.


## My Role
**1. 상품군별 데이터 해석**
  - 생활용품, 가구, 침구, 무형 카테고리 데이터 해석
  - 상품군별로 특징이 다를 것으로 판단되어 역할을 분담하여 데이터 분석을 실시하였다.

**2. 노출시간 전처리**
  - (1-1) 반올림
  - (1-2) Missing Value Imputation (`ffill`)
  - (1-3) 그룹화
<br>
  - (2-1) 노출시간 누적 및 총합 계산

**3. Business 모델에 안 맞는 데이터 처리**
  - 취급액 = 판매단가 X 주문량
  - 취급액이 판매단가보다 작은 경우는 오류이므로, 0원으로 변경

**4. 상품명 전처리**
  - (1) 브랜드 추출
  - (2) 세트 상품 여부
  - (3) 상품군별 소분류 추출

## What I Have Learned
**1. 실제 데이터는 생각보다 오류가 많다.**
  - (My Role의 3번 참고)
  
**2. 제공되지 않은 정보가 많다.**
  - 다양한 할인경로로 인해, 고객마다 할인받은 금액이 다르다.

**3. 불균형데이터 Upsampling**
  - 상품군별 데이터 개수가 달랐는데, 부족한 카테고리의 경우 n배 데이터수를 늘려주었다.
  - 이후 SMOTE와 같은 방법론이 있다는 것을 알게 되었다.

**4. 파생변수 하나가 성능 향상으로 이어진다.**
  - `누적 노출시간`이 성능 향상에 크게 기여하였다.

**5. Bayesian Optimization**
  - hyperparameter 선정 시, 사전지식을 활용하면 보다 좋은 성능으로 이어질 수 있다.

  
## Difficulty
1. 데이터가 정제되지 않거나 잘못 정제된 경우가 있었다.
2. 상품군별로 특징이 상이했다.
3. 학습데이터에 존재하지 않는 정보가 평가데이터에 존재했다.
4. 코로나로 인한 영향을 계산하기 어려웠다. 

## Result
**Best Broadcasting Schedule Suggestion**
1. 상품군별 PRIME TIME 선정
  - Prime Time이란, 해당 상품군이 가장 인기있는 시간대를 의미한다.
2. 상품군별 추천 상품
  - 매출액에 큰 부분을 차지하는 상품들은 Prime Time에 배치한다.
  - 구매수량에 영향을 받는 상품들은 비교적 애매한 시간대에 배치한다.
  - EX) 의류의 경우, prime time에 사치품(다운코트, 밍크코트)을 판매하고, 그외 시간대에는 기본 아이템(플리스 자켓, 티셔츠)을 판매한다.

---

## Presentation
{{< expand "Presentation Slides. Click to expand." >}}![nsshop02](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-02.png)
  ![nsshop03](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-03.png)
  ![nsshop04](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-04.png)
  ![nsshop05](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-05.png)
  ![nsshop06](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-06.png)
  ![nsshop07](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-07.png)
  ![nsshop08](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-08.png)
  ![nsshop09](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-09.png)
  ![nsshop10](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-10.png)
  ![nsshop11](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-11.png)
  ![nsshop12](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-12.png)
  ![nsshop13](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-13.png)
  ![nsshop14](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-14.png)
  ![nsshop15](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-15.png)
  ![nsshop16](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-16.png)
  ![nsshop17](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-17.png)
  ![nsshop18](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-18.png)
  ![nsshop19](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-19.png)
  ![nsshop20](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-20.png)
  ![nsshop21](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-21.png)
  ![nsshop22](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-22.png)
  ![nsshop23](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-23.png)
  ![nsshop24](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-24.png)
  ![nsshop25](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-25.png)
  ![nsshop26](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-26.png)
  ![nsshop27](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-27.png)
  ![nsshop28](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-28.png)
  ![nsshop29](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-29.png)
  ![nsshop30](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-30.png)
  ![nsshop31](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-31.png)
  ![nsshop32](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-32.png)
  ![nsshop33](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-33.png)
  ![nsshop34](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-34.png)
  ![nsshop35](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-35.png)
  ![nsshop36](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-36.png)
  ![nsshop37](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-37.png)
  ![nsshop38](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-38.png)
  ![nsshop39](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-39.png)
  ![nsshop40](images/posts/project/nsshop_bigcontest/NS홈쇼핑실적예측-40.png)
{{< /expand >}}

<br>
<br>