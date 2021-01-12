---
collapsible: false
date: "2021-01-12T10:09:56+09:00"
description: API는 무엇인가
draft: false
title: API
weight: 3
---

## Part 3. API는 무엇인가
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 1. API 정의
- Application Programming Interface
- 두 개의 시스템이 서로 상호작용하기 위한 인터페이스(데이터 주고 받기!)
- 일반적으로 API는 REST API를 지칭한다. 
- ex) Web API: 웹을 통해 외부 서비스들로부터 정보를 불러오는 API

### 2. API 접근 권한
- Authentication: Identity가 맞다는 증명
- Authorization: API를 통한 어떠한 액션을 허용
- 둘은 다르다! Athentication을 하였다고 하더라도 Authorization을 허용하지 않을 수 있다!
- Security 이슈가 중요하다.

###### API Key?
- 보통 `Request URL` 혹은 `Request Header`에 포함되는 긴 스트링
- ex) Google Maps Platform > Geocoding API
    - https://<span></span>maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=`YOUR_API_KEY` 
    - 여기서 `YOUR_API-KEY` 이부분을 채워주지 않으면, request denied가 뜨게 된다.

###### Baisc Auth
- username:password와 같은 credential을 Base64로 인코딩한 값을 `Request Header` 안에 포함

###### OAuth 2.0
- End User <=> My App <=> Server(ex. Spotify)
- (1) App에서 End User한테 생일, 전화번호, 플레이리스트와 같은 정보를 가져가는 것에 대해서 동의를 받는다.
- (2) 동의서를 받아왔으니 Server한테 API 요청을 하고 그에 맞는 데이터를 요구한다.

### 3. Spotify Web API
![](https://developer.spotify.com/assets/WebAPI_intro.png)
- [Spotify](https://developer.spotify.com/documentation/web-api/reference/) 직접 방문해보기

### 4. Endpoints & Methods
- Resource: API를 통해 리턴된 정보
- Endpoint: Resource 안에는 여러 개의 Endpoints가 존재
- Method: 자원 접근에 허용된 행위(GET, POST, PUT, DELETE)

| Method  | Action |
| :-----: | :----: |
| GET     | 해당 리소스를 조회하고 정보를 가져온다.|
| HEAD    | 응답코드와 HEAD만 가져온다.|
| POST    | 요청된 리소스를 생성한다.|
| PUT     | 요청된 리소스를 업데이트 한다.|
| DELETE  | 요청된 리소스를 삭제한다.|

### 5. Parameters
- Parameters:  Endpoint를 통해 Request할 때 같이 전달하는 옵션들

| Type          | 내용 |
| :-----------: | :------- |
| Header        | Request Header에 포함. 주로 Authorization에 관련|
| Path          | Query String(`?`) 이전에 Endpoint Path 안에 포함. <br> ex) id|
| Query String  | Query STring(`?`) 이후에 포함. <br> ex) ?utm_source=facebook&...|
| Request Body  | Request Body 안에 포함. 주로 JSON 형태|

<br> <br>

--- 