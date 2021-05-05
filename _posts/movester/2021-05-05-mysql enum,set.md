---
layout: post
title: "[DB] 06. mysql enum, set"
subtitle: "mysql enum,set"
categories: movester-server
tags: server
comments: true
date: "2021-05-05"
related_posts:
  - category/_posts/movester/2021-04-30-DB컨벤션1.md
  - category/_posts/movester/2021-05-01-DB컨벤션2.md
published: true
---

movester에서는 각 스트레칭 별로 자세와 효과를 각각 3개씩 지정할 수 있다.
여기서 자세와 효과를 별도의 테이블로 분리하여 나타낼 것인가. 그렇다면 한 스트레칭당
레코드의 수가 증가하고 테이블간의 의존도도 높아지니 고민이 생겼다.
이를 해결할 수 있을 법한 자료형으로 set을 발견하였다.
set과 enum은 매우 밀접하기에 enum 또한 설명하겠다.

## 1. ENUM

---

- 여러 값 중 허용된 값만 저장되게끔 제약을 걸어두는 것
- 오직 하나의 값만 선택 가능

## 2. SET

---

- 여러 값 중 허용된 값만 저장되게끔 제약을 걸어두는 것
- 정해진 리스트 내에서 중복되지 않는 여러 값을 가질 수 있는 string object

## 3. 비교

---

- ENUM은 하나의 값만 저장이 되는 반면 SET은 다중 값이 저장이 가능하다.
- 그렇다면 SET을 사용한다면 자세와 효과를 굳이 별도의 테이블로 저장해도 되지 않아도 될 것 같은데? 그게 더 효율적일까?

## 4. SET의 단점

---

- 최대 64개의 데이터를 담을 수 있다.
- 효율적인 인덱스를 사용하지 않으므로, 빈번하게 사용되는 질의의 조건으로 사용하는 칼럼이라면 정규화를 통해 별도의 relation 테이블을 구성하는 것이 좋다.

---

🔗 [enum 단점 레퍼](https://velog.io/@leejh3224/%EB%B2%88%EC%97%AD-MySQL%EC%9D%98-ENUM-%ED%83%80%EC%9E%85%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%90%EC%95%84%EC%95%BC-%ED%95%A0-8%EA%B0%80%EC%A7%80-%EC%9D%B4%EC%9C%A0)
🔗 [set 단점 레퍼](https://judekim.tistory.com/53)
🔗 [set 활용법 레퍼](https://yahwang.github.io/posts/32)

---

## 5. 나의 경우

1. 2개의 값으로 구분된다면 enum보다는 tinyint를 사용하자.
2. 정해진 것중 하나만 선택하는 경우 enum을 사용하자 (단점이 많다는데 이는 궈니에게 질의해봐야하는 문제같다.) db의 무결성을 보면 enum, 하지만 이는 비지니스 로직에서 충분히 컨트롤 가능해보인다.
3. set이 좋을지 별도 릴레이션이 좋을지 고민이다...
   그렇지만 set에 대한 레퍼가 별로 없고 자주 이용되지 않는다는 것을 봤을 때 그 장점이 특출나지 않기 때문인 것 같다.(인덱스 측면이라던가) 그래서 별도 참조 테이블을 만드는 것이 더 좋아보이긴 한다.
