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
### 불균형데이터의 정의
일반적으로 두 개의 클래스가 있는 상황에서, 한 클래스에 속한 원소가 나머지 클래스에 속한 원소에 비해 월등하게 많은 경우를 데이터가 불균형한 상황이라고 정의한다.
$$IR(Imbalance Ratio) = \frac{\text{# of Majority class}}{\text{# of Minority class}}$$

## 2. Related Work
### 2-1. Data Level Methods
데이터 자체를 건드려서 해결하는 방식을 Data Level Method라고 칭한다. 본 논문에서 이야기하는 대표적인 예시로는 Balanced Random Forest(BRF)가 있다. 이는 다수 클래스에 속하는 것들을 적게 뽑는(undersampling) 방식이다. 이외에 SMOTE와 같은 oversampling 기법들도 있고, undersampling과 oversampling이 결합된 하이브리드 방식도 있다.

### 2-2. Algorithmic Level Methods


### 2-3. Bayes Rule 
$$\delta_B(x) = I{p(x) \be 1/2}$$

## 3. Quantile Classifier
- Densitiy-based approach
- q*-classifier
- Response-based sampling 
- q*-classifier is invariant to response-based sampling

## 4. Performance
- G-mean
- TNR+TPR optimal

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
