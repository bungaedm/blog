---
author: 손지우
date: "2021-10-03"
tags: null
title: A Random Forests Quantile Classifier for Class Imbalanced Data
tags:
- Statistics
- Machine Learning
---

O’Brien, R., & Ishwaran, H. (2019). A random forests quantile classifier for class imbalanced data. Pattern recognition, 90, 232-249.
<!--more-->

## In Short
불균형데이터 처리를 위해, quantile classifier을 사용한 Random Forest

## 1. Introduction
### 1-1. 불균형데이터의 정의
일반적으로 두 개의 클래스가 있는 상황에서, 한 클래스에 속한 원소가 나머지 클래스에 속한 원소에 비해 월등하게 많은 경우를 데이터가 불균형한 상황이라고 정의한다. (여기서는 `$Y=1$`이 `Minoritiy`, `$Y=0$`이 `Majority`라고 생각하자.)

5개의 근접원소들에 대해서 Majority 클래스에 속하는 원소가 0~1개인 원소를 `Safe`, 2~3개는 `Borderline`, 4~5개는 `Rare`라고 부른다.

### 1-2. IR (Imbalance Ratio)
$$IR = \frac{\text{# of Majority class}}{\text{# of Minority class}}$$

### 1-3. Marginally imbalanced
정의: `$p(x) \ll \frac{1}{2} \text{ for all } x \in X \text{ where } p(x) = P(Y=1|X=x)$`

### 1-4. Conditionally imbalanced
정의: `$\text{there exists a set } A \subset X \text{ with nonzero probability, } P(X \in A) >0, \text{ such that } P(Y=1|X \in A) \approx 1 \text{ and } p(x) \ll \frac{1}{2} \text{ for } x \notin A$`

### 1-5. Notation 정리
아래는 본 논문의 Table 1이다.
![Notation](images/posts/paper/imbalanced_randomforest_table1.PNG)

## 2. Related Work
### 2-1. Data Level Methods
데이터 자체를 건드려서 해결하는 방식을 Data Level Method라고 칭한다. 본 논문에서 이야기하는 대표적인 예시로는 `Balanced Random Forest(BRF)`가 있다. 이는 다수 클래스에 속하는 것들을 적게 뽑는(undersampling) 방식이다. 이외에 SMOTE와 같은 oversampling 기법들도 있고, undersampling과 oversampling이 결합된 하이브리드 방식도 있다. 아래는 해당 논문에서 추가적으로 언급된 방법론들이다.
- One-sided Sampling: Tomek Links
- Neighborhood Balanced Bagging
- SMOTEBoost, RUSBoost, EUSBoost: combine boosting with sampling data at each boosting iteration

### 2-2. Algorithmic Level Methods
위처럼 데이터의 균형을 직접적으로 조절하는 방식이 있는가하면, 알고리즘적으로 분류 성능을 높이고자 하는 노력들도 있었다. 아래는 다양한 방법론들 예시이다.
- SHRINK
- Helling Distance Decision Trees(HDDT)
- Near-Bayesian Support Vector Machines(NBSVM)
- Class Switching according to NEarest Enemy Distance

### 2-3. Bayes Decision Rule 
$$\delta_B(x) = I\big( p(x) \geq 1/2 \big)$$
참고로 여기서 `$p(x) = P(Y=1 | X=x)$`이다. 이는 IR이 커지면 문제가 된다. `$p(x)$`가 0에 가까우면 해당 classifier는 Majority 클래스로 예측하게 되는데, 일반적으로 다수의 원소가 속해있는 클래스로 예측하도록 `$p(x)$`가 0에 가깝게 되는 경우가 많기 때문이다.

## 3. Q*-Classifier
### 3-1. Quantile classifier
$$\delta_q(x) = I\big( p(x) \geq q \big), \ 0<q<1$$
quantile classifer가 무엇인지 이해하면, 해당 논문의 핵심 포인트인 q*-classifier을 이해할 수 있다. 이는 알고리즘적으로 데이터 불균형 문제를 해결하고자 하는 축에 속하며, Density-based approach라고 할 수 있다.

해당 방법론은 크게 두 가지 장점이 있다. 첫번째는 **TPR과 TNR을 최대화한다**는 점이다. 두번째는 cost-weighted Bayes classifier과 같이 작동함으로써 **weighted risk를 최소화**해준다.

$$r(\delta, \ell_0, \ell_1) = E\Big[\ell_{0}1_{(\hat{\delta}(X)=1, Y=0)} + \ell_{1}1_{(\hat{\delta}(X)=0, Y=1)}\Big]$$

여기서 `$\ell_0$`와 `$\ell_1$`은 각각 Majority 원소 또는 Minority 원소를 잘못 분류할 때의 cost이며, 모두 양수이다.

cost-weighted risk의 관점에서 보면, 최적의 classifier는 cost-weighted Bayes rule을 활용하는 것인데, 이는 아래와 같이 나타낼 수 있다.
$$\delta_{WB}(x) = 1_{\big(p(x) \geq \frac{\ell_0}{\ell_0 + \ell_1}\big)}$$

이것이 최적인 이유는 모든 분류기에 대해서 `$r(\delta_{WB}, \ell_0, \ell_1) \leq r(\hat{\delta}, \ell_0, \ell_1)$`를 만족하며, 그 리스크가 아래를 만족하기 때문이다.
$$r(\delta_{WB}, \ell_0, \ell_1) = E\Big[min\Big(\ell_1p(X), \ell_0(1-p(X))\Big)\Big]$$

- Density-based approach
$$\delta_D(x) = 1_{\big(f_{X|Y}(x|1) \geq f_{X|Y}(x|0)\big)}$$
여기서 주목해야 할 점은 conditional density of the response 가 아니라 conditional density of the features(`$f_{X|Y}$`)를 활용했다는 점이다. 이로 인해 소수 클래스의 prevalance 효과를 제거할 수 있습니다.

- Response-based sampling
- q*-classifier is invariant to response-based sampling

## 4. Performance
- G-mean
- TNR+TPR optimal
TNR(True Negative Rate)와 TPR(True Positive Rate)의 합을 최대화시켜주는 분류기를 `TNR+TPR optimal`이라고 부른다.


## 5. Comparison to BRF
- Why RFQ is better
- ex1) Simulated data
- ex2) Cognitive impairment data
- ex3) Customer churn data

## 6. Variable Importance
- Breiman-Culter importance(tree-based) : not fit
- G-mean with Ishwaran-Kogalur importance(ensemble) : do fit

## 7. Multiclass Imbalanced Data
- ex1) Waveform simulations
- ex2) Cassini simulations

## 8. Comparison to Boosting

## 9. Discussion

## 10. Further Reference
불균형데이터에 대해서 알고 싶다면 아래의 세 논문을 추가 참고해보면 좋을 것 같다.
1. Krawczyk, B. (2016). Learning from imbalanced data: open challenges and future directions. Progress in Artificial Intelligence, 5(4), 221-232.
2. Haixiang, G., Yijing, L., Shang, J., Mingyun, G., Yuanyue, H., & Bing, G. (2017). Learning from class-imbalanced data: Review of methods and applications. Expert Systems with Applications, 73, 220-239.
3. Das, S., Datta, S., & Chaudhuri, B. B. (2018). Handling data irregularities in classification: Foundations, trends, and future challenges. Pattern Recognition, 81, 674-693.

이 논문에 대해서 영어로 정리된 [깃헙 페이지](https://luminwin.github.io/randomForestSRC/articles/imbalance.html)가 있다. 
