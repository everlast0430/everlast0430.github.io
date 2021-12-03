---
title: "있었는데요 없었습니다"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59043](https://programmers.co.kr/learn/courses/30/lessons/59043)

문제명 : 있었는데요 없었습니다

문제 : 천재지변으로 인해 일부 데이터가 유실되었습니다. 입양을 간 기록은 있는데, 보호소에 들어온 기록이 없는 동물의 ID와 이름을 ID 순으로 조회하는 SQL문을 작성해주세요.

SQL : MySQL

```sql
SELECT INS.ANIMAL_ID, INS.NAME
FROM ANIMAL_INS AS INS
INNER JOIN ANIMAL_OUTS AS OUTS ON INS.ANIMAL_ID = OUTS.ANIMAL_ID
WHERE INS.DATETIME > OUTS.DATETIME
ORDER BY INS.DATETIME ASC
```

입양보다 빠른날짜를 찾으면 된다! (INS.DATETIME > OUTS.DATETIME)

실행결과 화면 ⬇️

![image](https://user-images.githubusercontent.com/43924464/144545614-2946069a-2a47-4bd8-9a0e-bf523f80ba1a.png)
