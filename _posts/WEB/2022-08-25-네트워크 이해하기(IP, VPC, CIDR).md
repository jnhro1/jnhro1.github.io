---
layout: post
title: "[WEB] 네트워크 이해하기(IP, VPC, CIDR)"
subtitle: "router"
categories: WEB
tags: web
comments: true
---


## #1. IP

- internet protocal
- 컴퓨터가 연결된 네트워크 끝단의 주소
- 기기가 인터넷에 접속한 곳의 네트워크상 위치

<br><br>

### IPv4

255.255.255.255 ⇒ 256^4 ⇒ 46억개

세계인구는 78억

전세계에서 ip 주소 안겹치고 사용할 수가 없어!

- 공인 ip (public ip)
    - 서울시 개발구 코딩로 12 케어닥 아파트
    - 케어닥의 공인 ip내에서 직원들은 사설 ip로 할당받음
    - 일부만 public > ELB 연결하여 통신
        - ELB에는 ALB(L7), NLB(L4)
- 사설 ip (private ip)
    - 101동 202호, 102동 304호
    - 특정 컴퓨터들을 하나의 공통된 공인ip에 묶어서 그 안에서만 안 겹치는 사설 ip를 각각 줌
    - 내부 네트워크 내에서 위치를 찾아갈 때 사용
    - 사설 ip에서 다른 공인 ip로는 접근 가능
    - 다른 공인 ip에서 사설 ip로는 접근 불가능 (6동 404호 가지고 어떻게 찾아!)
    - 사설ip에서 웹사이트 실행시키는 법
        - 포트포워딩 : 원하는 기기에다 포트를 하나씩 줌
            - 공인아이피:101, 공인아이피:104 …
        - DMZ : 모든 포트를 싸그리 한 군데 몰빵
    - 사설 IP 대역 (RFC1918)
        - 인터넷을 사용하지 않고 우리끼리 사용하는!
    - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
    - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
    - 192.169.0.0 - 192.168.255.255 (192.168.0.0/16)

- 고정 ip
    - 서버 같은 곳에 사용
    - 비쌈
    - 한국은 1억개의 ip를 가지고 있음
- 유동 ip
    - 일반 가정이나 기기들에는, 주기적으로 ip를 회수해서 인터넷을 사용 중인 곳에만 나눠줌
    - 놀고있는 ip는 회수
    - ip가 이따금씩 변경됨 (해킹으로부터 안전)
    - 유동 ip인 공인 ip를 썼다면..? 매번 변경해줘야해??
        - DDNS(dynamin DNS)
            - 수시로 바뀌는 유동 ip를 조회해서 고정된 도메인에 연결해줌

<br><br>

### IPv6

- 16진수 4개가 8개로 연결
- 2^128, 3.4*10^38 개 고정 ip 사용 가능
- 1234:5678:9ABC:DEF0:1234:5678:0ABC:DEF0

<br><br>

## #2. IP 쪼개기 (클래스 < 사이더)

[IP주소 클래스 종류(A클래스, B클래스 등등..)란 무엇인가?](https://jhnyang.tistory.com/503)

[[네트워크] CIDR이란?(사이더 란?)](https://kim-dragon.tistory.com/9)

- 이제는 클래스 방식보다 사이더를 선호함 (ip대역대를 더 세부적으로 쪼갤 수 있어서)

<br><br>

## #3. VPC

- Virtaul Private Cloud
- 독립된 하나의 네트워크를 구성하기 위한 가장 큰 단위
- 🤔 이 대역대가 VPC 에 포함되어 있는지 아닌지를 구분할 정도의 공부 필요!
    - bottle rocket: 컨테이너용 리눅스 os (운영 자동화 관점에서 컨테이너 환경에 최적화)

<br><br>

### 서브넷

- VPC 를 쪼개어 놓은 것
- 각각의 서브넷의 대역대가 VPC 에 포함되어 있도록 설계해야함!
- private이 public보다 더 많은 대역대를 갖도록 설정 (for 보안)
- public subnet
    - 인터넷에 연결되어 있음
- private subnet
    - 인터넷에 연결되어 있지 않음
    - 나누어놓은 이유는? for 보안 강화
    - 그러면 private subnet은 외부 통신 못해?
        - 가능하게끔 만든 것이 NAT gateway!

<br><br>

### 라우팅 테이블

- 각각의 서브넷의 요청을 라우팅할 테이블 작성
- 보통 public, private 서브넷을 위해 각각 설정
- public은 0.0.0.0 이 IGA 로
- private은 0.0.0.0 이 NAT 게이트웨이로

<br><br>

### NAT 게이트웨이

- private 서브넷의 요청을 외부와 연결하기 위한 게이트웨이
- public 서브넷 내부에 생성해야함
- private 요청을 public으로 넘겨줘서 소통함 (public이 다리 역할)
- EIP(탄력적 IP)가 필요한 시점! (고정 IP임)

[[AWS강좌]6.VPC(1)](https://www.youtube.com/watch?v=FeYagEibtPE)

<br><br>

## #4. 실습해봅시다!

1. AWS 계정에 VPC 생성하기
2. public/private 서브넷 각각 2개씩 생성
3. 대역대 알아서 쪼개보기
4. 라우팅 테이블, 인터넷 게이트웨이, NAT 게이트웨이, EIP 붙여보기

![Untitled](/assets/img/study/cidr.png)