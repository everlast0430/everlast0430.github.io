---
title: "아픈 동물 찾기"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59036](https://programmers.co.kr/learn/courses/30/lessons/59036)

문제명 : 아픈 동물 찾기

SQL : MySQL

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE INTAKE_CONDITION = 'Sick'
```
