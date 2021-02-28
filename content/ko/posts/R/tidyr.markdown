---
collapsible: false
date: "2021-02-28T10:08:56+09:00"
description: null
draft: false
title: tidyr
weight: 2
---

# tidyr 패키지 훑어보기
tidyr은 tidy data를 형성하기 위해 고안된 패키지입니다. tidy data에서 1) 열은 변수를 의미하고, 2) 행은 하나의 케이스를 의미하며, 3) 하나의 셀은 하나의 값을 의미합니다.


```r
library(tidyverse)
```
{{< img src="/images/posts/r/tidyr/tidyr.png" width="250px" position="center" >}}

## 목차
1. nest

#### nest
예시를 통해, 단순히 group_by를 하는 것과 group_by 이후 nest를 한 후에 어떻게 데이터가 정리되는지 확인해보자.
{{<expand "nest 예시">}}

```r
iris %>%
  group_by(Species)
```

```
## # A tibble: 150 x 5
## # Groups:   Species [3]
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
##           <dbl>       <dbl>        <dbl>       <dbl> <fct>  
##  1          5.1         3.5          1.4         0.2 setosa 
##  2          4.9         3            1.4         0.2 setosa 
##  3          4.7         3.2          1.3         0.2 setosa 
##  4          4.6         3.1          1.5         0.2 setosa 
##  5          5           3.6          1.4         0.2 setosa 
##  6          5.4         3.9          1.7         0.4 setosa 
##  7          4.6         3.4          1.4         0.3 setosa 
##  8          5           3.4          1.5         0.2 setosa 
##  9          4.4         2.9          1.4         0.2 setosa 
## 10          4.9         3.1          1.5         0.1 setosa 
## # ... with 140 more rows
```

```r
iris %>%
  group_by(Species) %>%
  nest()
```

```
## # A tibble: 3 x 2
## # Groups:   Species [3]
##   Species    data             
##   <fct>      <list>           
## 1 setosa     <tibble [50 x 4]>
## 2 versicolor <tibble [50 x 4]>
## 3 virginica  <tibble [50 x 4]>
```
{{</expand>}}

###### 참고
[1] https://gomguard.tistory.com/229
