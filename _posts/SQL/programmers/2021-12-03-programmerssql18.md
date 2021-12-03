---
title: "NULL 처리하기"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59410](https://programmers.co.kr/learn/courses/30/lessons/59410)

문제명 : NULL 처리하기

SQL : MySQL

```sql
SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name'), SEX_UPON_INTAKE
FROM ANIMAL_INS
```

이름이 없는 동물의 경우 해당 동물의 이름을 'NO name'으로 출력해달라고 하였다.

이런경우 IFNULL을 써서 IFNULL(NAME, 'No name') 해주면 된다!

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144527874-8cf69c2a-d4d9-489a-9ab3-82e6105a26f3.png)
