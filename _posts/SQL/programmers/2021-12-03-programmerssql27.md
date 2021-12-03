---
title: "보호소에서 중성화한 동물"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59045](https://programmers.co.kr/learn/courses/30/lessons/59045)

문제명 : 보호소에서 중성화한 동물

문제 : 보호소에서 중성화 수술을 거친 동물 정보를 알아보려 합니다. 보호소에 들어올 당시에는 중성화되지 않았지만, 보호소를 나갈 당시에는 중성화된 동물의 아이디와 생물 종, 이름을 조회하는 아이디 순으로 조회하는 SQL 문을 작성해주세요.

SQL : MySQL

```sql
SELECT INS.ANIMAL_ID, INS.ANIMAL_TYPE, INS.NAME
FROM ANIMAL_INS AS INS
LEFT JOIN ANIMAL_OUTS AS OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.SEX_UPON_INTAKE LIKE 'intact%' AND
      OUTS.SEX_UPON_OUTCOME NOT LIKE 'intact%'
ORDER BY INS.ANIMAL_ID ASC
```

핵심은 'intect'로 시작하는 경우 중성화되지 않은경우 반대는 중성화된 경우이다.

보호소에 들어왔을 때, 중성화되지 않은 경우는 "INS.SEX_UPON_INTAKE LIKE 'intact%'"로 찾고
입양할 당시 중성화 된 경우는 "OUTS.SEX_UPON_OUTCOME NOT LIKE 'intact%'"로 찾으면 된다!

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144548990-23a790fc-3383-428b-b6b1-cfe4ee7af5a7.png)
