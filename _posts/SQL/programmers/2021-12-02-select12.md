---
title: "고양이와 개는 몇 마리 있을까"
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

문제 주소 : [https://programmers.co.kr/learn/courses/30/lessons/59040](https://programmers.co.kr/learn/courses/30/lessons/59040)

문제명 : 고양이와 개는 몇 마리 있을까

SQL : MySQL

```sql
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) as count
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE ASC
```

GROUP BY를 사용하여

컬럼 값이 같은 것 끼리 하나로 묶어주자!

코드실행을 해보면 ANIMAL_TYPE으로 묶어놓으니 고양이 개 각각 15, 85마리씩 나온다

![image](https://user-images.githubusercontent.com/43924464/144368390-b8c76886-47f9-48d5-a051-da27fbc8ae45.png)
