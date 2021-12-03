---
title: "입양 시각 구하기(2)"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59413](https://programmers.co.kr/learn/courses/30/lessons/59413)

문제명 : 입양 시각 구하기(2)

SQL : MySQL

```sql
WITH RECURSIVE TEMP AS (
	SELECT 0 AS HOUR
	UNION ALL
	SELECT HOUR+1 FROM TEMP WHERE HOUR<23)

SELECT HOUR, COUNT(A.ANIMAL_ID)
FROM TEMP AS T
  LEFT JOIN ANIMAL_OUTS AS A
  ON T.HOUR = HOUR(A.DATETIME)
GROUP BY HOUR
ORDER BY HOUR

```

먼저 WITH RECURSIVE 문에 대한 이해가 필요하다.

1. 메모리 상에 가상의 테이블을 저장한다
2. 재귀 쿼리를 이용하여 실제로 테이블을 생성하거나 데이터삽입(INSERT)을 하지 않아도 가상 테이블을 생성할 수 있다.

```sql
WITH RECURSIVE 테이블명 AS(
  SELECT 초기값 AS 컬럼별명1
  UNION ALL
  SELECT 컬럼별명1 계산식 FROM 테이블명 WHERE 제어문
)
```

RECURSIVE 문을 통해 0~23시까지의 테이블을 만들어주고

기존테이블과 LEFT 조인을 통해서 합쳐준 후,

GROUP BY 및 COUNT를 사용해주면 된다!

![image](https://user-images.githubusercontent.com/43924464/144526955-60b620d2-0d7b-47f5-9fcb-9fded9f77ea2.png)
