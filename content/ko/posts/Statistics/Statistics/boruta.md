---
collapsible: false
date: "2022-02-01T10:08:56+09:00"
description: Boruta
draft: false
title: Boruta
weight: 99
---

## Boruta

![Boruta](images/posts/statistics/boruta/boruta1.png)
 
### Summary
Random Forest나 XGBoost처럼 Feature Importance를 알 수 있는 알고리즘의 경우에 활용할 수 있는 Feature Selection 방법이다.

### Description
X변수를 랜덤셔플한 Shaodw 변수를 만들고, 이들의 변수 중요도 중 최댓값과 비교하여 변수를 선택하거나 제거하는 방법이다. 그런데 랜덤셔플한 변수이기 때문에 여러번 돌려서 하게 되는데, 그때마다 shadow 변수의 변수 중요도 최댓값과 비교하여 더 큰 변수 중요도를 몇 번이나 보였는지 체크한다. 그리고나서 0.5 확률로 binomial 분포를 그려서 하위 2.5%의 횟수에 해당하는 경우에만 변수를 제거한다.