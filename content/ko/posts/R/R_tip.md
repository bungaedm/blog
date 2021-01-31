---
collapsible: false
date: "2021-01-31T10:08:56+09:00"
description: null
draft: false
title: R 꿀팁
weight: 2
---

## 1. 추천 사이트
- [R for data science](https://r4ds.had.co.nz/)

## 2. 검색 팁
구글링 시, 뒤에 'Rpubs' 붙이기

## 3. blogdown Auto-Knit 끄기
Ctrl+S 단축키로 수시로 저장하는 습관 때문에, Rmd 파일을 작업할 때 knit가 수시로 되어 작업속도에 영향을 미친다.
이럴 때는 `.Rprofile`이라는 이름의 파일을 찾아서  
> blogdown.knit.on_save = `TRUE`

라는 코드에서 `TRUE`를 `FALSE`로 바꿔주어야 한다.