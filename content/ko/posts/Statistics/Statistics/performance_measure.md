---
collapsible: false
date: "2021-03-07T10:08:56+09:00"
title: Performance Measure
weight: 99
---

## Performance Measure
성능을 평가하는 지표에는 여러 가지가 있다. 그중에서 대표적인 몇 개를 알아보고자 한다.

### 1. Accuracy
$$Accuracy = \frac{\text{correctly predicted}}{\text{all dataset}}$$ 

Accuracy는 balanced data가 아니라면 좋은 지표로서의 역할을 하기 힘들다. 왜냐하면 A,B,C,D라는 그룹이 있을 때, B~D가 각각 10개의 케이스 밖에 없고 A가 혼자서 500개의 케이스가 있다고 가정한다면 Accuracy 지표는 A 그룹에 의해 좌지우지 될 것이기 때문이다.

### 2. Precision

> Given a class prediction from the classifier, how likely is to be correct?

### 3. Recall

> Given a class, will the classifier detect it?

### 4. F1 Score

###### 참고
[1] [허민석 유튜브](https://www.youtube.com/watch?v=HBi-P5j0Kec)