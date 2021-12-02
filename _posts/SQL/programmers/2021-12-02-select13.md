---
title: "동명 동물 수 찾기"
excerpt:

categories:
  - sqlcote
tags:
  - [코딩테스트, sql]

toc: true
toc_sticky: true

date: 2021-12-02
last_modified_at: 2021-12-02
---

사이트명 : 프로그래머스(Programmers)

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59041](https://programmers.co.kr/learn/courses/30/lessons/59041)

문제명 : 동명 동물 수 찾기

SQL : MySQL

```sql
SELECT NAME, COUNT(NAME) AS co
FROM animal_ins
WHERE NAME is not null
GROUP BY NAME
HAVING co >= 2
ORDER BY NAME ASC
```

동물 보호소에 들어온 동물 이름 중 두 번 이상 쓰인 이름과 해당 이름이 쓰인 횟수를 조회하는 SQL문을 작성해야된다.

이름을 COUNT하고 GROUP BY를 사용하여 같은 이름끼리 묵어주었다. 이때 2개 이상 출력해야하니까 HAVING절을 썻다.

마지막으로 이름이 없는 동물을 제외해야하니까 WHERE절에 NAME is not null을 하였다.

실행결과는 다음과 같다.

![image](https://user-images.githubusercontent.com/43924464/144372861-d4a4b4e8-b74d-4df1-a10c-967efc07d859.png)
