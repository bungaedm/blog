---
collapsible: false
date: "2021-03-07T10:08:56+09:00"
title: F1 Score
weight: 98
---

## Performance Measure
성능을 평가하는 지표에는 여러 가지가 있다. 그중에서 대표적인 몇 개를 알아보고자 한다.

### 1. Accuracy
$$Accuracy = \frac{\text{correctly predicted}}{\text{all dataset}}$$ 

Accuracy는 balanced data가 아니라면 좋은 지표로서의 역할을 하기 힘들다. 왜냐하면 A,B,C,D라는 그룹이 있을 때, B~D가 각각 10개의 케이스 밖에 없고 A가 혼자서 500개의 케이스가 있다고 가정한다면 Accuracy 지표는 A 그룹에 의해 좌지우지 될 것이기 때문이다.

### 2. Precision
> Given a class prediction from the classifier, how likely is to be correct?
내가 'TRUE'라고 말한 것 중에서 몇 %가 진짜 'TRUE'인가?

$$Precision = \frac{TP}{TP + FP} $$
TP: True Positive, FP: False Positive

### 3. Recall
> Given a class, will the classifier detect it?
진짜 'TRUE' 중에서 내가 'TRUE'라고 말한 것은 몇 %인가?

$$Recall = \frac{TP}{TP + FN} $$
TP: True Positive, FN: False Negative

### 4. F1 Score
Precision과 Recall의 조화평균
$$F1 \text{ score} = 2 \times \frac{\text{precision}\times\text{recall}}{\text{precision}+\text{recall}} $$

{{< img src="/images/posts/statistics/f1_score.PNG" width="400px" position="center" >}}
<p style='text-align: center; color:gray'> 출처: https://www.youtube.com/watch?v=HBi-P5j0Kec </p>

F1 Score는 상대적으로 imbalanced data에서도 효과가 좋다. 그 이유는 위 그림을 통해서 이해할 수 있다.  
조화평균은 더 극단치에 대해서 페널티를 주기 때문이다.

###### 참고
[1] [허민석 유튜브](https://www.youtube.com/watch?v=HBi-P5j0Kec)