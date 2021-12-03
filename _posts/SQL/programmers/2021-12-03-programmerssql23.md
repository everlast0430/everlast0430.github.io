---
title: "DATETIME에서 DATE로 형 변환"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59414](https://programmers.co.kr/learn/courses/30/lessons/59414)

문제명 : DATETIME에서 DATE로 형 변환

문제 : ANIMAL_INS 테이블에 등록된 모든 레코드에 대해, 각 동물의 아이디와 이름, 들어온 날짜1를 조회하는 SQL문을 작성해주세요. 이때 결과는 아이디 순으로 조회해야 합니다.

SQL : MySQL

```sql
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS '날짜'
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

다음의 그림과 같이 DATETIME을 형변환 해달라고 하였다.

"DATE_FORMAT"을 이용해 변환할 수 있다!

![image](https://user-images.githubusercontent.com/43924464/144532616-b7daf49f-2b1a-4f56-b1fa-43f9b8c45990.png)

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144532598-baf4f1f5-d7fe-4a2a-ad43-3c7302c54954.png)
