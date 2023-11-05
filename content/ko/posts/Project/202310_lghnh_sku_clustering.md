---
date: "2023-10-31T10:08:56+09:00"
description: null
draft: false
title: LG생활건강 유사제품 군집화
weight: 1
---

## 유사제품 군집화
- 분석기간: 2023 September ~ 2023 October
- LG생활건강 데이터&머신러닝 Project팀 인턴 당시 진행한 프로젝트이다.

## 1. Domain Knowledge
### 1-1. 자재마스터
`자재마스터`(Material master data)란, 기업이 관리하는 모든 재고품, 구매품, 생산품등 물품들의 정보를 총체적으로 정리한 자료를 의미한다.

### 1-2. SKU
`SKU`는 Stock Keeping Unit의 약자로, 재고 관리 단위를 의미합니다. 유통업체와 물류업체에서 사용되는 용어입니다. 예를 들어, A샴푸를 용량에 따라 A샴푸 250ml, A샴푸 500ml, A샴푸 1L 이런 식으로 다르게 판매를 하고 있다면, 각각은 다른 SKU입니다. 용량뿐만 아니라, 특정 행사용 기획상품의 경우도 당연히 구분 요소가 될 수 있습니다.

### 1-3. 반제품
`반제품`은 제품이 두 개 또는 여러 개의 공정을 거쳐서 완성될 때 일부의 공정이 끝나서 다음 공정에 인도될 완성품 또는 부분품으로서 일정한 제품으로서는 미진한 것이지만, 가공이 일단 완료됨으로써 저장 가능하거나 판매가능한 상태에 있는 부품을 의미한다. SKU가 다르더라도, 반제품을 공유하는 경우 같은 상품으로 볼 수 있는 여지가 있다.

# ---

## 2. What I Have Learned

### 2-1. NER (Named Entity Recognition)
`NER`은 한국어로 `개체명 인식`이다. 해당 방식을 통해서, 전처리된 자재명 중에서 어떤 것이 브랜드인지, 브랜드라인지, 제품종류인지, 제품상세인지 구분하는 모델을 학습할 우 있었다. `spaCy`라는 알고리즘을 활용하였다. 한국어 뉴스로 학습한 사전학습모델을 fine-tuning하여 적용하였다. 무엇보다, 직접 훈련데이터를 며칠간 만들어서 학습시킨 것이 개인적으로 유의미한 경험이었다. spaCy를 보다 다양한 과제에서 활용할 수 있을 것 같은데, 이를 위해서는 추후에 추가적인 공부가 뒷받침된다면 더욱 좋을 것 같다.

### 2-2. Streamlit
`Streamlit`은 python에서 분석한 것들을 쉽게 웹에 띄울 수 있는 프레임워크이다. `R`의 `shiny`와 같이 대시보드를 만드는 역할이라고 보면 된다. 또한, 커뮤니티가 적당히 활발하여 [dynamic filters](https://github.com/arsentievalex/streamlit-dynamic-filters)와 같은 기능들을 활용하여 대시보드를 고도화할 수 있었다. 이외에도 [streamlit-extras](https://extras.streamlit.app/)에서 유용한 추가기능들을 살펴볼 수 있었다. 특히 아래의 코드에서 `edited_df`는 대시보드 상에서 dataframe을 수정할 수 있게 해주는 것이었어서 유용하게 활용했다.

```python
edited_df = st.experimental_data_editor(df)
favorite_command = edited_df.loc[edited_df["rating"].idxmax()]["command"]
st.markdown(f"Your favorite command is **{favorite_command}** 🎈")
```

[Streamlit Gallery](https://streamlit.io/gallery)를 보면 다양한 형식의 예시들을 확인할 수 있다. 대부분 github을 통해서 코드를 공유 중이니, 개발 시 참고하면 좋을 듯 하다. 나아가, streamlit 공식홈페이지에서 github과 연동하여 개인당 3개까지 무료로 배포를 할 수 있다. 용량과 같은 측면에서 일부 제한이 있기는 하지만, 그래도 유용하게 활용 가능할 것 같다. 만약 3개를 초과해서 활용해서 한다면, netlify를 추후에 이용해도 좋지 않을까 싶다.

개인적으로 가장 큰 어려움이자 배움이 되었던 것은, session_state를 설정하고 캐시데이터를 관리하는 것이었다. 특정 버튼을 누르면 초기화가 되는데, 전체가 초기화되는 것을 막고 대시보드의 성능을 고도화하기 위해서 적지 않은 시간을 투자했던 경험이 유의미했다. 이는 추후 React와 같은 프론트 프레임워크에 대해 공부를 시작하는 데에 자극제가 되어주었다.

## 3. Result
https://lghnh-internship.streamlit.app/

보안상의 문제로, 최종발표자료 및 대시보드를 직접 반출할 수는 없었다. 대신, 두 달 간 어떤 순서로 '유사제품 군집화' 모델을 개발해왔는지에 대해 Streamlit을 통해 타임라인으로 작성해보았다. 자세한 내용은 위 링크를 통해서 확인 가능하다.

## 4. ETC
Streamlit을 다뤄본 이후, 프론트에 관심이 생겨서 클래스101에서 React를 포함한 개발 관련 강좌를 살펴보는 중이다.

## 참고사이트
https://m.blog.naver.com/y10237/221289910088