---
title: "이름이 있는 동물의 아이디"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59407](https://programmers.co.kr/learn/courses/30/lessons/59407)

문제명 : 이름이 있는 동물의 아이디

SQL : MySQL

```sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
```

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144527695-5c164f80-af2e-4660-bea4-9f283c88f2ac.png)
