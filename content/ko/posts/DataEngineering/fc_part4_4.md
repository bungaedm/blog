---
collapsible: false
date: "2021-01-13T10:09:56+09:00"
description: 데이터의 이해와 데이터베이스
draft: false
title: SQL 활용 (MySQL)
weight: 8
---

## Part 4. 데이터의 이해와 데이터베이스
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 1. Artist Data
```sql
SELECT genre, COUNT(*) FROM artist_genres GROUP BY 1 ORDER BY 2 DESC LIMIT 20;
```

### 2. Artist Genre Analysis with SQL
```sql
SELECT popularity, name FROM artists ORDER BY 1 DESC LIMIT 20;

SELECT genre, COUNT(*) FROM artists t1 JOIN artist_genres t2 ON t2.artist_id = t1.id WHERE t1.popularity > 80 GROUP BY 1 ORDER BY 2 DESC LIMIT 20;
```
* join을 통해 ERD의 장점을 활용하여 기초분석을 할 수 있다.