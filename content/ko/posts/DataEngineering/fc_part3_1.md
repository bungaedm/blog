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
- 보통 Request URL 혹은 Request Header에 포함되는 긴 스트링