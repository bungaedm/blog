---
collapsible: false
date: "2021-02-21T10:08:56+09:00"
description: null
draft: false
title: 신뢰구간, 신용구간
weight: 3
---

# 신뢰구간과 신용구간
간단하게 구분하자면, 신뢰구간은 빈도주의자가, 신용구간은 베이지안이 사용하는 것이다.  
일반적으로 신용구간을 신뢰구간으로 착각하는 경우가 많다.

### 신뢰구간 (Confidence Interval)
{{< notice info "Definition" >}}
“If we repeat the experiment infinitely many times, 95% of the experiments will capture the population parameter in their confidence intervals.”
{{< /notice >}} <br>

해석하자면, 무수히 많이 반복하여 데이터를 얻고 신뢰구간을 산출한다면, 그 수많은 신뢰구간 중 95%는 모수를 갖고 있을 것으로 **신뢰**한다는 의미이다. 그렇기 때문에 한번의 실험결과만으로 신뢰구간을 구하고 이를 활용하는 데에는 다소 무리가 있어보인다. 하지만 중심극한정리를 통해 정규성을 확보함으로써 어느 정도의 논리적 비약은 막는다고 빈도론자들은 생각한다.

### 신용구간 (Credential Interval)
{{< notice info "Definition" >}}
“There is 95% probability/plausibility/likelihood that the population parameter lies in the interval.”
{{< /notice >}} <br>

해당 구간에 모수가 있을 확률을 구한다. 95%가 되는 구간은 무수히 많이 잡을 수 있겠지만, 그중에서 HPD(Highest posterior Density) region을 구하여 활용한다. 이는 x축에 평행한 선을 위에서부터 내려오면서 적용하여 그 사이 영역의 넓이가 95%가 되는지 확인하는 방법으로 계산한다.

신용구간을 사용함으로써 얻을 수 있는 장점은 크게 두 가지로 요약 된다.
1) 사후분포가 정규분포가 아니더라도 확률계산을 할 수 있다. 이는 정의역에 대한 전제를 고려할 수 있다는 장점으로 이어진다.
2) 사전확률을 고려함으로써 신뢰구간보다 빠르게 신용구간을 구할 수 있다.

<img src="/ko/posts/Statistics/Statistics/interval_files/figure-html/unnamed-chunk-1-1.png" width="672" />


###### 참고사이트
[1] https://towardsdatascience.com/do-you-know-credible-interval-e5b833adf399
[2] FCB figure 3.6
