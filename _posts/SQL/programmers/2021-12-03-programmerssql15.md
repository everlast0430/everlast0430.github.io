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
SELECT HOUR(DATETIME) AS HOUR, COUNT(*)
FROM ANIMAL_OUTS
WHERE HOUR(DATETIME) BETWEEN '9' AND '19'
GROUP BY HOUR
ORDER BY HOUR ASC

```

문제를 읽어보면 09:00부터 19:59까지 시간대별 입양 건수를 뽑아야한다

먼저 HOUR라는 함수를 이용해 DATETIME 컬럼을 시간대별로 출력할 수 있다. 이를 이용해 WHERE절에 BETWEEN을 써서 09:00~19:59 범위를 잡아주고

GROUP BY를 이용해 시간대별 카운트 개수를 출력하면 된다

![image](https://user-images.githubusercontent.com/43924464/144375916-4ea3cb1a-bcf5-4cb6-b90e-c800dca9fed9.png)
