---
title: "루시와 엘라 찾기"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59046](https://programmers.co.kr/learn/courses/30/lessons/59046)

문제명 : 루시와 엘라 찾기

SQL : MySQL

```sql
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
FROM ANIMAL_INS
WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
```

동물 보호소에 들어온 동물 중 이름이 Lucy, Ella, Pickle, Rogan, Sabrina, Mitty인 동물의 아이디와 이름, 성별 및 중성화 여부를 조회하는 SQL 문을 작성해야된다.

WHERE절에 IN을 사용하여 WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty') 해주었다!

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144528928-1463ac37-2f01-420c-8245-aa1b1d3ece08.png)
