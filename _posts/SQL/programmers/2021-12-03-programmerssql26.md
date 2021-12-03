---
title: "오랜 기간 보호한 동물(1)"
excerpt:

categories:
  - sqlcote
tags:
  - [코딩테스트, sql]

toc: true
toc_sticky: true

date: 2021-12-03
last_modified_at: 2021-12-03
---

사이트명 : 프로그래머스(Programmers)

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59044](https://programmers.co.kr/learn/courses/30/lessons/59044)

문제명 : 오랜 기간 보호한 동물(1)

문제 : 아직 입양을 못 간 동물 중, 가장 오래 보호소에 있었던 동물 3마리의 이름과 보호 시작일을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 시작일 순으로 조회해야 합니다.

SQL : MySQL

```sql
SELECT INS.NAME, INS.DATETIME
FROM ANIMAL_INS AS INS
LEFT JOIN ANIMAL_OUTS AS OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE OUTS.DATETIME IS NULL
ORDER BY INS.DATETIME ASC
LIMIT 3
```

아직 양을 못 간 동물의 경우 ANIMAL_OUT 테이블의 DATETIME이 NULL일 것이다 (입양기록이 없으니까). 따라서 두 테이블을 조인 후, WHERE 절에 "WHERE OUTS.DATETIME IS NULL"해서 OUTS 테이블의 입양기록이 없는 조건으로 검색후 ORDER BY에서 INS.DATETIME해서 가장 오래 머물렀던 동물 3마리를 찾으면 된다!

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144545614-2946069a-2a47-4bd8-9a0e-bf523f80ba1a.png)
