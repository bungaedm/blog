---
date: "2021-03-04T10:08:56+09:00"
description: null
draft: false
title: Bayesian Linear Regression
weight: 9
---

## Chapter 09. <br> The Multivariate Normal Model
본 포스팅은 **First Course in Bayesian Statistical Methods**를 참고하였다.

### 1. Linear Regression Model
### 2. Bayesian estimation for a regression model
### 3. Model Selection


```r
library(tidyverse)
```

{{<expand "Code Example">}}

```r
## Data load 
data = dget('https://www2.stat.duke.edu/~pdh10/FCBS/Inline/yX.o2uptake')
y = data[,1]
X = data[,-1]
inv = solve

## set prior
g = length(y)
nu0 = 1
s20 = summary(lm(y~-1+X))$sigma^2
n = length(y)
p = ncol(X)

## MCMC setup
S = 1000
set.seed(2021)
BETA = matrix(NA, nrow=S, ncol=p)
sigma2 = matrix(NA, nrow=S, ncol=1)
BETA[1,] = inv(t(X) %*% X) %*% t(X) %*% y
sigma2[1,] = s20

## gibbs sampling
nun = nu0 + n
betan = (g/(g+1)) * inv(t(X) %*% X) %*% t(X) %*% y
for(s in 2:S){
  s2n = nu0*s20 + t(y-X%*%BETA[s-1,]) %*% (y-X%*%BETA[s-1,])
  sigma2[s,] = 1/rgamma(1, shape=nun/2, rate=s2n/2)
  
  Sigman = (g/(g+1)) * sigma2[s,] * inv(t(X) %*% X)
  BETA[s,] = MASS::mvrnorm(n=1, betan, Sigman)
}

## graph
colnames(BETA) = colnames(X)
gather(as.data.frame(BETA)) %>% 
  ggplot(aes(y=value, fill=key)) + geom_histogram() +
  coord_flip() + facet_wrap(~key, scales='free_x') +
  ggtitle('Posterior samples of Beta') +
  theme(legend.position = 'None')
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

<img src="/ko/posts/Statistics/Bayesian/fcb09_files/figure-html/unnamed-chunk-2-1.png" width="672" />
{{</expand>}}
