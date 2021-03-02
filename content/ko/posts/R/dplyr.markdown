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
library(MASS)
```

# 목차
1. rowwise
1-1. pmax
2. slice
3. relocate
4. lag, lead

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
## 1     5     4     3
## 2     5     3    10
## 3     4     0    -1
## 4     5     6     8
## 5     4     1     4
```

```r
data(survey)
glimpse(survey)
```

```
## Rows: 237
## Columns: 12
## $ Sex    <fct> Female, Male, Male, Male, Male, Female, Male, Female, Male, ...
## $ Wr.Hnd <dbl> 18.5, 19.5, 18.0, 18.8, 20.0, 18.0, 17.7, 17.0, 20.0, 18.5, ...
## $ NW.Hnd <dbl> 18.0, 20.5, 13.3, 18.9, 20.0, 17.7, 17.7, 17.3, 19.5, 18.5, ...
## $ W.Hnd  <fct> Right, Left, Right, Right, Right, Right, Right, Right, Right...
## $ Fold   <fct> R on L, R on L, L on R, R on L, Neither, L on R, L on R, R o...
## $ Pulse  <int> 92, 104, 87, NA, 35, 64, 83, 74, 72, 90, 80, 68, NA, 66, 60,...
## $ Clap   <fct> Left, Left, Neither, Neither, Right, Right, Right, Right, Ri...
## $ Exer   <fct> Some, None, None, None, Some, Some, Freq, Freq, Some, Some, ...
## $ Smoke  <fct> Never, Regul, Occas, Never, Never, Never, Never, Never, Neve...
## $ Height <dbl> 173.00, 177.80, NA, 160.00, 165.00, 172.72, 182.88, 157.00, ...
## $ M.I    <fct> Metric, Imperial, NA, Metric, Metric, Imperial, Imperial, Me...
## $ Age    <dbl> 18.250, 17.583, 16.917, 20.333, 23.667, 21.000, 18.833, 35.8...
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
## 1     5     4     3     5
## 2     5     3    10    10
## 3     4     0    -1     4
## 4     5     6     8     8
## 5     4     1     4     4
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
## 1     5     4     3    10
## 2     5     3    10    10
## 3     4     0    -1    10
## 4     5     6     8    10
## 5     4     1     4    10
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
## 1     5     4     3     5
## 2     5     3    10    10
## 3     4     0    -1     4
## 4     5     6     8     8
## 5     4     1     4     4
```
{{< /expand >}}

#### 2. slice()
행 선택
{{<expand "slice 예시">}}

```r
# choose 10 rows with smallest 10 pulse values.
survey %>%
  slice_min(Pulse, n = 10)
```

```
##       Sex Wr.Hnd NW.Hnd W.Hnd    Fold Pulse    Clap Exer Smoke Height      M.I
## 1    Male   20.0   20.0 Right Neither    35   Right Some Never 165.00   Metric
## 2  Female   16.5   17.0 Right  L on R    40    Left Freq Never 167.64 Imperial
## 3  Female   18.0   17.5 Right  R on L    48 Neither Freq Never 165.00   Metric
## 4    Male   21.0   21.0 Right  L on R    48 Neither Freq Never 174.00   Metric
## 5  Female   18.0   17.9 Right  R on L    50    Left None Never 165.00   Metric
## 6  Female   15.5   15.5 Right Neither    50   Right Some Regul     NA     <NA>
## 7    Male   18.0   19.0 Right  L on R    54 Neither Some Regul     NA     <NA>
## 8    Male   22.0   21.5  Left  R on L    55    Left Freq Never 200.00   Metric
## 9    Male   20.5   19.5 Right  L on R    56   Right Freq Never 179.00   Metric
## 10   Male   19.8   20.0  Left  L on R    59   Right Freq Never 180.00   Metric
##       Age
## 1  23.667
## 2  17.417
## 3  18.667
## 4  21.333
## 5  30.750
## 6  18.500
## 7  17.750
## 8  18.500
## 9  17.417
## 10 17.417
```

```r
# choose 10 columns with greatest 10 pulses values.
survey %>%
  slice_max(Pulse, n = 10)
```

```
##       Sex Wr.Hnd NW.Hnd W.Hnd   Fold Pulse    Clap Exer Smoke Height      M.I
## 1    Male   19.5   20.5  Left R on L   104    Left None Regul  177.8 Imperial
## 2  Female   19.0   18.5  Left L on R   104    Left Freq Never  170.0   Metric
## 3  Female   18.5   18.0  Left L on R   100 Neither Some Never  171.0   Metric
## 4    Male   21.0   20.4 Right L on R   100   Right Freq Heavy  184.0   Metric
## 5  Female   17.5   17.5 Right R on L    98    Left Freq Never     NA     <NA>
## 6    Male   17.5   17.0  Left L on R    97 Neither None Never  165.0   Metric
## 7    Male   22.5   23.0 Right R on L    96   Right None Never  170.0   Metric
## 8    Male   21.4   21.0 Right L on R    96 Neither Some Never  180.0   Metric
## 9  Female   17.5   17.8 Right R on L    96   Right Some Never     NA     <NA>
## 10 Female   18.5   18.0 Right R on L    92    Left Some Never  173.0   Metric
## 11 Female   18.0   17.7  Left R on L    92    Left Some Never     NA     <NA>
## 12 Female   18.5   18.0 Right R on L    92   Right Freq Never  172.0   Metric
## 13 Female   18.0   18.0 Right L on R    92 Neither Freq Never  165.0   Metric
## 14 Female   16.3   16.2 Right L on R    92   Right Some Regul  152.4 Imperial
## 15   Male   20.0   19.5 Right R on L    92   Right Some Never  179.1 Imperial
##       Age
## 1  17.583
## 2  17.250
## 3  18.917
## 4  20.083
## 5  17.667
## 6  19.500
## 7  19.417
## 8  19.000
## 9  18.667
## 10 18.250
## 11 17.583
## 12 17.500
## 13 20.000
## 14 23.500
## 15 18.917
```

```r
# randomly select 10 rows.
survey %>%
  slice_sample(n = 10)
```

```
##       Sex Wr.Hnd NW.Hnd W.Hnd    Fold Pulse    Clap Exer Smoke Height    M.I
## 1    Male   18.5   18.5 Right  R on L    90   Right Some Never    167 Metric
## 2    Male   18.5   18.0 Right Neither    63 Neither Freq Never    196 Metric
## 3  Female   18.5   18.0 Right  R on L    64   Right Freq Never     NA   <NA>
## 4  Female   17.5   17.0 Right  R on L    83 Neither Freq Occas    168 Metric
## 5  Female   17.0   17.4 Right  R on L    NA Neither Some Never     NA   <NA>
## 6  Female   17.0   16.6 Right  R on L    68   Right Some Never    171 Metric
## 7  Female   19.5   18.5 Right  R on L    80   Right Some Never    170 Metric
## 8    Male   20.0   20.0 Right Neither    35   Right Some Never    165 Metric
## 9  Female   17.0   17.5 Right  R on L    64    Left Some Never     NA   <NA>
## 10   Male   20.5   20.0 Right  R on L    75    Left Some Never    183 Metric
##       Age
## 1  22.333
## 2  20.083
## 3  16.917
## 4  17.083
## 5  17.167
## 6  17.667
## 7  18.250
## 8  23.667
## 9  19.167
## 10 19.667
```

```r
# randomly select 10% of the data observations.
survey %>%
  slice_sample(prop = .05)
```

```
##       Sex Wr.Hnd NW.Hnd W.Hnd    Fold Pulse    Clap Exer Smoke Height      M.I
## 1    Male   18.5   18.0 Right  L on R    64   Right Freq Never 180.34 Imperial
## 2    Male   17.0   18.0 Right  L on R    78    Left Some Never 170.18 Imperial
## 3  Female   18.5   18.5 Right  R on L    76    Left Freq Never 175.00   Metric
## 4    Male   21.9   22.2 Right  R on L    NA   Right Some Never 187.00   Metric
## 5    Male   19.8   20.0  Left  L on R    59   Right Freq Never 180.00   Metric
## 6  Female   17.0   17.3 Right  R on L    74   Right Freq Never 157.00   Metric
## 7    Male   20.8   21.4 Right  R on L    62 Neither Freq Never 175.26 Imperial
## 8    Male   19.1   19.1 Right Neither    NA   Right Some Never 177.00   Metric
## 9  Female   17.5   17.0 Right  R on L    83 Neither Freq Occas 168.00   Metric
## 10 Female   17.0   17.6 Right  L on R    76   Right Some Never 165.00   Metric
## 11   Male   18.8   18.2 Right  L on R    78   Right Freq Never 180.00   Metric
##       Age
## 1  17.833
## 2  18.333
## 3  24.167
## 4  18.917
## 5  17.417
## 6  35.833
## 7  18.083
## 8  19.917
## 9  17.083
## 10 23.583
## 11 17.500
```
{{</expand>}}

#### 3. relocate()
relocate: changes the order of the columns.
{{<expand "relocate 예시">}}

```r
# move columns with factor variables to the front
survey %>%
  relocate(where(is.factor)) %>% colnames()
```

```
##  [1] "Sex"    "W.Hnd"  "Fold"   "Clap"   "Exer"   "Smoke"  "M.I"    "Wr.Hnd"
##  [9] "NW.Hnd" "Pulse"  "Height" "Age"
```

```r
# move Pulse before Height
survey %>%
  relocate(Pulse, .before = Height) %>% colnames()
```

```
##  [1] "Sex"    "Wr.Hnd" "NW.Hnd" "W.Hnd"  "Fold"   "Clap"   "Exer"   "Smoke" 
##  [9] "Pulse"  "Height" "M.I"    "Age"
```

```r
# move Pulse to the end
survey %>%
  relocate(Pulse, .after = last_col()) %>% colnames()
```

```
##  [1] "Sex"    "Wr.Hnd" "NW.Hnd" "W.Hnd"  "Fold"   "Clap"   "Exer"   "Smoke" 
##  [9] "Height" "M.I"    "Age"    "Pulse"
```
{{</expand>}}

#### 4. lag(), lead()
{{<expand "lag, lead 예시">}}

```r
lag(1:5)
```

```
## [1] NA  1  2  3  4
```

```r
lag(1:5, n = 2)
```

```
## [1] NA NA  1  2  3
```

```r
lag(1:5, default = 0)
```

```
## [1] 0 1 2 3 4
```

```r
lead(1:5)
```

```
## [1]  2  3  4  5 NA
```

```r
lead(1:5, default = 6)
```

```
## [1] 2 3 4 5 6
```
{{</expand>}}

#### 5. between(), near()
{{<expand "between, near 예시">}}

```r
# between: >=, <= 조건을 한번에 사용하기
between(1:12, 7, 9)
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE FALSE
```

```r
# near: ==의 안전한 버전(특히 소수점 계산시)
sqrt(2) ^ 2 == 2
```

```
## [1] FALSE
```

```r
near(sqrt(2) ^ 2, 2)
```

```
## [1] TRUE
```
{{</expand>}}

###### 참고
[1] slice와 relocate 예시는 slack `슬기로운통계생활`을 참고하였습니다.
