---
collapsible: false
date: "2021-03-30T10:09:56+09:00"
description: null
draft: false
title: 맵 커스텀
weight: 1
---

## Intro
본 포스팅은 [WeViz 유튜브](https://www.youtube.com/watch?v=quZfx68_erE)를 적극 참고하였습니다.

## 결과물
{{< img src="/images/posts/tableau/map_custom.png" width="1000px" position="center" >}}

## 1. 기본 수정
  - [맵] - [맵 계층]
  - 스타일 **어둡게** 변경
  - 기본도, 토지 피복도 체크박스 해제
  - 해안선 체크

## 2. mapbox 이용하기
  - [mapbox 홈페이지](https://account.mapbox.com/)
  - Studio -> New Style -> Basic(다른 스타일 선택해도 무방)

### 2-1. 영역 색깔 바꾸기
특정 영역 클릭 후 색깔 변경

### 2-2. 폰트 바꾸기
STEP1. 텍스트는 기본적으로 자물쇠 잠금 해제(override) 클릭 후 변경 가능
STEP2. 특정 레이블을 선택 -> Components -> Typography
STEP3. 원하는 글꼴 검색 후 적용(Noto sans 추천)
  
### 2-3. 영어 레이블을 한글 레이블로 바꾸기
STEP1. 특정 레이블 클릭 -> Layers -> `T-country label` 클릭
STEP2. `override`를 클릭하여 `name_en`을 `name_ko`로 바꿔준다.

### 2-4. 배포하기
오른쪽 상단 Publish 클릭 -> `Publish as new`

### 2-5. Tableau로 가져오기
STEP1. mapbox `preview only` 링크 복사
STEP2. Tableau 맵 관리 -> 추가
STEP3. **내보내기** 꼭 하기! (다음에도 활용하기 위해서!)
   
## Reference
[1] [WeViz 유튜브](https://www.youtube.com/watch?v=quZfx68_erE)
