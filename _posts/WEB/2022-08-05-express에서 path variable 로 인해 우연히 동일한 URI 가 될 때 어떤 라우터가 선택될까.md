---
layout: post
title: "[WEB] express에서 path variable 로 인해 우연히 동일한 URI 가 될 때 어떤 라우터가 선택될까?"
subtitle: "router"
categories: WEB
tags: web
comments: true
---


오늘도 제리의 한마디로 시작된 2시간의 라우터 공부….🙀

오늘의 주제는, `express 환경에서, REST API 의 URI가 path varibale 로 인해 우연히 동일해진다면 어떤 라우터가 호출될까` 입니다.

우리가 열심히 시간을 소비했으니,,, 이걸 보는 다른 사람들은 이 글을 통해 시간을 절약할 수 있기를 바라며 알게 된 것을 기록해봅니다🏝

<br><br>

![Untitled](/assets/img/study/router1.png)

`A 라우터`와 `B 라우터`는 `path variable`의 이름만 다르고 동일한 URI를 가진다.

오른쪽과 같이 api 가 요청됐을 때 선택되는 라우터는 먼저 선언된 `A 라우터` 이다. (절대 `B 라우터`는 선택될 수 없다.)

<br>

여기서 의문.

그렇다면 `B 라우터` 는 아에 실행자체도 되지 않는 것일까?

![Untitled](/assets/img/study/router2.png)

`B 라우터`의 console 이 찍히지 않았다.

<br>

![Untitled](/assets/img/study/router3.png)

`A 라우터`에서 next() 를 호출하니, `B 라우터`가 실행되었다.

클라이언트는 `A 라우터`의 response 를 받은 것을 확인하자.

또한 클라이언트측에서는 200 status 로 오류없이 받았지만 서버에서는 `A 라우터`의 response 후에 `B 라우터`에서도 response 하는 것을 할 수 없다며 에러를 나타내고 있다.

<br>

![Untitled](/assets/img/study/router4.png)

`A 라우터` 에서 response, next 를 하지 않았다.

클라이언트에서 response를 무한 대기한다. (timeout 시간까지)

또한 `B 라우터`는 실행되지 않았다.

<br>

`A 라우터` 에서 response를 하지 않고 next() 만 한다면?

<br>

![Untitled](/assets/img/study/router5.png)

`B 라우터`를 실행하며 response가 1개이므로 정상 동작한다.

<br><br>

### 중간 path variable 값이 우연이 일치한다면?

![Untitled](/assets/img/study/router6.png)

해당 경우 `A 라우터`가 호출된 것을 볼 수 있다.

<br><br>

`A 라우터` 와 `B 라우터`의  선언 위치를 바꾼다면?


![Untitled](/assets/img/study/router7.png)

이번엔 `B 라우터`가 호출되었다.

<br>

이를 통해 path variable 값이 우연히 일치해서 동일 URI를 가지게 될 때도 먼저 선언된 라우터가 호출되는 것을 알 수 있다 (변수로 인한 중복 매칭의 경우 ⇒ 위의 경우 fruits)

(이런 상황이 발생하지 않게끔 설계를 잘 해야한다!🤯)

(spring의 경우, 라우터의 선언 위치와 별개로 path variable 의 갯수가 적은 순(명시적 URI 갯수가 많은 순)으로 라우터가 호출된다고 한다. 참고 [https://okky.kr/article/546504](https://okky.kr/article/546504))

<br><br>

### 여기까지 결론

- path variable 변수명만 다르고 동일한 URI를 가진 REST API 는 먼저 선언된 라우터로 호출된다.
- 해당 라우터에서 next() 를 호출하지 않으면 해당 라우터 실행 후 종료된다.
- next() 를 호출하면, 동일한 URI를 가진 라우터가 순차적으로 호출된다.
- 두 라우터가 모두 response 를 한다면, 클라이언트는 첫번째 라우터의 응답값만 받고 서버에서는 에러가 난다.

<br><br>

### 번외. express에서 REST API는 대소문자를 구분하지 않는다?

![Untitled](/assets/img/study/router8.png)

![Untitled](/assets/img/study/router9.png)

APPLE 과 banana 의 대소문자를 바꿨음에도  `C 라우터`가 호출된다.

별도로 설정하지 않은 express 환경에서 REST API는 대소문자를 구분하지 않는다는 것을 확인할 수 있다.

(아래 공식문서를 보면 설정을 통해 대소문자를 구분하도록 변경 가능해보인다)

[https://expressjs.com/ko/api.html#express.router](https://expressjs.com/ko/api.html#express.router)

[http://daplus.net/url-url은-대소-문자를-구분해야합니까/](http://daplus.net/url-url%EC%9D%80-%EB%8C%80%EC%86%8C-%EB%AC%B8%EC%9E%90%EB%A5%BC-%EA%B5%AC%EB%B6%84%ED%95%B4%EC%95%BC%ED%95%A9%EB%8B%88%EA%B9%8C/)

<br><br>

### 나의 회고 🤫

node 는 왜 알면 알수록 새로울까…

나만 라우터에 이런 뻘짓(?)을 해본걸까? 😭

함께 고민한 제리가 말하길.. "애초에 이걸 고민하지 않는게 정배이거나 다른 사람들도 모르거나.."

후… 우선 2시간의 뻘짓을 통해 해당 경우에 라우터가 어떻게 동작하는지 명확하게 알았으니 오늘의 2시간은 값진걸로! (출근전 새벽이라는게 문제지만..)

아쉬운 것은 위의 행위의 근거를 찾지 못했다는거..? ㅠㅠ

가장 best 는 해당 사항을 고려하여 미리미리 URI 설계를 명확하게 하는 것이 좋은 것 같다!