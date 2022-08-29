---
collapsible: false
date: "2022-08-14T10:08:56+09:00"
description: Imbalanced Dataset
title: Imbalanced Dataset
weight: 2
---

# Imbalanced

## 1. Undersampling

### 1-1. Random Undersampling
말그래돌 다수 데이터를 소수 데이터의 개수에 맞추어 랜덤으로 적게 뽑는 방식이다. 다수 클래스의 정보가 소수 클래스의 정보를 압도하는 것을 막기 위함이다.

### 1-2. Tomek's Link
가장 가까운 데이터가 다른 클래스일 경우, 한 쌍으로 묶고 이를 토멕 링크라고 부른다. 그중에서 다수 클래스에 속한 데이터를 제거하는 방식이다.

### 1-3. CNN (Condensed Nearest Neighbors)
1. 소수 클래스 데이터는 그대로 유지한다.
1. 다수 클래스 데이터는, 1NN(KNN에서 K를 1로 설정한 것)에서 NN이 소수 클래스에 속하면 남기고 아니면 제거한다.

### 1-4. One Sided Selection
[Tomek's Link](/posts/machinelearning/imbalanced/#1-2-tomeks-link) + [CNN(Condensed Nearest Neighbors)](/posts/machinelearning/imbalanced/#1-3-cnn-condensed-nearest-neighbors)  
Tomek's Link를 먼저 적용하고 CNN을 이어서 적용하는 방법이다.

### 1-5. ENN (Edited Nearest Neighbors)
다수 클래스 데이터 중에서 K개의 NN이 다수 클래스이면 삭제하는 방식이다. 단, K개의 NN이 모두 다수 클래스일 때 삭제하는 방법(`kind_sel="all"`)이 있고, 과반수 이상일 경우 삭제하는 방법(`kind_sel="mode"`)이 있다.

### 1-6. Neighborhood Cleaning Rule
[CNN(Condensed Nearest Neighbors)](/posts/machinelearning/imbalanced/#1-3-cnn-condensed-nearest-neighbors) + [ENN(Edited Nearest Neighbors)](/posts/machinelearning/imbalanced/#1-5-enn-edited-nearest-neighbors)

## 2. Oversampling

### 2-1. Random Oversampling
소수 데이터를 랜덤으로 복제하여 다수 데이터와 개수를 맞추는 방식이다.

### 2-2. ADASYN
ADASYN은 `Adaptive Synthetic Sampling`의 약자이다. 소수 데이터 중에서 KNN을 골라서 직선상의 데이터를 생성하는 방식이다.

### 2-3. SMOTE
SMOTE는 `Synthetic Minority Oversampling Technique`의 약자이다. ADASYN처럼 소수 클래스 데이터를 활용하여 데이터를 생성하지만, 대신에 무조건 소수 데이터라고 하지 않고 분류 모형에 따라 클래스를 구분한다는 것이 차이점이다.

## 3. Hybrid

### 3-1. SMOTE + ENN

### 3-2. SMOTE + Tomek's Link

# ---

위 방법론들은 모두 python 라이브러리 중에서 `imblearn`에서 모두 구현이 되어있다.

# ---

## Reference
[1] [참고사이트](https://datascienceschool.net/03%20machine%20learning/14.02%20%EB%B9%84%EB%8C%80%EC%B9%AD%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EB%AC%B8%EC%A0%9C.html#smote-enn)
