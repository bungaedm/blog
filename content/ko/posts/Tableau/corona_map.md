---
collapsible: false
date: "2021-03-30T10:08:56+09:00"
description: null
draft: false
title: 코로나 
weight: 2
---

## Intro
본 포스팅은 [WeViz 유튜브](https://www.youtube.com/watch?v=KTnxZCNud1E)를 적극 참고하였습니다.
시각화에 관심이 있었는데, 해당 연합동아리 소속의 친구에게 추천을 받아서 참고하게 되었습니다.

## 결과물
{{< img src="/images/posts/tableau/corona_map.png" width="1000px" position="center" >}}

## 배운점
1. 구글 스프레드시트 GEOCODE 활용법
    - 경도(longitude), 위도(latitude) 자동 추출 기능

2. MAKELINE, MAKEPOINT 활용법
`MAKELINE(MAKEPOINT(30.602101,114.316826), MAKEPOINT([Latitude],[Longitude]))`

3. 맵 관리
    - 커스텀 맵 디자인 추가

## Reference
[1] [WeViz 유튜브](https://www.youtube.com/watch?v=KTnxZCNud1E)
[2] [데이터 출처](https://www.worldometers.info/coronavirus/?utm_campaign=homeAdUOA%3FSi)