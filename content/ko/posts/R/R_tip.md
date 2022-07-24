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

## 4. 자동정렬 단축키
`ctrl + I` 

## 5. python의 dictionary처럼 사용하기

```r
# recode: case_when의 특수한 형태로서 데이터를 교체할 때 사용할 수 있을 것이다.
tmp_char <- sample(c("a", "b", "c"), 10, replace = TRUE)
# !!!을 활용하면, python에서 dictionary 형태로 활용하는 것처럼 쓸 수 있다.
level_key <- c(a = "apple", b = "banana", c = "carrot")
recode(tmp_char, !!!level_key)
```

## 6. Rmarkdown ggplot 그래프에서 한글 출력하기
```r
library(extrafont)
library(showtext)

fond_add('nanum', 'NanumGothic.ttf')
showtext_auto()
font_import()
theme_set(theme_bw(base_family='nanum'))
```
- 그런데 code chunk 안에 들어가는 한글에서는 pdf 만들 때 안 나오던데 이거는 어떻게 해야하는지 아직 모르겠다.

## 7. 반복문에서 tidyverse 변수명 처리
```r
variable_name = rlang::ensym(variable_name)
```