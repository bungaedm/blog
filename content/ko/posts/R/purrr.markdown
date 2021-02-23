---
collapsible: false
date: "2021-02-21T10:08:56+09:00"
description: null
draft: false
title: purrr
weight: 2
---

# purrr 패키지 훑어보기

```r
library(purrr)
```

## 목차
1. map, map2

#### map, map2

```r
num <- c(1,2,4,5,7)
num2 <- c(3,5,6,8,9)

#list
map(num, function(x){x^2})
```

```
## [[1]]
## [1] 1
## 
## [[2]]
## [1] 4
## 
## [[3]]
## [1] 16
## 
## [[4]]
## [1] 25
## 
## [[5]]
## [1] 49
```

```r
map2(num, num2, sum)
```

```
## [[1]]
## [1] 4
## 
## [[2]]
## [1] 7
## 
## [[3]]
## [1] 10
## 
## [[4]]
## [1] 13
## 
## [[5]]
## [1] 16
```

```r
#numeric vector
map_dbl(num, function(x){x^2})
```

```
## [1]  1  4 16 25 49
```

```r
map2_dbl(num, num2, sum)
```

```
## [1]  4  7 10 13 16
```

