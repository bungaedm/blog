---
collapsible: false
date: "2021-03-11T10:08:56+09:00"
description: 머신러닝 개괄
title: ML Intro
weight: 1
---

## Machine Learning
- 주어진 데이터를 통해서 입력변수와 출력변수 간의 관계를 만드는 함수 `$f$`를 만드는 것
- 주어진 데이터 속에서 데이터의 특징을 찾아내는 함수 `$f$`를 만드는 것

### 1. 기본 개념구분
- 지도 학습: 회귀(Regression), 분류(Classification)
- 비지도 학습: PCA, 군집분석
- 강화 학습: 수많은 시뮬레이션을 통해 현재의 선택이 먼 미래에 보상이 최대로 하는 action을 학습

### 2. 다양한 머신러닝 기법
1. 선형회귀분석: 선형관계를 가정하여, 독립변수의 중요도와 영향력 파악
2. DT(Decision Tree): 독립변수의 조건에 따라 종속변수를 분리
3. KNN(K-Nearest Neighbor): 새로 들어온 데이터의 주변 K개의 데이터의 class로 분류
4. NN(Neural Network): 입력층/은닉층/출력층 으로 구성된 모형. 각 층을 연결하는 노드의 가중치를 업데이트하며 학습
5. SVM(Support Vector Machine): class 간 거리가 최대가 되도록 decision boundary 만드는 방법
6. K-means Clustering: Label 없이 데이터의 군집 k개 생성
7. Ensemble Learning: 여러 개의 모델을 결합하여 사용하는 모델로, 구체적으로는 다양한 알고리즘 종류가 있다.
  7-1. Bagging: 모델을 다양하게 만들기 위해 데이터를 재구성
  7-2. Random Forest: 모델을 다양하게 만들기 위해 데이터뿐만 아니라 변수도 재구성
  7-3. Boosting: 맞추기 어려운 데이터에 대해 좀 더 가중치를 두어 seqeuntial하게 학습하는 개념 (ex. AdaBoost, Gradient Boosting(Xgboost, LightGBM, CatBoost)
  7-4. Stacking: 모델의 output값을 새로운 독립변수로 활용
8. Deep Learning: 딥러닝은 사실 머신러닝의 부분집합이다. 하지만 워낙 깊고 다양하기에 따로 다루도록 하겠다.

### 3. 모형의 적합성 평가 및 실험설계
데이터를 Training-Validation-Test, 총 세 가지 세트로 나눈다.

#### K-Fold Cross Validation
데이터를 k개 부분으로 나누 뒤, 하나를 검증집합 나머지를 학습집합으로 한다. 이 과정을 k번 반복해서 k개의 성능지표를 구하고 그것들의 평균을 구한다.

#### LOOCV(Leave One Out Cross Validation)
데이터를 k개의 부분으로 나누기에 부족할 때, 데이터 한 개씩을 빼가면서 K-fold CV를 하는 방식과 똑같이 한다.

### 4. 과적합(Overfitting)
머신러닝에서 가장 주의해야 할 것 중 하나가 바로 과적합이다. 이와 관련해서는 [Bias-Variance Tradeoff](/posts/statistics/statistics/bias_variance/)에 대한 이해가 필요하다. 그리고 아주 간단하게 이해하기 위해서는 아래 두 사진을 참고하면 될 것이다.

![overfitting1](images/posts/machine_learning/overfitting1.png)
![overfitting2](images/posts/machine_learning/overfitting2.png)

###### 참고
[1] https://medium.com/@cs.sabaribalaji/overfitting-6c1cd9af589
[2] https://www.researchgate.net/figure/The-overfitting-of-model-a-training-error-and-true-error-b-depiction-of-Eq-33_fig5_333505702