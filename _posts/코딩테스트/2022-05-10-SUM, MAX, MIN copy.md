---
layout: post
title: "[Level2] SUM, MAX, MIN"
subtitle: "SUM, MAX, MIN"
categories: 코딩테스트
comments: true
tags: SQL
---

<br>

## 최솟값 구하기


[programmers](https://programmers.co.kr/learn/courses/30/lessons/59038) <br>

<br>

```sql
SELECT MIN(DATETIME) AS '시간'
  FROM ANIMAL_INS
```

<br>

## 동물 수 구하기

[programmers](https://programmers.co.kr/learn/courses/30/lessons/59406) <br>

<br>

```sql
SELECT COUNT(ANIMAL_ID) AS 'count'
  FROM ANIMAL_INS
```

<br>

## 중복 제거하기

[programmers](https://programmers.co.kr/learn/courses/30/lessons/59408) <br>

<br>

```sql
SELECT COUNT(DISTINCT NAME) AS 'count'
  FROM ANIMAL_INS
 WHERE NAME IS NOT NULL
```

<br>

## NULL 처리하가ㅣ

[programmers](https://programmers.co.kr/learn/courses/30/lessons/59410) <br>

<br>

```sql
  SELECT ANIMAL_TYPE, IFNULL(NAME, 'No name') AS NAME, SEX_UPON_INTAKE
    FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

<br>
