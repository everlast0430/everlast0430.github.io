---
title: "이름이 없는 동물의 아이디"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59039](https://programmers.co.kr/learn/courses/30/lessons/59039)

문제명 : 이름이 없는 동물의 아이디

SQL : MySQL

```sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NULL
```

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144527540-4420aeba-2761-4be4-b635-0a135acfaedf.png)
