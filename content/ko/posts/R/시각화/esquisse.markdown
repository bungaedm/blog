---
collapsible: false
date: "2021-03-19T10:08:56+09:00"
description: null
draft: false
title: esquisse
weight: 1
---

# esquisse
esquisse는 드래그 앤 드롭(drag & drop)으로 ggplot을 간단하게 그릴 수 있는 획기적인 패키지이다.  
복잡한 커스터마이징은 디테일한 수정이 추가적으로 필요하겠지만, 간단한 특징들을 반복적인 코드수정과 확인과정을 거치기 않고서도 즉각적으로 그래프 모양을 확인할 수 있다는 큰 장점이 있다.
거의 Tableu 같은 느낌도 든다. 간단한 ggplot 그릴 때 또는 ggplot 입문자가 먼저 거쳐가도 좋을 것 같다.


```r
library(ggplot2)
library(dplyr)
library(esquisse)
```

**STEP1**. `Addins`을 클릭하고, `ggplot2 builder`를 이어서 클릭한다.
{{< img src="/images/posts/r/visualization/esquisse/esquisse7.png" width="350px" position="center">}}

**STEP2**. `validate imported data`를 클릭한다.
{{< img src="/images/posts/r/visualization/esquisse/esquisse1.png" width="500px" position="center">}}

**STEP3**. 드래그 앤 드롭으로 X축, Y축 등을 설정한다.
{{< img src="/images/posts/r/visualization/esquisse/esquisse2.png" width="750px" position="center">}}

**STEP4-1**. `Labels & Title`에서는 제목과 축 이름 등을 설정한다.
{{< img src="/images/posts/r/visualization/esquisse/esquisse3.png" width="500px" position="center">}}

**STEP4-2**. `Plot Options`에서는 색깔을 포함한 전반적인 테마를 설정한다.
{{< img src="/images/posts/r/visualization/esquisse/esquisse4.png" width="500px" position="center">}}

**STEP4-4**. `Data`에서는 표현하고픈 또는 표현하고 싶지 않은 데이터를 필터링한다.
{{< img src="/images/posts/r/visualization/esquisse/esquisse5.png" width="500px" position="center">}}

**STEP4-3**. `Export & Data`에서는 `Insert code in script`를 클릭하면 아래와 같은 코드를 바로 script로 옮겨준다.
{{< img src="/images/posts/r/visualization/esquisse/esquisse6.png" width="500px" position="center">}}

**STEP5**. 친절하게 라이브러리까지 제시된 코드를 실행한다.

**STEP6**. 추가적으로 더 손 보고 싶은 부분을 개선한다.

{{<expand "코드 예시">}}

```r
library(dplyr)
library(ggplot2)

mpg %>%
 filter(displ >= 1.6 & displ <= 5.95) %>%
 filter(year >= 1999 & year <= 2005.2) %>%
 filter(hwy >= 12L & hwy <= 33L) %>%
 ggplot() +
 aes(x = manufacturer, y = cyl) +
 geom_boxplot(fill = "#18c975") +
 scale_y_continuous(trans = "log10") +
 theme_gray()
```

<img src="/ko/posts/R/시각화/esquisse_files/figure-html/unnamed-chunk-2-1.png" width="672" />
{{</expand>}}

###### 참고
[1] https://www.youtube.com/watch?v=FWLxE-ARuO8
