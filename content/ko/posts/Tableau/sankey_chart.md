---
collapsible: false
date: "2021-03-30T10:10:56+09:00"
description: null
draft: false
title: 생키 차트
weight: 1
---

## Intro
생키 차트(Sankey Chart)는 흐름(Flow)을 보여주기에 최적화된 차트 형태이다. 예를 들어 지역이나 국가 간의 에너지를 표현하는데에 적합하다. 

{{<expand "기본 전처리 코드(R)">}}
```r
library(tidyverse)
library(xlsx)

setwd('C:/Users/bunga/Desktop/Tableau/data')
data <- read.table('seoul_park_raw.txt', header=TRUE, sep='\t')
data <- as.tibble(data)
data <- data %>%
  mutate(ym = (ym*100)%%100)
data <- data %>% select(-total)
data <- data[data['region'] != '합계',]
data <- data %>% 
  mutate(ord = as.numeric(gsub(',', '', ord)),
         health = as.numeric(gsub(',', '', health)),
         bicycle = as.numeric(gsub(',', '', bicycle)),
         event = as.numeric(gsub(',', '', event)),
         special = as.numeric(gsub(',', '', special)),
         etc = as.numeric(gsub(',', '', etc))) %>% 
  mutate_all(~ifelse(is.na(.), 0, .))

write.table(data, file = "seoul_park.txt", sep = "\t", row.names = FALSE)
write.xlsx(data,file='seoul_park.xlsx', sheetName = 'Sheet1')
```
{{</expand>}}


## Reference
[1] [WeViz 유튜브](https://www.youtube.com/watch?v=vM8d2aqsunk)
[2] https://qliksense.tistory.com/32
[3] 서울열린데이터광장