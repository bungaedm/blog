---
collapsible: true
date: "2022-07-21T10:08:56+09:00"
description: AnoGAN
title: AnoGAN
weight: 1
---

# AnoGAN
Schlegl, T., Seeböck, P., Waldstein, S. M., Schmidt-Erfurth, U., & Langs, G. (2017, June). Unsupervised anomaly detection with generative adversarial networks to guide marker discovery. In International conference on information processing in medical imaging (pp. 146-157). Springer, Cham.


## In Short
Semi-supervised Anomaly Detection, 근데 GAN을 곁들인. (솔직히 unsupervised는 아니다.)

## 1. Introduction
정상데이터만으로 `DCGAN`을 학습시킨 후, image space에서 latent space로의 mapping을 base로 하는 새로운 anoamly score를 제안한 방법론

## 2. Related Work
### 2-1. DCGAN
![DCGAN](images/posts/deep_learning/DCGAN.JPG)

DCGAN: Deep Convolutional Generative Adversarial Networks
- 특징. **Walking in the latent space**
  - 1. latent vector을 조금씩 변경하면 이미지(그림)도 부드럽게 변경된다.
  - 2. Generator에서 Memorization이 일어나는 것이 아니다. (이미지를 외워서 보여주는 것이 아니다.)
  - 3. 즉, 이미지와 overfitting되는 것이 아니다.

### 2-2. t-SNE embedding

## 3. Methods

### 3-1. DCGAN
정상데이터로 DCGAN을 학습시킨다.

$$
\min_G\max_D V(D,G) = E_{x\sim p_{data}(x)}[\log D(x)] + E_{z\sim p_z(z)}[\log(1-D(G(z)))] \\
z : \text{latent vetor} \\
x: \text{data sample} \\
G(z): \text{generated sample using z} \\
D(x): \text{probability that x is a real sample}
$$


## 4. Performance Comparison
### 4-1. Dataset
### 4-2. Baseline
### 4-3. Main Results

## 5. Conclusion

# ---

## Critical Point (MY OWN OPINION)
1. 솔직하게 이 방법론은 DCGAN에서 정상데이터만으로 학습하기 때문에, unsupervised라고 하기에는 어렵지 않나 싶다. 굳이 따지자면 semi-supervised가 맞지 않나 싶다.

# ---

## Reference
[1] https://sensibilityit.tistory.com/506
[2] [Youtube 영상](https://www.youtube.com/watch?v=t2eZzmeRcAg)
