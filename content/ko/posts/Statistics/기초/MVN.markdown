---
collapsible: false
date: "2021-01-08T10:08:56+09:00"
description: 다변량 정규분포
draft: false
title: MVN
weight: 1
---



# 1. Drawing MVN plots with ggplot2

```r
mu = matrix(c(0,10), ncol=1) 
invSig = solve(matrix(c(4,10,10,100), ncol=2, byrow=TRUE))
```

## 1-1. Vectorize + Outer

```r
dmvlnorm = function(theta, mu, invSig){
  (-nrow(mu)/2) * log(2*pi) + 0.5*log(det(invSig)) - 0.5*(t(theta-mu) %*% invSig %*% (theta-mu))
}

calc.dens = Vectorize(function(a,b){
  theta = c(a,b)
  exp(dmvlnorm(theta, mu, invSig))
})

A = seq(-5, 5, length=100)
B = seq(-15, 40, length=100)
dense = outer(A, B, FUN=calc.dens)
rownames(dense) = A
colnames(dense) = B
dens = reshape2::melt(dense)
colnames(dens) = c('a','b','dens')

ggplot(data=dens, aes(x=a, y=b)) +
  geom_raster(aes(fill=dens, alpha=dens), interpolate=TRUE) +
  geom_contour(aes(z=dens), color='black', size=0.2) +
  scale_fill_gradient(low='cornflowerblue', high='steelblue', guide=FALSE) +
  scale_alpha(range=c(0,1), guide=FALSE) +
  labs(title='MVN density', x='alpha', y='beta')
```

<img src="/ko/posts/Statistics/기초/MVN_files/figure-html/unnamed-chunk-3-1.png" width="672" />

# 2. Gibbs sampling for MVN draws

```r
Y = dget('https://www2.stat.duke.edu/~pdh10/FCBS/Inline/Y.reading')
ggplot(data.frame(Y)) +
  geom_point(aes(x=pretest, y=posttest))
```

<img src="/ko/posts/Statistics/기초/MVN_files/figure-html/unnamed-chunk-4-1.png" width="672" />

## Prior specification

```r
Mu0 <- c(50,50)
Lambda0 = matrix(c(625,312.5,312.5,625), ncol=2)
nu0 = 4
S0 = (nu0-nrow(Lambda0)-1) * Lambda0
```

## Gibbs Sampler

```r
inv = solve
n = nrow(Y)
ybar = colMeans(Y)
Sigma = cov(Y) #initials
S = 5000
MU = matrix(NA, nrow=S, ncol=2)
SIGMA = matrix(NA, nrow=S, ncol=4)

for(s in 1:S){
  
  # update Mu
  Lambdan = inv(inv(Lambda0) + n*inv(Sigma))
  Mun = Lambdan %*% (inv(Lambda0) %*% Mu0 + n*inv(Sigma) %*% ybar)
  Mu = MASS::mvrnorm(n=1, Mun, Lambdan)
  
  # update Sigma
  Sn = S0 + (t(Y)-c(Mu)) %*% t((t(Y)-c(Mu)))
  Sigma = inv(rWishart(1, nu0+n, inv(Sn))[,,1])
  
  MU[s,] = Mu
  SIGMA[s,] = c(Sigma)
}

disp = tail(1:S, S/2)

p1 <- data.frame(mu1=MU[disp,1], mu2=MU[disp,2]) %>% 
  ggplot(aes(x=mu1, y=mu2)) + geom_point(size=0.5, color='steelblue') +
  geom_abline(slope=1, intercept=0) +
  coord_fixed(ratio=1) +
  ggtitle('Posterior darws of MU')

meandiff = MU[disp,2] - MU[disp,1]
p2 <- data.frame(meandiff=meandiff) %>% 
  ggplot(aes(x=meandiff)) +
  geom_histogram(color='white', fill='steelblue', bins=30) +
  geom_vline(xintercept=0) +
  ggtitle('Posterior draws of Mu2 - Mu1')

grid.arrange(p1, p2, ncol=2)
```

<img src="/ko/posts/Statistics/기초/MVN_files/figure-html/unnamed-chunk-6-1.png" width="672" />

# 3. Gibbs Sampling for NA imputation

```r
Y = dget('https://www2.stat.duke.edu/~pdh10/FCBS/Inline/Y.pima.miss')
head(Y)
```

```
##   glu bp skin  bmi
## 1  86 68   28 30.2
## 2 195 70   33   NA
## 3  77 82   NA 35.8
## 4  NA 76   43 47.9
## 5 107 60   NA   NA
## 6  97 76   27   NA
```


```r
psych::pairs.panels(Y, method='pearson', density=T, breaks=20, hist.col='steelblue')
```

<img src="/ko/posts/Statistics/기초/MVN_files/figure-html/unnamed-chunk-8-1.png" width="672" />


```r
# priors
n = nrow(Y)
p = ncol(Y)
Mu0 = c(120,64,26,26)
sd0 = Mu0/2
L0 = matrix(0.1, p, p)
diag(L0) = 1
L0 = L0*outer(sd0,sd0)
nu0 = p+2
S0 = (nu0-p-1)*L0

Sigma = S0
Y.full = Y
O = 1*(!is.na(Y))
for(j in 1:p){
  Y.full[is.na(Y.full)[,j], j] = mean(Y.full[,j], na.rm=TRUE) #mean imputation
}
inv = solve
S = 100

for(s in 1:S){
  # update Mu
  ybar = colMeans(Y.full)
  Ln = inv(inv(L0) + n*inv(Sigma))
  Mun = Ln %*% (inv(L0) %*% Mu0 + n*inv(Sigma) %*% ybar)
  Mu = MASS::mvrnorm(n=1, Mun, Ln)
  
  # update Sigma
  Sn = S0 + (t(Y.full) - c(Mu)) %*% t((t(Y.full) - c(Mu)))
  Sigma = inv(rWishart(1, nu0+n, inv(Sn))[,,1])
  
  # update missing data
  for(i in 1:n){ #iterate over rows
    b = (O[i,] == 0) #missing at each row
    a = (O[i,] == 1) #observed at each row
    if(sum(b)==0) next
    iSa = inv(Sigma[a,a])
    beta.j = Sigma[b,a] %*% iSa
    Sigma.j = Sigma[b,b] - Sigma[b,a] %*% iSa %*% Sigma[a,b]
    Mu.j = Mu[b] + beta.j %*% (t(Y.full[i,a]) - Mu[a])
    Y.full[i,b] = MASS::mvrnorm(1, Mu.j, Sigma.j)
  }
  if(s%% 10 == 1) cat(s, '\t')
}
```

```
## 1 	11 	21 	31 	41 	51 	61 	71 	81 	91 	
```


```r
colSums(is.na(Y))
```

```
##  glu   bp skin  bmi 
##   15   23   25   22
```

```r
colSums(is.na(Y.full))
```

```
##  glu   bp skin  bmi 
##    0    0    0    0
```



