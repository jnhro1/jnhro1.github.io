---
layout: post
title: "[WEB] GraphQL 훑어보기"
subtitle: "GraphQL 훑어보기"
categories: WEB
tags: web
comments: true
---

## SQL vs GQL

### 목적

- SQL: 데이터베이스 시스템에 저장된 데이터를 효율적으로 가져오는 것
- GQL: 웹 클라이언트가 데이터를 서버로 부터 효율적으로 가져오는 것

<br>

### 호출

- SQL: 주로 백엔드 시스템에서 작성 및 호출
- GQL: 주로 클라이언트 시스템에서 작성 및 호출

- GQL은 **API를 위한 쿼리 언어**이며, **타입 시스템**을 사용하여 쿼리를 실행하는 **서버사이드 런타임**이다.
- GraphQL은 특정한 **데이터베이스**나 **스토리지**에 귀속되어 있지 않으며, **기존 코드와 데이터에 의해 대체**된다.

<br><br>

## REST API 의 한계

- REST API는 모든 리소스들을 하나의 endPoint에 연결하고, 연결된 endPoint는 리소스와 관련된 내용만 관리하게 하는 것
- 이는 단순한 서비스에서는 좋지만, 복잡한 서비스나 클라이언트 요청에 따라 Over-Fetching과 Under-Fetching이 발생할 수 있다.
- 리소스별로 미세하게 다른 endPoint를 갖도록 구현하는 것은 어렵다 > 비슷하지만 endPoint가 다른 API 가 많이 파생된다.

<br>

### Over-Fetching

- 사용자의 데이터를 조회하는 `GET /api/users/:id`
- 나는 name 만 필요한데, name, gender, create_at 등 더 많은 정보를 주는 경우가 발생

<br>

### Under-Fetching

- 요청에 맞게 유효한 데이터를 보여주기 위해 여러 API 를 호출해야 하는 경우
- 뭅스터 admin에서 유저 상세정보 조회시, `GET /api/users/:id` `GET /api/records/:id` 등 여러 API 를 호출해야하는 경우가 발생

<br><br>

## GraphQL

- 위의 REST API 의 한계를 극복하고자 등장
- endPoint는 통상 1개만 생성하고 클라이언트에게 필요한 데이터는 클라이언트가 직접 쿼리를 작성, 호출하여 반환 받도록 한다.

<br>

### 위의 Over-Fetching 해결하기

```json
query {
	user(user_idx:1){
				name
	}
}
```

<br>

### 위의 Under-Fetching 해결하기

```json
query {
	user(user_idx:1){
				name
				create_at
				id
	}
	record(user_idx:1){
				score
	}
}
```

<br>

### 장점

- 클라이언트가 필요한 데이터만 반환받을 수 있다.
- 1번의 호출로 원하는 데이터를 한번에 가져올 수 있다. (서버 리소스 최적화, 클라이언트의 요청에 대한 부담 감소)
- 서버에서 할 수 있는 일을 스키마 단위로 파악 가능
- 확장에 용이하다.

<br>

### 단점

- FE, BE 모두 어느정도의 러닝커브가 있다.
- 단순한 서비스에서는 사용하기에 복잡하다. (서버와 클라이언트 중간에 GraphQL이라는 Service Broker 레이어가 추가 > 기존 클라이언트에서 데이터를 조합하던 역할이 GraphQL로 옮겨짐 > 역할의 분리 > 유지보수는 쉬워지고, 구현은 복잡해진다.)
- 캐싱 기능의 구현이 복잡하다.
- 요청이 text 로 날아가기 때문에 파일 전송 등을 구현하기가 어렵다.

<br><br>

## 어떨 때 사용할까

### GraphQL

- 서로 다른 모야으이 다양한 요청들에 대해 응답할 수 있어야 할 때
- 대부분의 요청이 CRUD 에 해당할 때

<br>

### REST API

- http 와 https 에 의한 캐싱을 잘 사용하고 싶을 때
- 파일 전송 등 단순한 text 로 처리되지 않는 요청들이 있을 때
- 요청의 구조가 정해져 있을 때

<br><br>

but! GraphQL and REST API 도 가능하다!

- 하나의 endPoint를 GraphQL용으로 만들고, 다른 REST API endPoint를 만들어놓는 것은 개발자의 자유다.
- 하나의 목표를 위해 두 API structure를 섞어놓는 것은 API 의 품질을 떨어뜨릴 수 있으므로 주의!
- ex) 사용자 정보를 등록할 때는 REST API, 수정할 때는 GraphQl 은 안돼!