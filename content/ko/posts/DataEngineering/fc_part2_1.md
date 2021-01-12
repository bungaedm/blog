---
collapsible: false
date: "2021-01-12T10:08:56+09:00"
description: 데이터 엔지니어 기초 다지기
draft: false
title: Part 2-1. 기초
weight: 1
---

## Part 2. 데이터 엔지니어 기초 다지기
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.


### 1. Unix 환경 및 커맨드
`cd`
`mkdir`
`ls`
`cp`
`./run.sh`
`chmod +x run.sh`: Permission denied 되었을 때, 권한 부여하기

###### run.sh 코드(참고용)
```
#!/bin/bash
python examply.py 1 > example.txt
python examply.py 2 > example.txt
```
*주의사항: `python` 대신에 `python3`를 쓰면 Windows에서는 오류가 날 수도 있다.

### 2. AWS 기초 및 CLI 세팅
AWS CLI (Command Line Interface) [다운로드](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/install-cliv2-windows.html)

* IAM에서 사용자 추가하기
* Windows Powershell에다가 `aws configure` 입력하기
* 액세스 키 ID와 비밀 액세스 키 입력하기

이는 앞으로 shell에서 `aws` command를 쳐도 가능하게끔 세팅해놓는 것이다.

<br>
<br>