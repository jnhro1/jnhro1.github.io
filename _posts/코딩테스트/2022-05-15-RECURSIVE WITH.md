---
layout: post
title: "[Level4] RECURSIVE WITH"
subtitle: "RECURSIVE WITH"
categories: 코딩테스트
comments: true
tags: SQL
---

<br>

## 입양 시각 구하기(2)


[programmers](https://programmers.co.kr/learn/courses/30/lessons/59413) <br>

<br>

```sql
WITH RECURSIVE TEMP AS (
        SELECT 0 AS NUM
     UNION ALL
        SELECT NUM+1 FROM TEMP
         WHERE NUM < 23)


   SELECT T.NUM AS HOUR, COUNT(O.ANIMAL_ID) AS COUNT
     FROM TEMP T
LEFT JOIN ANIMAL_OUTS O
       ON T.NUM = HOUR(O.DATETIME)
 GROUP BY T.NUM
 ORDER BY T.NUM
```

<br>

