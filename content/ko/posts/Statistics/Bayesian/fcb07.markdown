---
date: "2021-01-31T10:08:56+09:00"
description: null
draft: false
title: MVN
weight: 7
---

## Chapter 07. <br> The Multivariate Normal Model
본 포스팅은 **First Course in Bayesian Statistical Methods**를 참고하였다.

### 1. Multivariate Normal Desity
### 2. Semiconjugate prior distribution for the mean
### 3. inverse-Wishart Distribution

### 4. Gibbs Sampling of the mean and covariance
#### 4-1. Draw yourself Figure 7.1

```r
library(tidyverse)
library(gridExtra)
library(MASS)
library(reshape2)
library(ash)
```



```r
# 초기 설정
inv <- solve
MU = matrix(c(50,50), ncol=1)
SIGMA = matrix(c(64,0,0,144), ncol=2)

# MVN pdf
calc.dmvn = Vectorize(function(a,b, mu=MU, sigma=SIGMA){
  y <- c(a,b)
  log.p <- (-nrow(mu)/2)*log(2*pi) - 0.5*log(det(sigma)) - 0.5*(t(y-mu) %*% inv(sigma) %*% (y-mu))
  exp(log.p)
})
```

```r
# do it at once
allInOne <- function(corr){
  SIGMA = matrix(c(64,0,0,144), ncol=2)
  s1 <- sqrt(SIGMA[1,1]); s2 <- sqrt(SIGMA[2,2])
  SIGMA[1,2] <-  s1*s2*corr; SIGMA[2,1] <-  s1*s2*corr
  
  # MVN density function
  calc.dmvn = Vectorize(function(a,b, mu=MU, sigma=SIGMA){
    y <- c(a,b)
    log.p <- (-nrow(mu)/2)*log(2*pi) - 0.5*log(det(sigma)) - 0.5*(t(y-mu) %*% inv(sigma) %*% (y-mu))
    exp(log.p)
  })
  
  # sample
  sample = mvrnorm(n=30, mu=MU, Sigma=SIGMA)
  sample = data.frame(sample)
  colnames(sample) = c('y1','y2')
  
  # calculate density
  xLim = seq(20, 80, length=101)
  yLim = seq(20, 80, length=101)
  density.mvn <- outer(xLim, yLim, FUN=calc.dmvn)
  rownames(density.mvn) <- xLim
  colnames(density.mvn) <- yLim
  density.mvn <- melt(density.mvn)
  
  # graph
  density.mvn %>% 
    ggplot(aes(x=Var1, y=Var2)) +
    geom_tile(aes(fill=value, alpha=value)) +
    geom_contour(aes(z=value), color='white', size=0.1) +
    geom_point(data=sample, mapping=aes(x=y1, y=y2, color='red'), show.legend=FALSE) +
    scale_fill_gradient(low='grey', high='steelblue', guide=FALSE) +
    scale_alpha(guide=FALSE) +
    theme(legend.position='None') + theme_bw() +
    ggtitle(paste0('corr=',corr)) + xlab('y1') + ylab('y2')
}
```


```r
p1 <- allInOne(corr=-0.5)
p2 <- allInOne(corr=0)
p3 <- allInOne(corr=0.5)
grid.arrange(p1,p2,p3, nrow=1)
```

<img src="/ko/posts/Statistics/Bayesian/fcb07_files/figure-html/unnamed-chunk-4-1.png" width="672" />

#### 4-2. Draw yourself Figure 7.2



```r
# Load Data
test <- matrix(c(59, 43, 34, 32, 42, 38, 55, 67, 64, 45, 49, 72, 34, 
                 70, 34, 50, 41, 52, 60, 34, 28, 35, 77, 39, 46, 26, 38, 43, 68, 
                 86, 77, 60, 50, 59, 38, 48, 55, 58, 54, 60, 75, 47, 48, 33), ncol=2, byrow=FALSE)
colnames(test) <- c('pretest','posttest')

# Preparing
n <- nrow(test)
ybar <- colMeans(test)
Sigma <- cov(test)
THETA <- NULL
SIGMA <- NULL
inv <- solve
sample.size = 5000
sample.new = NULL

# prior
mu0 <- c(50,50); nu0 <- 4 #(nu0 = p+2 = 4) 
S0 <- L0 <- matrix(c(625,312.5,312.5,625), nrow=2, ncol=2)

set.seed(2021)
for(i in 1:sample.size){
  # update theta
  Ln = inv(inv(L0) + n*inv(Sigma))
  mun = Ln %*% (inv(L0)%*%mu0 + n*inv(Sigma)%*%ybar)
  theta = mvrnorm(1, mun, Ln)
  
  # update sigma
  Sn = S0 + (t(test)-theta)%*%t(t(test)-theta)
  Sigma = inv(rWishart(1, nu0+n, inv(Sn))[,,1])
  
  # Save results
  THETA <- rbind(THETA, theta)
  SIGMA <- rbind(SIGMA, c(Sigma))
  
  # sample new
  sample.new = rbind(sample.new, mvrnorm(n=1, mu=theta, Sigma=Sigma))
}
rownames(THETA) <- 1:sample.size
rownames(SIGMA) <- 1:sample.size
```


```r
# graph(코드 따라하기)
par(mfrow=c(1,2),mgp=c(1.75,.75,0),mar=c(3,3,1,1))

plot.hdr2d(THETA,xlab=expression(theta[1]),ylab=expression(theta[2]) )
abline(0,1)

plot.hdr2d(sample.new,xlab=expression(italic(y[1])),ylab=expression(italic(y[2])), 
           xlim=c(0,100),ylim=c(0,100) )
points(test[,1],test[,2],pch=16,cex=.7)
abline(0,1)
```

<img src="/ko/posts/Statistics/Bayesian/fcb07_files/figure-html/graph-1.png" width="672" />


```r
# graph(ggplot 활용)
p1 <- data.frame(THETA) %>% 
  ggplot(aes(x=pretest, y=posttest)) +
  geom_point(size=1, color='orange') +
  geom_abline(slope=1, intercept=0) +
  xlab(expression(theta[1])) + ylab(expression(theta[2])) +
  ggtitle('Posterior draws of Mu')

p2 <- data.frame(sample.new) %>% 
  ggplot(aes(x=pretest, y=posttest)) +
  geom_point(size=1, color='orange') +
  geom_abline(slope=1, intercept=0) +
  xlab(expression(y[1])) + ylab(expression(y[2])) +
  ggtitle('Posterior Predictive')

grid.arrange(p1, p2, nrow=1)
```

<img src="/ko/posts/Statistics/Bayesian/fcb07_files/figure-html/graph2-1.png" width="672" />
