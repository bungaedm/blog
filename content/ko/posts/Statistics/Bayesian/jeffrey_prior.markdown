---
collapsible: false
date: "2021-03-07T10:08:56+09:00"
title: Jeffrey's Prior
weight: 99
---

## Jeffrey's Prior

### 1. uninformative prior의 제한점
uninformative prior를 임의로 주게 될 경우, 여러 문제점이 있을 수 있는데 그 중 하나는 변수변환에 취약해질 수 있다는 점이다.  
예를 들어, `$p(\theta) \propto 1$`라고 uninformative prior를 주자. 그리고 `$ \phi = exp(\theta)$`라고 가정해보자.

`\begin{align}
p(\phi) &\propto p(\theta) \bigg|\frac{d\theta}{d\phi}\bigg| \\
&\propto \frac{1}{\phi} \neq 1
\end{align}`

변수변환 후에는 prior가 uninformative하지 않게 되어버림을 확인할 수 있다.

### 2. Jeffrey's prior
그렇다면 어떻게 해야 변수변환에 강건한 prior를 줄 수 있을까?
$$\pi(\phi) \propto \sqrt{I(\theta)} $$
위와 같이 주면 된다. 여기서 `$I(\theta) $`는 [Fisher Information](/posts/statistics/statistics/fisher_information/)을 뜻하며, 아래와 같다.
`$$I(\theta) = -E\Big[ \frac{\partial^2}{\partial{\theta}^2}ln L(x|\theta) \Big]$$`

### 3. 증명
이를 한 번 증명해보자. 단, `$\phi = f(\theta), \ f:\text{one-to-one}$`이다.

`\begin{align}
p(\theta) \propto \sqrt{I(\theta)} &\xrightarrow{?} \ p(\phi) \propto \sqrt{I(\phi)} \\
\\
p(\phi) &= p(\theta) \bigg| \frac{\partial\theta}{\partial\phi} \bigg| \\
\\
I(\phi) &= -E\bigg[\frac{\partial^2}{\partial\phi^2}lnL(y|\phi) \bigg] \\
&= E\bigg[\big(\frac{\partial}{\partial\phi}lnL(y|\phi) \big)^2 \bigg] \\
&= E\bigg[\big(\frac{\partial}{\partial\theta}lnL(y|\theta) \big)^2 \big(\frac{\partial\theta}{\partial\phi} \big)^2 \bigg] \\ 
&= I(\theta) \bigg|\frac{\partial\theta}{\partial\phi} \bigg|^2\\
\\
p(\phi) &= p(\theta)\bigg|\frac{\partial\theta}{\partial\phi} \bigg| \\
&\propto \sqrt{I(\theta)}\bigg|\frac{\partial\theta}{\partial\phi} \bigg| \\
&=\sqrt{I(\phi)} \\
\\
\rightarrow p(\phi) &\propto \sqrt{I(\phi)}
\end{align}`

---
<br> 
<p style='text-align: center; color:gray'> 혹시 궁금한 점이나 잘못된 내용이 있다면, 댓글로 알려주시면 적극 반영하도록 하겠습니다. </p>

<br>
<br>
