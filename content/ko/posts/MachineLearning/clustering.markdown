---
collapsible: false
date: "2021-10-28T10:08:56+09:00"
description: Clustering
title: Clustering
weight: 3
---

# Clusetring

주어진 데이터의 특성을 고려해서 데이터 집단을 정의하고, 데이터 집단을 대표할 수 있는 대표점을 찾는 과정이다.

## 1. K-Means Clustering
![Kmeans](images/posts/machine_learning/kmeansClustering.gif)

Step 1. 클래스 개수 결정 & 중심점 무작위 선택
Step 2. 가장 가까운 클래스에 데이터를 배정
Step 3. 클래스 중심점 재계산
Step 4. 수렴할 때까지 Step 2~3 반복

{{<expand "장단점">}}
단점 1. 클래스 개수를 미리 결정하여야 한다.
단점 2. 이상치들로 인해 평균값을 기준으로 하는 것이 옳지 않을 수 있다. 이때는 중앙값을 사용하는 K-Medians를 활용해볼 수 있다.
{{</expand>}}

## 2. Mean-Shift Clustering
![MeanShift1](images/posts/machine_learning/meanShiftClustering1.gif)

높은 밀도를 보이는 지역으로 sliding window를 옮겨가는 hill climbing 알고리즘이다.
![MeanShift2](images/posts/machine_learning/meanShiftClustering2.gif)

{{<expand "장단점">}}
장점 1. 클래스 개수를 미리 결정할 필요가 없다.
단점 1. kernel = 반지름 r 사이즈를 선택해야 한다.
{{</expand>}}

## 3. DBSCAN
Density-Based Spatial Clustering of Applications with Noise
![DBSCAN](images/posts/machine_learning/DBSCAN.gif)

{{<expand "장단점">}}
장점 1. 클래스 개수를 미리 결정할 필요가 없다.
단점 1. 별로 잘 작동하지 않는다.
{{</expand>}}

## 4. EM Clusteinrg with GMM
Expectation-Maximization using Gausian Mixture Models
![EMwithGMM](images/posts/machine_learning/EMwithGMM.gif)

Step 1. K-means와 같이 클러스터 개수를 정하고,각 클러스터의 가우시안 분포 모수를 정한다.
Step 2. 각 데이터가 특정 클러스터에 속할 확률을 계산한다.
Step 3. Step 2에서 계산한 확률(likelihood)에 근거하여 이를 최대화하는 가우시안 분포의 새로운 모수를 계산한다.

{{<expand "장단점">}}
장점 1. K-Means보다 유연하다.
장점 2. 각 데이터는 여러 클러스터를 가질 수 있지만, 확률을 계산하여 판단할 수 있다.
{{</expand>}}

## 5. Agglomerative Hierarchical Clustering
![HAC](images/posts/machine_learning/agglomerativeHierarchicalClustering.gif)

Step 1. 각 데이터를 각각의 클러스터로 본다. 
Step 2. 평균 간의 거리를 통해서 두 클러스터를 하나로 합친다. 
Step 3. 나무의 뿌리가 만들어질 때까지 Step2를 계속한다. 또는 원하는 개수의 클러스터 개수가 되면 멈춘다.

{{<expand "장단점">}}
장점 1. 클래스 수를 미리 결정하지 않으며, 오히려 원하는 클래스 개수에 따라 정할 수 있다.
장점 2. 데이터의 계층적 구조를 잘 반영한다.
장점 3. 불균형데이터에 대해 좋다(?, [5])
단점 1. K-Means나 GMM에 비해 계산량이 크다.
{{</expand>}}

## 6. Deep Clustering for Unsupervised Learning of Visual Features
unsupervised learning 모델(k-means)에서 나오는 pseudo label(cluster index)를 pre-training모델에 fine-tuning을 시킨다.

![DeepClustering](images/posts/machine_learning/deep_clustering.PNG)

### 6-1. Main
1. Conv Top layer를 이용해 클러스터링 알고리즘(K-Means)을 사용
2. Pseudo Label를 생성해 Fine-Tuning

### 6-2. Detail
1. Sobel filter를 통해 edge 검출
2. Feature map 차원 축소 (PCA)
3. Pseudo Label를 생성할 때 균등 샘플링

## 참고자료
[1] https://zinniastop.blogspot.com/2019/10/5.html
[2] https://astralworld58.tistory.com/58
[3] https://www.youtube.com/watch?v=cCwzxVwfrgM
[4] http://dsba.korea.ac.kr/seminar/?mod=document&uid=28
[5] https://towardsdatascience.com/clustering-analyses-with-highly-imbalanced-datasets-27e486cd82a4
