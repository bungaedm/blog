---
collapsible: false
date: "2021-03-14T10:08:56+09:00"
description: null
draft: false
title: naniar
weight: 1
---

# naniar 패키지 훑어보기
NA 관련해서 직관적으로 깔끔한 그래프로 훑어볼 수 있게 도와주는 패키지이다.  
본 포스팅은 [해당 사이트](https://naniar.njtierney.com/articles/naniar-visualisation.html)를 적극참고하여 작성하였다.


```r
library(tidyverse)
library(naniar)
```

## vis_miss

```r
vis_miss(airquality)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-2-1.png" width="672" />

## gg_miss_var

```r
gg_miss_var(airquality)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-3-1.png" width="672" />

```r
gg_miss_var(airquality, show_pct = TRUE)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-3-2.png" width="672" />

```r
gg_miss_var(airquality, facet = Month)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-3-3.png" width="672" />

## gg_miss_case

```r
gg_miss_case(airquality)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-4-1.png" width="672" />

## gg_miss_upset

```r
gg_miss_upset(riskfactors)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-5-1.png" width="672" />

```r
n_var_miss(riskfactors)
```

```
## [1] 24
```

```r
gg_miss_upset(riskfactors, nsets = n_var_miss(riskfactors)) 
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-5-2.png" width="672" />

```r
gg_miss_upset(riskfactors, nsets = 4) #nset: 변수 개수
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-5-3.png" width="672" />

```r
gg_miss_upset(riskfactors, nsets = 10, nintersects = 5) #nintersects: 변수조합 수
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-5-4.png" width="672" />

## geom_miss_point
ggplot과 응용

```r
ggplot(airquality, aes(x = Ozone, y = Solar.R)) +
  geom_point()
```

```
## Warning: Removed 42 rows containing missing values (geom_point).
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-6-1.png" width="672" />

```r
ggplot(airquality, aes(x = Ozone, y = Solar.R)) +
  geom_miss_point()
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-6-2.png" width="672" />

## gg_miss_fctfas

```r
gg_miss_fct(oceanbuoys, year)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-7-1.png" width="672" />

## miss_var_summary

```r
riskfactors %>%
  group_by(marital) %>%
  miss_var_summary()
```

```
## # A tibble: 231 x 4
## # Groups:   marital [7]
##    marital variable      n_miss pct_miss
##    <fct>   <chr>          <int>    <dbl>
##  1 Married smoke_stop       120    91.6 
##  2 Married pregnant         117    89.3 
##  3 Married smoke_last        84    64.1 
##  4 Married smoke_days        73    55.7 
##  5 Married drink_average     68    51.9 
##  6 Married health_poor       67    51.1 
##  7 Married drink_days        67    51.1 
##  8 Married weight_lbs         6     4.58
##  9 Married bmi                6     4.58
## 10 Married diet_fruit         4     3.05
## # ... with 221 more rows
```

## miss_var_span, gg_miss_span

```r
miss_var_span(pedestrian, hourly_counts, span_every = 3000)
```

```
## # A tibble: 13 x 5
##    span_counter n_miss n_complete prop_miss prop_complete
##           <int>  <int>      <dbl>     <dbl>         <dbl>
##  1            1      0       3000  0                1    
##  2            2      0       3000  0                1    
##  3            3      1       2999  0.000333         1.00 
##  4            4    121       2879  0.0403           0.960
##  5            5    503       2497  0.168            0.832
##  6            6    555       2445  0.185            0.815
##  7            7    190       2810  0.0633           0.937
##  8            8      0       3000  0                1    
##  9            9      1       2999  0.000333         1.00 
## 10           10      0       3000  0                1    
## 11           11      0       3000  0                1    
## 12           12    745       2255  0.248            0.752
## 13           13    432       2568  0.144            0.856
```

```r
gg_miss_span(pedestrian, hourly_counts, span_every = 3000)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-9-1.png" width="672" />

```r
gg_miss_span(pedestrian, hourly_counts, span_every = 3000, facet = sensor_name)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-9-2.png" width="672" />

## 그외 다양한 

```r
gg_miss_case_cumsum(airquality)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-10-1.png" width="672" />

```r
gg_miss_var_cumsum(airquality)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-10-2.png" width="672" />

```r
gg_miss_which(airquality)
```

<img src="/ko/posts/R/시각화/naniar_files/figure-html/unnamed-chunk-10-3.png" width="672" />
