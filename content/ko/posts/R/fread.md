---
collapsible: false
date: "2021-02-08T10:08:56+09:00"
description: null
draft: false
title: fread
weight: 99
---

# fread 패키지로 대용량 데이터 빠르게 불러오기
약 24000행의 샘플 csv가 있다고 가정하자. 그렇다면 fread와 read.csv의 성능은 다음과 같다.

```{r}
library(data.table)

fread('sample.csv') #약 24000x3

system.time(fread('sample.csv')) 
# 사용자  시스템 elapsed 
#  0.02    0.00    0.01 

system.time(read.csv('sample.csv'))
# 사용자  시스템 elapsed 
#  0.74    0.03    0.77
```

read.csv는 0.77초가 걸리는 데에 반해 fread는 0.01초 만에 읽어왔다.
이외에도 3백만 행의 csv으로 실험해본 결과, 각각 2초와 33초로 그 성능 차이가 더욱 도드라짐을 알 수 있었다.