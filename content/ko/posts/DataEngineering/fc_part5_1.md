---
collapsible: false
date: "2021-01-13T10:09:56+09:00"
description: 데이터 엔지니어링 구축
draft: false
title: 데이터 레이크
weight: 10
---

## Part 5. 데이터 엔지니어링 구축
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 1. 데이터 레이크 vs. 데이터 웨어하우스

| 구분 | 데이터 레이크 | 데이터 웨어하우스 |
| :--- | :---: | :---: |
| Data Structure| Raw | Processed |
| Purpose of Data | Not yet Determined | In Use |
| Users | Data Scientists | Business Professionals |
| Accessibility | High / Quick to update | Complicated / Costly |

* Schema의 차이가 가장 크다.
* 데이터레이크는 차세대 시스템으로서 더욱 주목 받게 될 것이다.
* ETL(Extract-Transform-Load)

### 2. 데이터 레이크 아키텍처

###### 데이터 파이프라인의 관심사
- 어떻게 관리
- 스케쥴링
- 에러 핸들링
- 데이터 백필 

### 3. AWS S3
- AWS S3 버킷 생성
- cf. AWS Glue: table schema 관리 (데이터 레이크는 바뀐다.)

### 4. JSON, Parquet
```python
# JSON 형식으로 하는 법
with open('top_tracks.json', 'w') as f:
    for i in top_tracks:
        json.dump(i, f)
        f.write(os.linesep)
```

```python
# Parquet 형식으로 하는 법
# 퍼포먼스적으로 우수해진다.
top_tracks = pd.DataFrame(raw)
top_tracks.to_parquet('top-tracks.parquet', engine='pyarrow', compressions='snappy')

dt = datetime.utcnow().strftime("%Y-%m-%d")

s3 = boto3.resource('s3')
object = s3.Object('spotify-artists', 'dt={}/top-tracks.parquet'.format(dt))
data = open('top-tracks.parquet', 'rb')
object.put(Body=data)
```
### 5. S3 Data Lake
```error
ImportError: Missing optional dependency 'pyarrow'. pyarrow is required for parquet support. Use pip or conda to install pyarrow.
```

[Spotify audio features](https://developer.spotify.com/documentation/web-api/reference/tracks/get-several-audio-features/)