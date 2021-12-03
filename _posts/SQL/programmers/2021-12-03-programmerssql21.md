---
title: "중성화 여부 파악하기"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59409](https://programmers.co.kr/learn/courses/30/lessons/59409)

문제명 : 중성화 여부 파악하기

문제 : 보호소의 동물이 중성화되었는지 아닌지 파악하려 합니다. 중성화된 동물은 SEX_UPON_INTAKE 컬럼에 'Neutered' 또는 'Spayed'라는 단어가 들어있습니다. 동물의 아이디와 이름, 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 이때 중성화가 되어있다면 'O', 아니라면 'X'라고 표시해주세요.

SQL : MySQL

```sql
SELECT ANIMAL_ID, NAME, if(SEX_UPON_INTAKE LIKE 'intact%', 'X', 'O') AS 중성화
FROM ANIMAL_INS
```

중성화 여부를 파악해달라고 하였다. 'intact%'로 시작하는 경우 중성화가 아니므로 if LIKE를 이용해 "if(SEX_UPON_INTAKE LIKE 'intact%', 'X', 'O') AS 중성화"로 작성하였다!

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144530529-2f419432-1678-40d1-a3af-4bd1f24c934d.png)
