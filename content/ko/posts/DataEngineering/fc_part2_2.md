---
collapsible: false
date: "2021-01-11T10:09:56+09:00"
description: 데이터 엔지니어 기초 다지기
draft: false
title: SQL 기초 (SQLite)
weight: 2
---

## Part 2-2. 데이터 엔지니어 기초 다지기
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 3. SQLite Studio
[SQLite Studio 다운로드](https://sqlitestudio.pl/index.rvt)
[데이터 다운로드](https://baseballdb.lawlesst.net/) <br>
- editor 여는 법: Tools > Open SQL Editor (or Alt + E)
- `SQLite`과 `MySQL`을 포함한 다른 프로그램들과 코드가 다른 것들이 사소하게 있을 수 있다. 

#### SQL 기본 문법 (1) SELECT
{{< highlight SQL >}}
SELECT * FROM Salaries LIMIT 10;
SELECT * FROM Salaries ORDER BY salary DESC LIMIT 10;

SELECT * 
FROM Salaries 
WHERE yearID = '2010'
AND lgID = 'AL'
ORDER BY salary DESC LIMIT 20;

--SUM, AVG
SELECT SUM(salary)
FROM Salaries
WHERE playerID = 'rodrial01';

--Concat, Count, Group By
SELECT nameFirst || ' ' || nameLast AS name FROM People Limit 10;
SELECT nameFirst || ' ' || nameLast AS name FROM People Where playerID = 'rodrial01';
SELECT COUNT(DISTINCT(nameFirst || ' ' || nameLast)) FROM People;
SELECT nameFirst || ' ' || nameLast AS name, COUNT(*) FROM People GROUP BY name HAVING COUNT(*) > 1;

SELECT
    teamID,
    SUM(Salary) as total_salary
FROM Salaries
GROUP BY teamID
ORDER BY total_salary DESC;
{{< /highlight >}}

#### SQL 기본 문법 (2) JOIN
{{< highlight SQL >}}
--Join
SELECT
    t2.nameFirst ||' '||t2.nameLast AS name,
    t1.salary
FROM
    Salaries t1
JOIN
    People t2 ON t2.playerID = t1.playerID
ORDER BY salary DESC
LIMIT 20;

--Quiz. Top paid player for each team in 2010
SELECT
    t1.teamID,
    t2.nameFirst||' '||t2.nameLast AS name,
    t1.salary --using MAX(salary) instead of ORDER BY would be more efficient
FROM
    Salaries t1
JOIN
    People t2 ON t2.playerID = t1.playerID
WHERE
    t1.yearID = '2010'
GROUP BY
    teamID
ORDER BY
    salary DESC;
    
-- Left Join, Right Join
SELECT t1.playerID, COUNT(*)
FROM People t1
LEFT JOIN AllstarFull t2 ON t2.playerID = t1.playerID
GROUP BY 1
ORDER BY COUNT(*) DESC
LIMIT 20;
{{< /highlight >}}

#### SQL 기본 (3) 데이터 타입들 및 키 값들, 테이블 생성
Primary Key: 빠른 처리, 중복 처리를 위해서 설정하기도 함!
Foreign Key: 다른 테이블에서 온 칼럼 처리
Unique: 중복 처리 방지
{{< highlight SQL >}}
-- 데이터베이스 테이블 생성
CREATE TABLE mytable (id INT, name VARCHAR(255), debut DATE);
CREATE TABLE mytable2 (id INTEGER PRIMARY KEY AUTOINCREMENT, name VARCHAR(255), debut DATE);
INSERT INTO mytable2 (name, debut) VALUES ('jiwoo', ' 2000-09-01');
SELECT * FROM mytable2;

--INSERT
INSERT INTO mytable2 (name, debut) VALUES ('jiwoo', '2000-09-05');
SELECT * FROM mytable2;

--Update 
UPDATE mytable2
SET debut = '2010-09-01'
WHERE id = 1;

--Replace
REPLACE INTO mytable2 (id, name, debut) VALUES (5, 'jiwoo2', '2015-09-01');
-- Update는 기존의 값이 없다면 아무런 행동도 하지 않지만, Replace는 기존의 값이 없다면 새로 만들어버린다는 차이점이 있다.
 
--Insert Or Ignore
INSERT OR IGNORE INTO mytable2 (id, name, debut) VALUES (1, 'jiwoo3', '2010-09-11');
--이미 id가 1인 행이 있을 경우, 그냥 insert into만 하면 'unique constraint failed'가 뜨지만 or ignore을 추가해주면 괜찮다.

-- Delete, ALter, Drop
-- 아주아주 신중하게 써야하는 커맨드들이다!
SELECT * FROM mytable2;
Delete FROM mytable2 WHERE id=1;

ALTER TABLE mytalbe2 RENAME TO players;
ALTER TABLE players ADD COLUMN DOB date;
SELECT * FROM players;

DROP TABLE mytable;
{{< /highlight >}}

#### SQL 기본 (4) Functions
{{<highlight SQL>}}
-- Functions(1) 기본처리 및 연산
SELECT * FROM players;
SELECT SUBSTR(name, 1, 3) FROM players;
SELECT UPPER(name) FROM players;
SELECT AVG(LENGTH(name)) FROM players; --MAX, AVG, COUNT, SUM

-- Functions(2) 날짜데이터, Case When
SELECT CURRENT_TIMESTAMP; --UTC기준
SELECT DATE('NOW');
SELECT DATETIME(CURRENT_TIMESTAMP, '+1 DAY');

SELECT
    id,
    name,
    CASE WHEN
        name = 'jiwoo' THEN 'OK'
    WHEN name = 'jiwoo2' THEN 'OK2'
    ELSE 'No OK'
    END AS ok_name, --CASE WHEN부터 여기까지가 variable 하나!
    debut
FROM players;


{{</highlight>}}

<br>