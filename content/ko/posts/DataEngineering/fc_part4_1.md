---
collapsible: false
date: "2021-01-13T10:09:56+09:00"
description: 데이터의 이해와 데이터베이스
draft: false
title: RDBMS
weight: 5
---

## Part 4. 데이터의 이해와 데이터베이스
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 0. Data Type
- numeric
- data/time
- character/string
- unicode character/string
- binary
- miscellaneous

### 1. Relational Database(RDB)
- 모든 데이터를 2차원의 테이블로 표현
- 하나 이상의 테이블로 구성
- Entity-Relationship 모델
- **Normalization** (Reduce Redundacy)

### 2. AWS 클라우드 MySQL 데이터베이스 생성
- aws.amazon.com > RDS > 데이터 생성
- Templates > `Free Tier`로 설정 (과금 예방)
- Public Access 허용하기
- VPC에서 인바운드 규칙에 MySql 추가하기

### 3. 터미널에서 데이터베이스 연결하기
(Windows 기준)
- mysql client workbench [다운로드](https://dev.mysql.com/downloads/workbench/)
- MySQL Workbench랑 AWS를 연결하고, 그것을 termianl(powershell)로 연결하는 법
- termianl에서 아래의 커맨드를 작성하고, 이어서 비밀번호를 입력해주면 된다.

```console
mysql -h {hostname} -P 3306 -D {Default Schema} -u {username} -p
```

### 4. MySQL 데이터베이스 안에서 테이블 생성
{{<highlight MySQL>}}
CREATE TABLE people (first_name VARCHAR(20), last_name VARCHAR(20), age INT);
SHOW TABLES;
{{</highlight>}}

### 5. 엔터티 관계도(ERD)
- Entity Relationship Diagram
- 데이터 모델링 설계 과정에서 사용하는 모델
- 약속된 기호를 이용하여 데이터베이스의 구조를 쉽게 이해하기 위함이다.

###### ERD의 기본요소
- Entities: 개체
- Attributes: 엔터티의 속성
- Relationship: 엔터티 간의 관계

![Relationship Cardinality](https://www.guru99.com/images/1/100518_0621_ERDiagramTu7.png)
![ERD example](https://www.guru99.com/images/1/100518_0621_ERDiagramTu1.png)

### 6. Primary Key & Unique Key

###### Primary Key
- 테이블에 하나 밖에 없는 유니크한 구별 값
- Null 값 안 됨

###### Foreign Key
- 한 개 이상 가능
- NULL 값도 가능

###### Unique Key
- Primary Key처럼 유니크하긴 하다.
- 하지만, Null 값은 하나는 가질 수 있다.
- 그리고 하나 이상의 유니크 키를 가질 수 있다.
- Primary Key보다는 index로서의 성능은 낮다.
- ex) Primary Key: 수험번호, Unique Key: 주민등록번호

<br>

--- 