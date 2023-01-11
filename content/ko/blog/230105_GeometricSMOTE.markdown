---
author: 손지우
date: "2023-01-04"
tags: null
title: Geometric SMOTE
---

Douzas, G., & Bacao, F. (2019). Geometric SMOTE a geometrically enhanced drop-in replacement for SMOTE. Information Sciences, 501, 118-135. <!--more-->

## In Short

## 1. Introduction
데이터불균형 문제는 언제나 중요한 문제이다.

## 2. Related Work

### 2-1. Modifications of the selection phase
데이터 불균형 문제는 게 두 가지로 나눠서 생각해볼 수 있다. 하나는 **between-class 불균형**, 나머지 하나는 **within-class 불균형**이다. 여기서 between-class 불균형은 기존에 우리가 알고 있던 majority와 minority의 극명한 빈도수 차이를 뜻하며, within-class 불균형은 같은 클래스 안에서도 세부 클래스로 나뉠 수 있다는 가능성에 대해 초점을 맞추고 있다. 

1. **between-class 불균형**
SMOTE와 ENN(Edited Nearest Neighbor)을 결합한 `SMOTE+ENN`모델은 between-class 불균형 문제에 주목한 대표적인 방법론 중에서 selection phase을 변형한 방법론 중 하나이다. 이는 SMOTE를 우선 진행한 후에, ENN을 통해서 잘못 분류된 샘플들은 제거해버리는 방식이다. 이외에도 `Borderline-SMOTE`, `MWMOTE`(Majority Weighted Minority Oversampling Technique for Imbalanced Data Set Learning), `ADASYN`, `KernelADASYN은` 모두 majority와 minority의 borderline instance를 기준으로, noisy한 샘플들이 만들어지는 것을 예방하는 방식이다.

2. **within-class 불균형**
within-class 불균형을 다루는 일반적인 방법은 클러스터링과 연관이 있다. `Cluster-SMOTE`은 k-means 알고리즘을 적용한 뒤에 SMOTE를 한다. 그리고 `DBSMOTE`은 DBSCAN을 활용하여 클러스터를 분류한 뒤, 클러스터 중앙값과 그로부터 가장 가까운 minority 샘플을 활용하여 새로운 샘플들을 만들어낸다. `A-SUWO`은 cross validation을 통해 확인한 특정한 사이즈로 minority 클래스의 클러스터를 만들고나서 새로운 샘플들을 만들어낸다. `SOMO`는 input space의 2차원 representation(U-matrix)를 만들고, SMOTE를 통해서 intra-cluster와 inter-cluster 샘플들을 만들어내는 방식을 통해서 manifold structure을 보존한다. SOMO와 유사하게, Kmeans와 SMOTE를 결합하여(`SMOTE+KMeans`), 확인된 클러스터의 밀도를 토대로 클래스 분포를 re-balance하는 방식도 있다. 마지막으로, oversampling방식과 ensemble 방법을 결합한 `SMOTEBoost`와 `DataBoost-IM` 등도 있다.

### 2-2. Modifications of the data generation mechanism
위의 Selection 파트에 비해, Data generation 파트는 상대적으로 덜 연구가 된 부분이다. `Safe-Level SMOTE`는 weight degree라는 safe level이라는 개념을 제안하였다. safe level을 통해서 safe level ratio가 계산되는데, line segment를 truncate하는 효과를 지닌다. Data Generation에서 아예 SMOTE가 아닌 방법도 있는데, 대표적으로는 `CGAN(Conditional GAN)`이 있다. CGAN은 input space의 local information보다는 true data distribution을 직접적으로 근사하는 데에 초점을 둔 방법이다.

## 3. Motivation

1. **Generation of noisy instances due to the selection of k-nearest neighbors**
![figure1](images/posts/blog/gsmote/figure1.PNG)

2. **Generation of noisy examples due to the selection of an initial observation**
![figure2](images/posts/blog/gsmote/figure2.PNG)
    
3. **Generation of nearly duplicated instances**
![figure3](images/posts/blog/gsmote/figure3.PNG)

4. **Generation of noisy instances due to the use of observations from two different minority class clusters.**
![figure4](images/posts/blog/gsmote/figure4.PNG)

## 4. Proposed Method

G-SMOTE는 SMOTE에서 data generation phase를 수정한 알고리즘이다.

1.  To define a safe area around each selected minority class instance such that the generated artificial minority instances inside the are are not noisy.
2.  To increase the variety of generated samples by expanding the minority class area.
3.  To parameterize the above characteristics based on a small number of transformations with a geometrical interpretation.

### 4-1. G-SMOTE Algorithm
![gsmote](images/posts/blog/gsmote/gsmote.PNG)

### 4-2. Functions

1. `Surface`
i) if `\(\alpha_{sel} = \text{minority}\)`, `\(\boldsymbol{x}_{surface} \in S_{min,k}\)`
![figure5](images/posts/blog/gsmote/figure5.PNG)
ii) if `\(\alpha_{sel} = \text{majority}\)`, `\(\boldsymbol{x}_{surface} \in S_{maj,1}\)`
![figure6](images/posts/blog/gsmote/figure6.PNG)
iii) if `\(\alpha_{sel} = \text{combined}\)`, `\(\boldsymbol{x}_{surface} = \arg\min_{\boldsymbol{x} \in (\boldsymbol{x}_{min}, \boldsymbol{x}_{maj})}(||\boldsymbol{x}_{center} - \boldsymbol{x}||)\)` where `\(\boldsymbol{x}_{min} \in S_{min,k}\)` and `\(\boldsymbol{x}_{maj} \in S_{maj,1}\)`
![figure7](images/posts/blog/gsmote/figure7.PNG)
![figure8](images/posts/blog/gsmote/figure8.PNG)

2. `Hyperball`
`$$\boldsymbol{x}_{gen} \leftarrow r^{1/p} \boldsymbol{e}_{sphere} \\
\text{where } \boldsymbol{e}_{sphere} \leftarrow \frac{\boldsymbol{v}_{normal}}{||\boldsymbol{v}_{normal}||} \\
\boldsymbol{v}_{normal} \leftarrow (v_1, ..., v_p) \sim N(0,1) \\
r \sim (0,1)$$`
![figure9](images/posts/blog/gsmote/figure9.PNG)

3. `Vectors` 
`$$\boldsymbol{x}_{//} \leftarrow x_{//}\boldsymbol{e}_{//} \\ 
\boldsymbol{x}_{\perp} \leftarrow \boldsymbol{x}_{gen} - \boldsymbol{x}_{//} \\
\text{where } \boldsymbol{e}_{//} \leftarrow \frac{\boldsymbol{x}_{surface} - \boldsymbol{x}_{center}}{||\boldsymbol{x}_{surface} - \boldsymbol{x}_{center}||} \\
x_{//} \leftarrow \boldsymbol{x}_{gen} \cdot \boldsymbol{e}_{//}$$`

4. `Truncate` 
`$$\boldsymbol{x}_{gen} \leftarrow \boldsymbol{x}_{gen} - 2\boldsymbol{x}_{//} \\
\text{if } |\alpha_{trunc} - x_{//}| > 1$$`
![figure10](images/posts/blog/gsmote/figure10.PNG)
![figure11](images/posts/blog/gsmote/figure11.PNG)

5. `Deform` 
`$$\boldsymbol{x}_{gen} \leftarrow \boldsymbol{x}_{gen} - \alpha_{def}\boldsymbol{x}_{\perp}$$`
![figure12](images/posts/blog/gsmote/figure12.PNG)
![figure13](images/posts/blog/gsmote/figure13.PNG)

6. `Translate`
`$$\boldsymbol{x}_{gen} \leftarrow \boldsymbol{x}_{center} + R\boldsymbol{x}_{gen}$$`
![figure14](images/posts/blog/gsmote/figure14.PNG)
![figure15](images/posts/blog/gsmote/figure15.PNG)


{{<expand "Figure 없는 버전">}}
1. `Surface`
i) if `\(\alpha_{sel} = \text{minority}\)`, `\(\boldsymbol{x}_{surface} \in S_{min,k}\)`
ii) if `\(\alpha_{sel} = \text{majority}\)`, `\(\boldsymbol{x}_{surface} \in S_{maj,1}\)`
iii) if `\(\alpha_{sel} = \text{combined}\)`, `\(\boldsymbol{x}_{surface} = \arg\min_{\boldsymbol{x} \in (\boldsymbol{x}_{min}, \boldsymbol{x}_{maj})}(||\boldsymbol{x}_{center} - \boldsymbol{x}||)\)` where `\(\boldsymbol{x}_{min} \in S_{min,k}\)` and `\(\boldsymbol{x}_{maj} \in S_{maj,1}\)`

2. `Hyperball`
`$$\boldsymbol{x}_{gen} \leftarrow r^{1/p} \boldsymbol{e}_{sphere} \\
\text{where } \boldsymbol{e}_{sphere} \leftarrow \frac{\boldsymbol{v}_{normal}}{||\boldsymbol{v}_{normal}||} \\
\boldsymbol{v}_{normal} \leftarrow (v_1, ..., v_p) \sim N(0,1) \\
r \sim (0,1)$$`

3. `Vectors` 
`$$\boldsymbol{x}_{//} \leftarrow x_{//}\boldsymbol{e}_{//} \\ 
\boldsymbol{x}_{\perp} \leftarrow \boldsymbol{x}_{gen} - \boldsymbol{x}_{//} \\
\text{where } \boldsymbol{e}_{//} \leftarrow \frac{\boldsymbol{x}_{surface} - \boldsymbol{x}_{center}}{||\boldsymbol{x}_{surface} - \boldsymbol{x}_{center}||} \\
x_{//} \leftarrow \boldsymbol{x}_{gen} \cdot \boldsymbol{e}_{//}$$`

4. `Truncate` 
`$$\boldsymbol{x}_{gen} \leftarrow \boldsymbol{x}_{gen} - 2\boldsymbol{x}_{//} \\
\text{if } |\alpha_{trunc} - x_{//}| > 1$$`

5. `Deform` 
`$$\boldsymbol{x}_{gen} \leftarrow \boldsymbol{x}_{gen} - \alpha_{def}\boldsymbol{x}_{\perp}$$`

6. `Translate`
`$$\boldsymbol{x}_{gen} \leftarrow \boldsymbol{x}_{center} + R\boldsymbol{x}_{gen}$$`
{{</expand>}}

### 4-3. Justification of the Algorithm

G-SMOTE extends the linear interpolation mechanism by introducing a geometric region where the data generation process occurs.

1. `\(S_{gen}\)` is initialized with empty.
2. `\(S_{min}\)` are shuffled.
3. `\(\boldsymbol{x}_{center}\)` is selected.
4. SMOTE의 selection 과정을 일반화한 파트이다. `Surface`에서 `\(\alpha_{sel}\)`에 따라 세 가지 경우의 수가 나온다. 자세한 거는 위를 참고하길 바란다.
5. `Vectors`에 해당하는 부분이다.  
`\(\boldsymbol{x}_{//}\)`: projection of `\(\boldsymbol{x}_{gen}\)` to unit vector `\(\boldsymbol{e}_{//}\)`  
`\(\boldsymbol{x}_{\perp}\)`: perpendicular to the same vector belonging also to the hyperplane dinfed by `\(\boldsymbol{x}_{gen}\)` and `\(\boldsymbol{e}_{//}\)`
6. 여기서부터 data generation 부분이다. `Hyperball`에 따라 `\(\boldsymbol{e}_{sphere}\)`와 `\(\boldsymbol{x}_{gen}\)`를 만든다.
7.  `Truncate`
8. `Deform`
9. `Translate`

## 5. Research Methodology

### 5-1. Experimental Data
총 69개 datasets
- UCI Machine Learning Repository: 13 datasets
- KEEL repository: 13 datasets
- Simulated data based on variations of the "MANDELION" dataset: 2 datasets
- additional datasets with higher imbalance ratios

### 5-2. Evaluation Measures
i) Accuracy
ii) AUC
iii) F-score <!--: Precision과 Recall의 조화평균-->
iv) G-mean <!--: Sensitivity와 Specificity의 기하평균-->

### 5-3. Machine Learning Algorithms
비교대상: SMOTE, Random Oversampling, NO oversampling
분류기: Logistic Regression, K-Nearest Neighbors, Decision Tree, Gradient Boosting Classifier

### 5-4. Experimental Procedure
5-fold cross validation  
`\(k \in {3,5}\)`  
`\(\alpha_{trunc} = \{-1.0, -0.5, 0.0, 0.25, 0.5, 0.75, 1.0\}\)`  
`\(\alpha_{def} = \{0.0, 0.2, 0.4, 0.5, 0.6, 0.8, 1.0\}\)`

통계적으로 유의한 차이가 있는지 보기 위해서 Friedman Test와 Holms Test를 진행하였다. 자세한 내용은 아래 6-2. Statistical Analysis를 참고하면 된다.

### 5-5. Software Implementation
python에서 해당 패키지가 구축되어있다.

## 6. Results and Discussion

### 6-1. Comparative Presentation
![table2](images/posts/blog/gsmote/table2.PNG)
![table3](images/posts/blog/gsmote/table3.PNG)
![table4](images/posts/blog/gsmote/table4.PNG)

### 6-2. Statistical Analysis
![table5](images/posts/blog/gsmote/table5.PNG)
![table6](images/posts/blog/gsmote/table6.PNG)

[Table 5] **Friedman Test**: oversampling 방식에 따라서 통계적으로 유의한 차이가 있는지 확인  
- 결론: 모든 분류기들은 oversampling 방식에 따라 모든 evaluation metric에서 평균 rank가 다르다.  

[Table 6] **Holms Test**: G-SMOTE를 기준으로 다른 방법론들보다 좋았는지 확인
- 결론: G-SMOTE가 다른 oversampling 방법들보다 성능이 좋다.

### 6-3. G-SMOTE taxonomy

G-SMOTE의 geometric hyperparameter: `\(\alpha_{trunc}, \alpha_{def}, \alpha_{sel}\)`

1. SMOTE
`\(\alpha_{trunc}=1.0, \alpha_{def}=1.0, \alpha_{sel}=\text{minority}\)`를 하면, 일반적인 SMOTE와 같다.

2. Modified SMOTE
`\(\alpha_{def}=1.0\)`으로 고정하더라도, 나머지 `\(\alpha_{trunc}, \alpha_{sel}\)`에 따라 SMOTE를 조금 더 변형된 형태로 활용할 수 있다. line segment 위에서 새로운 synthetic example을 만들어내는 것은 SMOTE와 같지만, `\(\alpha_{trunc}\)`와 `\(\alpha_{sel}\)`의 조합에 따라 truncated, expanded, rotated를 만들어낼 수 있다.

3. Pure G-SMOTE
`\(\alpha_{trunc}\)`와 `\(\alpha_{sel}\)`에 더불어서 `\(\alpha_{def}\)`을 자유롭게 설정하게 되면, data generation area가 직선(line segment)에서 초-회전타원체(hyper-spheroid)가 된다.

![table7](images/posts/blog/gsmote/table7.PNG)

Table 7을 통해서 알 수 있듯이, 총 26,391번의 실험에서 Pure G-SMOTE가 압도적으로 많은 빈도수로 성능이 좋았다.

### 6-4. Analysis and Tuning of optimal hyper-parameters

1. `\(\alpha_{trunc}, \alpha_{def}, \alpha_{sel}\)`의 의미  
`\(\alpha_{trunc}, \alpha_{def}, \alpha_{sel}\)`은 data generation process에서 영향을 미친다. 특히 `\(\alpha_{sel}=\text{majority}\)`의 경우, minority class area를 공격적으로 확장하게 되며, `\(\alpha_{trunc}\)`와 `\(\alpha_{def}\)`의 절댓값을 낮은 숫자로 설정할수록 더더욱 그러한 효과를 크게 볼 수 있다.

2. IR 또는 R과 geometric hyperparameter 간의 관계
여기서 IR은 Imbalance Ratio, R은 변수 수 대비 샘플 수를 의미한다.  

**i) High IR or Low R**
majority 또는 combined, 그리고 낮은 절댓값의 truncation, deformation hyperparameter가 더 좋은 성능을 보였다. 불균형도가 높은 경우에는 일반 SMOTE는 기존의 데이터와 거의 유사한 또는 noisy 샘플들을 만들어낸 것으로 보인다. 또한, R이 낮은 경우(sparse input space)에는 일반 SMOTE의 기본 linear interpolation 과정이 특정 방향의 input space에서만 샘플들 만들어내어 기존 데이터와 유사하거나 noisy한 샘플들을 만들어낸 것으로 해석할 수 있다.

**ii) Low IR or High R**
minority, 그리고 높은 절댓값의 truncation, deformation hyperparameter가 상대적으로 우수한 성능을 보였다. 불균형도가 낮거나 R이 큰 경우에는 input space가 이미 충분히 정보를 가지고 있어서 SMOTE의 단점을 극복할 수 있었던 것으로 해석해볼 수 있다.

## 7. Conclusions
정리하자면, G-SMOTE는 minority class area 근처에서 safe radius를 정하고 안전한 초-회전타원체 내에서 추가적인 샘플들을 만들어내는 방식이다. 적은 수의 hyperparameter를 조정해주기만 해도 퀄리티 좋은 샘플들을 만들어낼 수 있다는 측면에서 G-SMOTE는 이전보다 발전했다고 할 수 있다.

# ---

## MY OWN OPINION
1. Oversampling 과정에 있어서 Selection phase에 비해 상대적으로 덜 연구가 된 Data Generation phase에서 신선한 논문인 것 같다.

1. 같은 연구실에 계신 박사님께서는 AR-SMOTE(Angle-Rotated SMOTE) 연구도 하시고 했던 것으로 미루어보아, 굉장히 괜찮은 연구방향이라고 생각된다. 

1. Geometric SMOTE for Regression 논문도 있던데, 얼른 읽어봐야겠다. Classification과 달리 Regression에서는 y를 만들어내는 과정도 굉장히 중요할 것 같은데, 이러한 부분에 특히 더 주목해서 보아야겠다.

1. IR(불균형도)만 보는 것이 아니라 R(변수수 대비 샘플수)를 기준으로도 사후분석을 해볼 필요가 있다는 것을 배웠다.

1. G-SMOTE와 별개로, Related Works를 읽다보니 within-class 불균형에 주목하고 manifold structure를 보존하고자 하는 `SOMO`라는 논문을 알게 되었다.
