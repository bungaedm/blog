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
## 1     7     4     2
## 2     7     4     4
## 3     7     3     5
## 4     6     4     8
## 5     5     7     2
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
## 1     7     4     2     7
## 2     7     4     4     7
## 3     7     3     5     7
## 4     6     4     8     8
## 5     5     7     2     7
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
## 1     7     4     2     8
## 2     7     4     4     8
## 3     7     3     5     8
## 4     6     4     8     8
## 5     5     7     2     8
```

