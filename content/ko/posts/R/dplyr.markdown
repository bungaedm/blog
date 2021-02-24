---
collapsible: false
date: "2021-02-21T10:08:56+09:00"
description: null
draft: false
title: dplyr
weight: 2
---


```r
library(tidyverse)
```

# 목차
1. rowwise

### 데이터

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
## 1     5     4     9
## 2     4     8     8
## 3     6     7     3
## 4     5     0     4
## 5     6     8     8
```

#### 1. rowwise()
행별로 최대값 구하기

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
## 1     5     4     9     9
## 2     4     8     8     8
## 3     6     7     3     7
## 4     5     0     4     5
## 5     6     8     8     8
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
## 1     5     4     9     9
## 2     4     8     8     9
## 3     6     7     3     9
## 4     5     0     4     9
## 5     6     8     8     9
```

그런데 사실은 여기서 `rowwise`를 사용하지 않고 보다 간단하게 구할 수 있기도 하다.

```r
#간단한 버전
tmp %>% 
  mutate(max = pmax(x,y,z))
```

```
## # A tibble: 5 x 4
##       x     y     z   max
##   <dbl> <dbl> <dbl> <dbl>
## 1     5     4     9     9
## 2     4     8     8     8
## 3     6     7     3     7
## 4     5     0     4     5
## 5     6     8     8     8
```

