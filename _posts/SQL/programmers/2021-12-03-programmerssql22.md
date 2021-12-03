---
title: "오랜 기간 보호한 동물(2)"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59411](https://programmers.co.kr/learn/courses/30/lessons/59411)

문제명 : 오랜 기간 보호한 동물(2)

문제 : 입양을 간 동물 중, 보호 기간이 가장 길었던 동물 두 마리의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 보호 기간이 긴 순으로 조회해야 합니다.

SQL : MySQL

```sql
SELECT OUTS.ANIMAL_ID, OUTS.NAME
FROM ANIMAL_INS AS INS, ANIMAL_OUTS AS OUTS
WHERE INS.ANIMAL_ID = OUTS.ANIMAL_ID
ORDER BY OUTS.DATETIME - INS.DATETIME DESC
LIMIT 2
```

핵심은 ANIMAL_INS 테이블과 ANIMAL_OUTS 테이블의 DATETIME을 이용해 보호소에 얼마나 오래있었는지 계산할 수 있다. ORDER BY 절에 OUTS 테이블과 INS 테이블의 차를 구하면 된다.

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144531687-50cdc894-f4e2-4cc5-824b-7746dba947ae.png)
