---
title: "이름에 el이 들어가는 동물 찾기"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59047](https://programmers.co.kr/learn/courses/30/lessons/59047)

문제명 : 이름에 el이 들어가는 동물 찾기

문제 : 보호소에 돌아가신 할머니가 기르던 개를 찾는 사람이 찾아왔습니다. 이 사람이 말하길 할머니가 기르던 개는 이름에 'el'이 들어간다고 합니다. 동물 보호소에 들어온 동물 이름 중, 이름에 "EL"이 들어가는 개의 아이디와 이름을 조회하는 SQL문을 작성해주세요. 이때 결과는 이름 순으로 조회해주세요. 단, 이름의 대소문자는 구분하지 않습니다.

SQL : MySQL

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
WHERE ANIMAL_TYPE = 'Dog' AND NAME LIKE '%el%'
ORDER BY NAME ASC
```

핵심은 동물의 TYPE은 Dog이고 이름은 어느 위치건 'el'이 들어가야하니까 '%el%'을 사용하였다.

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144529449-01dd336f-40cb-4c0b-9a90-52244d6e7ce8.png)
