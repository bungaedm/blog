---
collapsible: false
date: "2021-01-11T10:09:56+09:00"
description: 데이터 엔지니어 기초 다지기
draft: false
title: Part 2-2. 기초
weight: 2
---

## Part 2. 데이터 엔지니어 기초 다지기
본 포스팅은 패스트캠퍼스(FastCampus)의 **데이터 엔지니어링 올인원 패키지 Online**을 참고하였습니다.

### 3. SQLite Studio
[다운로드](https://sqlitestudio.pl/index.rvt)
[데이터 다운로드](https://baseballdb.lawlesst.net/)
editor 여는 법: Tools > Open SQL Editor (or Alt + E)
*SQLite과 MySQL을 포함한 다른 프로그램들과 코드가 다른 것들이 사소하게 있을 수 있다. 

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

<br>
<br>