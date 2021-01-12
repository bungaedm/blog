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
powershell에서 `pip install requests` 실행하기

### 4. API를 통한 데이터 요청
```python
import sys
import requests
import base64
import json
import logging

client_id = '8b2afab1206d4d8c8485818b98a8d12e'
client_secret = '1dfcd633c62d4837b1ac78d76b251573'

def main():
    headers = get_headers(client_id, client_secret)
    params = {
        'q': 'BTS',
        'type': 'artist',
        'limit': 5
    }

    r = requests.get('https://api.spotify.com/v1/search', params=params, headers=headers)
    # print(r.status_code) # 200이면 이상 없는 것
    # print(r.text)
    # sys.exit(0)

def get_headers(client_id, client_secret):
    # 1시간만 있으면 expire되기 때문에 추가로 function 하나를 만들어두는 것이다.
    endpoint = 'https://accounts.spotify.com/api/token'
    encoded = base64.b64encode("{}:{}".format(client_id, client_secret).encode('utf-8')).decode('ascii')

    headers = {
        'Authorization': 'Basic {}'.format(encoded)
    }

    payload = {
        'grant_type': 'client_credentials'
    }

    r = requests.post(endpoint, data=payload, headers=headers)
    # 중간에 잘 되는지 확인해보는 코드
    # print(r.status_code)
    # print(r.text)
    # print(type(r.text)) #string으로 출력되므로 아래에서 json.loads를 통해 dictionary로 만들어줘야 한다.
    # sys.exit(0)

    access_token = json.loads(r.text)['access_token']
    headers = {
        'Authorization': "Bearer {}".format(access_token)
    }

    return headers

if __name__ == '__main__':
    main()

```

### 5. Status Code
* Status Code를 알아야 하는 이유: 데이터 엔지니어의 잘못이 아닌, Spotify 서버의 오류 등으로 인한 문제인지 체크할 수 있다.
* Spotify Web API 기준이지만, RFC 2616와 RFC 6585에 의해 일반적으로 통용되는 기준이다.

| STATUS CODE | DESCRIPTION |
| :--- | --- |
| 200	| `OK` - The request has succeeded. The client can read the result of the request in the body and the headers of the response. |
| 201	| `Created` - The request has been fulfilled and resulted in a new resource being created. |
| 202	| `Accepted` - The request has been accepted for processing, but the processing has not been completed.|
| 204	| `No Content` - The request has succeeded but returns no message body.|
| 304	| `Not Modified.` See Conditional requests.|
| 400	| `Bad Request` - The request could not be understood by the server due to malformed syntax. The message body will contain more information; see Response Schema.|
| 401	| `Unauthorized` - The request requires user authentication or, if the request included authorization credentials, authorization has been refused for those credentials.|
| 403	| `Forbidden` - The server understood the request, but is refusing to fulfill it.|
| 404	| `Not Found` - The requested resource could not be found. This error can be due to a temporary or permanent condition.|
| 429	| `Too Many Requests` - Rate limiting has been applied.|
| 500	| `Internal Server Error.` You should never receive this error because our clever coders catch them all … but if you are unlucky enough to get one, please report it to us through a comment at the bottom of this page.|
| 502	| `Bad Gateway` - The server was acting as a gateway or proxy and received an invalid response from the upstream server.|
| 503	| `Service Unavailable` - The server is currently unable to handle the request due to a temporary condition which will be alleviated after some delay. You can choose to resend the request again. |

### 6. 에러 핸들링

### 7. 페이지네이션 핸들링


<br> <br>

--- 