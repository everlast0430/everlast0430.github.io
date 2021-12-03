---
title: "없어진 기록 찾기"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59042](https://programmers.co.kr/learn/courses/30/lessons/59042)

문제명 : 없어진 기록 찾기

문제 : 천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

SQL : MySQL

```sql
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_OUTS AS OUTS
LEFT JOIN ANIMAL_INS AS INS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.ANIMAL_ID IS NULL
```

두 테이블을 조인 후, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 조회해야되니까 WHERE 절에 "WHERE INS.ANIMAL_ID IS NULL" 입력하면 된다!

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144544784-ee24f15b-06b5-4826-ae0d-a5f0ec837cc7.png)
