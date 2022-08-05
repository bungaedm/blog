---
date: "2021-01-31T00:08:29+09:00"
description: null
draft: false
title: Python 꿀팁
weight: 1
---

## 1. 추천 사이트
- [pandas_exercises](https://github.com/guipsamora/pandas_exercises)

## 2. 추천 책
- Python for Data Analysis (Wes McKinney)

## 3. 검색 팁
구글링 시, 뒤에 'towards data science' 또는 'medium' 붙이기  
하루 열람 제한이 있는데, 더 읽고 싶은 경우는 Chrome 시크릿 모드를 활용하면 제한이 풀린다. 

## 4. Matplotlib 한글 깨짐 현상 해결
```python
font_path = "C:/Windows/Fonts/NGULIM.TTF"
font = font_manager.FontProperties(fname=font_path).get_name()
rc('font', family=font)
```

## 5. Jupyter Notebook 셀 크기 조정
```python
from IPython.core.display import display, HTML
display(HTML("<style>.container { width:60% !important; }</style>"))
```

여기서 60%를 모니터 크기에 따라 80~100%로 바꾸어서 사용하면 된다 