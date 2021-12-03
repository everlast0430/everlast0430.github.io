---
title: "중복 제거하기"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59408](https://programmers.co.kr/learn/courses/30/lessons/59408)

문제명 : 중복 제거하기

SQL : MySQL

```sql
SELECT COUNT(DISTINCT(NAME)) as count
FROM ANIMAL_INS
```
