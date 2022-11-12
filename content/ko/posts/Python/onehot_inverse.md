---
date: "2022-11-11T00:08:29+09:00"
description: null
draft: false
title: Onehot Encoding 역변환
weight: 1
---

기본적으로 python에서 pandas를 활용하여 아래와 같은 방식으로 OneHot Encoding을 한다.
```python
pd.get_dummies(df)
```

그런데 이를 역변환하는 함수는 패키지를 통해 제공되고 있지는 않다. 그렇지만 아래의 reference에서 좋은 함수를 만들어서 공유해주셔서 이를 공유하고자 한다.

```python
def undummify(df, prefix_sep="_"):
    cols2collapse = {
        item.split(prefix_sep)[0]: (prefix_sep in item) for item in df.columns
    }
    series_list = []
    for col, needs_to_collapse in cols2collapse.items():
        if needs_to_collapse:
            undummified = (
                df.filter(like=col)
                .idxmax(axis=1)
                .apply(lambda x: x.split(prefix_sep, maxsplit=1)[1])
                .rename(col)
            )
            series_list.append(undummified)
        else:
            series_list.append(df[col])
    undummified_df = pd.concat(series_list, axis=1)
    return undummified_df
```

## Reference
[1] https://preservsun.tistory.com/entry/%EB%8D%94%EB%AF%B8%EB%B3%80%EC%88%98-%EC%A0%84%ED%99%98-%EC%A0%84%ED%99%98-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0-python-code