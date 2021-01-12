---
collapsible: false
date: "2021-01-12T10:09:56+09:00"
description: API는 무엇인가
draft: false
title: Spotify API
weight: 4
---

## Part 3. API는 무엇인가
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 1. Spotify App 생성 및 토큰 발급
###### Client Credentials Flow
![Client Credentials Flow](https://developer.spotify.com/assets/AuthG_ClientCredentials.png)

```json
{
   "access_token": "NgCXRKc...MzYjw",
   "token_type": "bearer",
   "expires_in": 3600,
}
```
* client id, client secret을 제공하면 우리는 3600초, 즉 1시간동안 사용할 수 있다.

### 2. Python 기본 
```python
import sys

def main():
    print('fastcampus')

#python으로 실행했을 때, 해당 py파일 이름이 전달되면, main()을 실행하라
if __name__ == '__main__':
    main()
#직접 py파일이 실행 안되고, import spotify_api와 같이 모듈처럼 import되면, ~~를 print하라.
else:
    print('this script i being imported')
```
* Windows는 Windows Powershell을 통해서 진행하면 된다.
* 기본적으로 위처럼 코딩을 시작하게 된다.

### 3. Python Requests 패키지
[`requests python library`](https://requests.readthedocs.io/en/master/api/) > Developer Interface 참고하기



### 4. API를 통한 데이터 요청

### 5. Status Code

### 6. 에러 핸들링

### 7. 페이지네이션 핸들링


<br> <br>

--- 