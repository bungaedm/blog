---
collapsible: false
date: "2021-02-21T10:08:56+09:00"
description: null
draft: false
title: dplyr
weight: 2
---

# dplyr 패키지 훑어보기

```r
library(tidyverse)
```

# 목차
1. rowwise
1-1. pmax

### 데이터
{{< expand "data" >}}

```r
tmp <- tibble(x=round(rnorm(n=5, mean=5, sd=1)),
       y=round(rnorm(n=5, mean=5, sd=3)),
       z=round(rnorm(n=5, mean=5, sd=5)))
tmp
```

```
## # A tibble: 5 x 3
##       x     y     z
##   <dbl> <dbl> <dbl>
## 1     4     2    14
## 2     6     4     4
## 3     6     0     2
## 4     4    11    13
## 5     5     4     6
```
{{< /expand >}}

#### 1. rowwise()
행별로 최대값 구하기
{{< expand "rowwise 예시" >}}

```r
#올바른 버전
tmp %>%
  rowwise() %>% 
  mutate(max = max(x,y,z))
```

```
## # A tibble: 5 x 4
## # Rowwise: 
##       x     y     z   max
##   <dbl> <dbl> <dbl> <dbl>
## 1     4     2    14    14
## 2     6     4     4     6
## 3     6     0     2     6
## 4     4    11    13    13
## 5     5     4     6     6
```

```r
#잘못된 버전
tmp %>%
  mutate(max = max(x,y,z))
```

```
## # A tibble: 5 x 4
##       x     y     z   max
##   <dbl> <dbl> <dbl> <dbl>
## 1     4     2    14    14
## 2     6     4     4    14
## 3     6     0     2    14
## 4     4    11    13    14
## 5     5     4     6    14
```
{{< /expand >}}

#### 1-1. pmax
그런데 사실은 여기서 `rowwise`를 사용하지 않고, `pmax`를 사용하면 보다 간단하게 구할 수 있기도 하다.
{{< expand "pmax 예시" >}}

```r
#간단한 버전
tmp %>% 
  mutate(max = pmax(x,y,z))
```

```
## # A tibble: 5 x 4
##       x     y     z   max
##   <dbl> <dbl> <dbl> <dbl>
## 1     4     2    14    14
## 2     6     4     4     6
## 3     6     0     2     6
## 4     4    11    13    13
## 5     5     4     6     6
```
{{< /expand >}}
