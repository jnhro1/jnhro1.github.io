---
layout: post
title: "[Level2] String, Date"
subtitle: "String, Date"
categories: 코딩테스트
comments: true
tags: SQL
---

<br>

## 루시와 엘라 찾기


[programmers](https://programmers.co.kr/learn/courses/30/lessons/59046) <br>

<br>

```sql
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE
  FROM ANIMAL_INS
 WHERE NAME IN("Lucy", "Ella", "Pickle", "Rogan", "Sabrina", "Mitty")
```

<br>

## 이름에 el이 들어가는 동물 찾기

[programmers](https://programmers.co.kr/learn/courses/30/lessons/59047) <br>

<br>

```sql
  SELECT ANIMAL_ID, NAME
    FROM ANIMAL_INS
   WHERE NAME LIKE '%EL%'
     AND ANIMAL_TYPE = 'Dog'
ORDER BY NAME
```

<br>

## 중성화 여부 파악하기

[programmers](https://programmers.co.kr/learn/courses/30/lessons/59409) <br>

<br>

```sql
  SELECT ANIMAL_ID
        ,NAME
        ,(CASE WHEN SEX_UPON_INTAKE LIKE 'Neutered %' OR SEX_UPON_INTAKE LIKE 'Spayed %'
              THEN 'O' ELSE 'X' END) AS 'SEX_UPON_INTAKE'
    FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

<br>

```sql
 SELECT ANIMAL_ID
        ,NAME
        ,IF(SEX_UPON_INTAKE LIKE 'Neutered %' OR SEX_UPON_INTAKE LIKE 'Spayed %','O','X')
         AS 'SEX_UPON_INTAKE'
    FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

<br>

## 오랜 기간 보호한 동물(2)

[programmers](https://programmers.co.kr/learn/courses/30/lessons/59411) <br>

<br>

```sql
SELECT A.ANIMAL_ID, A.NAME
    FROM ANIMAL_INS A
    JOIN ANIMAL_OUTS B
   USING(ANIMAL_ID)
ORDER BY B.DATETIME - A.DATETIME DESC
   LIMIT 2
```

<br>

```sql
  SELECT A.ANIMAL_ID, A.NAME
    FROM ANIMAL_INS A
    JOIN ANIMAL_OUTS B
      ON A.ANIMAL_ID = B.ANIMAL_ID
ORDER BY B.DATETIME - A.DATETIME DESC
   LIMIT 2
```

<br>

```sql
  SELECT A.ANIMAL_ID, A.NAME
    FROM ANIMAL_INS A
    JOIN ANIMAL_OUTS B
      ON A.ANIMAL_ID = B.ANIMAL_ID
ORDER BY DATEDIFF(B.DATETIME, A.DATETIME) DESC
   LIMIT 2
```

<br>

## DATETIME에서 DATE로 형 변환

[programmers](https://programmers.co.kr/learn/courses/30/lessons/59414) <br>

<br>

```sql
  SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS '날짜'
    FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

<br>
