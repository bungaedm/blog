---
date: "2020-01-02T10:08:56+09:00"
description: null
draft: false
title: YZ 투자자 프로파일링
weight: 1
---

## NH투자증권 Y&Z세대 투자자 프로파일링

본 대회는 NH투자증권에서 주최한 대회로, 2020년 급격하게 늘어난 20~30대 투자자들에 대한 분석을 하고 이를 <span style='background-color: #ffbf00'>시각화</span>하는 것이 주목적이었다.

코드가 상당히 복잡한 관계로 Dacon에 코드공유한 링크와 이미지 일부를 올리는 것으로 포스팅을 대체하고자 한다.

##### [Dacon 코드 공유](https://dacon.io/competitions/official/235663/codeshare/2209?page=5&dtype=recent&ptype=pub)

### Factor Analysis
![Factor Analysis](images/posts/nh_yz/Factor_Analysis_Diagram.png)

### Word Cloud_국내
![wdcd_kr](images/posts/nh_yz/wdcd_kr.png)

### Word Cloud_해외
![wdcd_oss](images/posts/nh_yz/wdcd_oss.png)

### Cluster Polygon
![Cluster Polygon](images/posts/nh_yz/Cluster_Polygon.png)

### Cluster Characteristics
![Cluster Characteristics](images/posts/nh_yz/Cluster_Characteristic.png)

### Idea Table
![Idea Table](images/posts/nh_yz/idea_table.png)

### Idea Sample
![Idea Sample](images/posts/nh_yz/idea_sample.png)

---

### Domain Knowledge

#### 1. 해외주식 소수점 거래
- 현재 신한금융투자, 한국투자증권에서 실현 중 
- 의결권, 배당권을 빼고 소수점 거래 가능! (ex. 한투 미니스톡)

#### 2. 휴면 고객에 대한 이벤트도 주기적으로 한다.


#### 3. ETP (= ETF + ETN)
[금융투자협회: 한눈에 알아보는 레버리지 ETP Guide](http://www.kifin.or.kr/)
- **괴리율**
  - 순자산가치(NAV) & 지표가치(IV)은 장중 실시간으로 산출한다.
    - 전일 종가로 확정된 순자산 가치 + 당일의 가격 움직임
  - 괴리율이 플러스(+): 추적대상 지수보다 고평가되어 있다(=비싸게 거래되고 있다).
  - 괴리율이 마이너스(-): 추적대상 지수보다 저평가되어 있다(=싸게 거래되고 있다.)
  - 괴리율이 너무 커지면, 한국거래소에서 단일가 매매 또는 매매거래정지를 시킬 수 있다.
  - 유동성공급자(Liquidity Provider, LP)
    - 괴리율이 과도하게 높을 경우: ETP를 매도하거나 대상자산을 매입해야겠다!
    - 괴리율이 과도하게 낮을 경우: ETP를 매수하거나 대상자산을 매도해야겠다!
    
- **복리 효과**
  - 레버리지 ETP의 운용방식: '일별' 수익률의 &pm;2배
  - 상승장일 경우, 2X의 상승률 > 인버스2X 하락률
  - 하락장일 경우, 2X의 하락률 < 인버스2X 상승률
  - 횡보장일 경우, 2X와 인버스2X 모두 손해볼 가능성이 높다. 즉, 장기투자에 적합하지 않다.
  - 상식: 레버리지 상품을 만들기 위해서는 파생상품을 집어넣는다.

- **롤오버(roll-over)**
  - 파생상품의 거래월물을 교체하는 것(파생상품의 만기에 발생)
  - 즉, 롤오버 과정에서 거래월물의 가겨차이에 따라 ETP의 자산가치 변동이 이루어질 수 있다.
  - 선물 '매수'포지션을 보유하고 있는 레버리지(2X) ETP
    - 거래월물의 가격차이가 (+)상태이면 이득, (-)상태이면 손해
  - 선물 '매도'포지션을 보유하고 있는 레버리지(2X) ETP
    - 거래월물의 가격차이가 (+)상태이면 손해, (-)상태이면 이득

- **지수의 방법론**
  - 괴리율, 복리효과, 롤오버만큼의 중요성은 아니지만, 간과해서는 안되는 POINT
  - ex) 2020년 4월 유례 없는 마이너스 유가 폭락으로 인한 <br> 원유선물의 거래월물 편입대상과 롤오버 방식 변화

---

## 배운 점
###### 1. 크롤링 능력
  - 크롤링은 Python을 통해서 진행하였다.
  - `BeautifulSoup`, selenium의 `webdriver` 등의 라이브러리를 활용하였다.
  - (1) 해외사이트 크롤링의 어려움
    - 느리고, 자잘한 오류가 생긴다.
    - 예를 들어, 특정 단어를 검색을 해도 나오지 않는다.
  - (2) 크롤링 권한 접근
    - 접근 권한의 제한으로 단순 크롤링을 할 수 없는 경우<br>Network-XHR의 `Request Headers` 활용하기
    - 에러 핸들링 관점에서 `데이터 엔지니어링`의 중요성 체감
###### 2. 파생변수 제작의 어려움
  - `run_away_cd`(휴면 여부)  라는 변수를 새롭게 만들었다.
  - 총 세가지 기준에 의해 각 고객의 휴면 여부를 판단하였는데, 그 기준은 다음과 같다.
    - (1) 거래주기에 비해 거래휴식기가 지나치게 긴 경우
    - (2) 예상 거래횟수에 비해 실제 거래횟수가 눈에 띄게 적은 경우
    - (3) 최근 2달 이내에 계좌개설한 사람들은 제외
  - 해당 변수를 만들고 유의성을 검토하는 데에 적지 않은 시간을 투자하였던 경험👍
  - 해당 변수를 만들기 위해, 중간과정에서 orr_prd, orr_cyl, orr_idx_1, orr_idx2를 만들었다.
###### 3. 요인분석 Factor 조합 노하우
  - 어려운 변수보다는 직관적으로 <mark>간단한 변수들의 조합</mark>으로 구성하는 것이 좋다..
  - 왜냐하면 요인분석 자체가 해석을 목적으로 하기 때문이다.

<br>
<br>