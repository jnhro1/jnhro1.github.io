---
layout: post
title: "RESTful한 API 만들기"
subtitle: "REST API"
categories: MOVESTER
tags: BACKEND
comments: true
related_posts:
published: true
---

- 목차
  - [1. 기존 API path 살펴보기](#)
  - [2. http 메소드 활용하기](#)
  - [3. REST API 디자인 가이드](#)
  - [4. path parameter vs query string](#)

  <br>

REST API. 지난 기록에도 있는 개발에서 절대 빠질수 없는 것이다.<br>
REST API에 대한 내용은 
[[WEB] REST 란 무엇일까](https://jnhro1.github.io/web/2021/06/13/REST.html) 해당 포스팅에 자세히 설명했으니
생략하고 오늘은 movester의 api를 restful 한 api로 변경하는 과정을 알아보자.<br>

<br><br>

## 1. 기존 API path 살펴보기

시작에 앞서 먼저 반성부터 하고 가자.<br>
REST API를 만들겠다는 나의 초기 다짐과는 무색하게 나는 전혀..! REST API를 만들고 있지 않았다.<br>
지금까지 사용했던 수정 전 api path를 보자<br>

```
// notice
router.post("/notice/create", noticeCtrl.noticeCreate);
router.get("/notice/list", noticeCtrl.noticeList);
router.post("/notice/detail", noticeCtrl.noticeDetail);
router.post("/notice/update", noticeCtrl.noticeUpdate);
router.post("/notice/delete", noticeCtrl.noticeDelete);

// faq
router.post("/faq/create", faqCtrl.faqCreate);
router.get("/faq/list", faqCtrl.faqList);
router.post("/faq/detail", faqCtrl.faqDetail);
router.post("/faq/update", faqCtrl.faqUpdate);
router.post("/faq/delete", faqCtrl.faqDelete);
```

공지사항과 자주묻는질문 게시판과 관련된 api path들이다.<br>
학부생 시절 했던 프로젝트의 path를 아무 생각없이 그대로 쓰고 있었다.<br>
생성할 땐 create, 특정 하나만 볼 땐 deatil, 전체를 구할 땐 list...<br>
url 만 보고 데이터 리소스를 바로 알 수 있도록 하자는 REST API와는 맞지 않는다.<br>
왜냐...! http 메소드가 get 과 post 로만 이루어져있기 때문이다.<br>
REST API는 애당초 http 메소드 (get, post, put, patch, delete)를 통해 필요한 데이터 리소스를 파악하자는 데서 시작된 것이다.<br><br>

멍청했던 나는... path에 특정 id 가 붙으면 (query string) post, 아니면 get으로 구현하고 있었다.😭<br>
그래도 이제서야라도 깨달은 것을 감사히 여기며 오늘은 나의 REST API 규칙을 정하고, 그 규칙에 맞게 수정해나가도록 하자.<br>

<br><br>

## 2. http 메소드 활용하기

위의 notice를 기준 삼아 http 메소드의 적절한 사용처를 알아보자.<br>

|method|path|의미|기존 내 path|
|---|---|---|---|
|GET|/api/notices|모든 공지사항 조회|GET /notice/list|
|GET|/api/notices/{noticeIdx}|특정 공지사항 조회|POST /notice/detail|
|POST|/api/notices|새 공지사항 추가|POST /notice/create|
|PUT|/api/notices/{noticeIdx}|특정 공지사항 갱신|POST /notice/update|
|DELETE|/api/notices/{noticeIdx}|특정 공지사항 삭제|POST /notice/delete|

<br>

어느 부분이 중점적으로 변했는지 보이는가?<br>
우선, GET/POST 만 사용하던 http 메소드를 더 맞는 상황에 맞게 다양하게 사용했다.<br>
메소드를 적절하게 사용한 덕분에 url에서 동사로 설명해주던 create, update, list, detail, delete가 변경된 url에서는 필요가 없어졌다.<br>
또한 기존 notice가 notices로 복수형이 되었는데 이 이유는 아래에서 설명하겠다.<br>
RESTful 한 API로 변경하면서 조금 더 http 메소드를 활용하며 데이터 리소스 관계가 명확해진 것을 볼 수 있다.<br><br>

> 내가 놓쳤던 부분은 url 이 페이지 중심이 아닌 자원(데이터) 중심이었어야 했다는 점!

<br>

나는 이 페이지가 무슨 일을 하는가에 중점을 두고 path를 작성하고 있었던 것이다.<br>
반성하자😭😭<br>

<br><br>

## 3. REST API 디자인 가이드

### 1) 심플하고 직관적으로 하자.

### 2) 동사보다는 명사를 활용하자.
`POST /api/setNotices`
보다는
`POST /api/notices`
로 의미를 http 메소드로 파악하게끔 하자

### 3) 단수보다는 복수를 사용하자.
이 이유때문에 기존 notice 를 notices 로 변경하였다.<br>

<br>

## 4. path parameter vs query string

나는 수정 전에는 POST를 사용한 것은 전부 query string으로 받아왔다.<br>
하지만 수정된 url에서는 path parameter인 것을 볼 수 있을 것이다.<br>
이 둘은 api가 사용되는 목적에 따라 구별되어야 한다.<br><br>

- path parameter : 별도로 정제되지 않은 데이터를 가져올 때 (모든 리스트 조회, 특정 id 조회)
- query string : 별도로 정제한 데이터를 가져올 때 (filter, sort, search)

<br>

게시판에 관련된 api에서는 모두 정제되지 않은 데이터가 필요했기에 path 를 사용했다.<br>
스트레칭 목록을 filter해와서 조회할 때는 query string 을 활용해야 할 것이다!<br>

다음에는 status code를 수정하는 과정도 함께 알아보도록 하자!

