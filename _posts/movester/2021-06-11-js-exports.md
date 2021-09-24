---
layout: post
title: "named exports, default exports 무슨 차이일까"
subtitle: "named/default exports"
categories: MOVESTER
tags: BACKEND
comments: true
date: "2021-06-11"
related_posts:
published: true
---

javascript에서 모듈을 exports (내보내기) 할 때는 2가지 방식이 있다.<br>
오늘은 named exports 와 default exports에 대해 알아보자.<br>
다음 두 코드의 차이점을 찾아보자.<br>

## named ? default? 이게 뭐야!

### 코드 A

```
const join = (req, res, next) => {
    const err = validationResult(req);
    if (!err.isEmpty()) {
        return res
            .status(statusCode.BAD_REQUEST)
            .json({ requestParameteError: err.array() });
    }
    next();
};

const login = (req, res, next) => {
    const err = validationResult(req);
    if (!err.isEmpty()) {
        return res
            .status(statusCode.BAD_REQUEST)
            .json({ requestParameteError: err.array() });
    }
    next();
};

module.exports = {
    join,
    login
};
```
<br>

### 코드 B

```
module.exports = {
    join: (req, res, next) => {
        const err = validationResult(req);
        if (!err.isEmpty()) {
            return res
                .status(statusCode.BAD_REQUEST)
                .json({ requestParameteError: err.array() });
        }
        next();
    },

    login: (req, res, next) => {
        const err = validationResult(req);
        if (!err.isEmpty()) {
            return res
                .status(statusCode.BAD_REQUEST)
                .json({ requestParameteError: err.array() });
        }
        next();
    }
};
```

<br>
두 코드는 같은 기능을 하는 함수인 join, login을 가지고 있다.<br>
여기서 다른 점은 <b>어떻게 이 함수들을 exports 하는가</b>이다.<br>
<br>
사실 나에겐 코드 A가 너무나도 당연했다.<br>
그러다 구글링을 통해 코드 B의 형태를 보고는 오잉? 스러웠다.<br>
처음 본 코드 B는 생긴게 간지나보인달까..?ㅋㅋㅋ<br>
<br>
자, 잡담은 그만하고 저 둘이 도대체 무엇이고 무슨 차이인지 알아보자.<br>
<br>
저 둘의 공식적인 이름은<br>
<br>
`코드 A : named exports`
<br><br>
`코드 B : default exports`
<br>
<br>
이다.
<br>

`named exports : CommonJS에서 내보낼 단일 객체를 module.exports 변수에 할당하는 방식 `
<br>
<br>
`default exports :ES6에 export default 키워드를 사용해서 명시적으로 선언하는 방식`

<br>
즉 default exports는 ES6 문법에서 새로 도입된 exports 방식이다.<br>
<br>
## 차이점 1. 하나의 모듈에서 몇 개가 존재할 수 있느냐

<b>하나의 모듈에서, named exports는 여러개 존재할 수 있지만, default exports는 단 하나만 가능하다.</b><br>

```
const exprotsTest = {
    join: (req, res, next) => {
        const err = validationResult(req);
        if (!err.isEmpty()) {
            return res
                .status(statusCode.BAD_REQUEST)
                .json({ requestParameteError: err.array() });
        }
        next();
    },

    login: (req, res, next) => {
        const err = validationResult(req);
        if (!err.isEmpty()) {
            return res
                .status(statusCode.BAD_REQUEST)
                .json({ requestParameteError: err.array() });
        }
        next();
    }
};

module.exports = exprotsTest;
```
<br>
위의 코드는 default exports에서 exprotsTest 단 하나만을 내보낸 경우이다.<br>
가장 상위의 exprotsTest만 하나일 뿐 이 안의 join, login 등의 함수는 복수개 작성 가능하다.<br>
<br>
named exports 가 복수개 exports 가 가능하다는 것은 위의 코드 A에서<br>
`module.exports = {
    join,
    login
};`
<br>
통해 복수개가 가능함을 볼 수 있다.<br>

## 차이점 2. 이름을 지정하는 것의 의무

두 exports의 naming에서도 볼 수 있듯이 <br><br>
`named : 이름 지정 가능(필수)`
<br><br>
`default : 이름 지정 안해도됨`
<br><br>
의 차이가 있다.<br><br>
물론 named 에서 이름을 지정하여 exports 했더라도 require가 받을 때 변수명을 다르게 지정하여 받으면 되니 무조건 named exports의 이름을 따를 필요는 없다.

## 그래서 뭐가 좋아?

사실 정답은 없다.<br>
개발자 본인이 어떤 것이 더 편한가에 달린 것 같다.<br>

다만 나의 경우 default exports 가 그저 간지나보여서 named exports이던 나의 소스코드들을 default exports로 바꾸다 보니 느낀 점이 있는데,<br>

>기존 const moduleName = {}의 구조에서 module.exports = {} 의 구조로 변경

- 별도의 이름을 지정해주지 않고 모듈을 exports 해주니 파일명을 다시 한번 살펴보지 않고는 내가 지금 어떤 모듈을 작업중인지 파악하기 어려웠다.<br>
<br>
- 또한 named exports에 경우 내가 이 함수를 만들 때 어떤 용도로 쓰일지 미리 이름을 정하여 exports 해준 반면, default exports가 되니 require을 받을 때마다 저 작업을 다시 생각하며 이름을 다시 지어줘야 했다.<br>
<br>
- 한 모듈 내에서 통일성을 가지기 위해
```
functionName1 : {}
functionName2 : {}
functionName3 : {}
  ```
의 구조를 했다가 functionName2에서 functionName1을 호출해야한다면 functionName1 은 const 나 let 으로 변수를 할당받은 것이 아니기 때문에 에러가 났다. <br>
- 마지막으로 이건 아주 개인적인 것이지만

```
const functionName = {}
const functionName = {}
const functionName = {}
```
의 구조가
```
functionName : {}
functionName : {}
functionName : {}
  ```
보다 함수의 시작과 끝을 알기 좋았다.<br>
<br><br>

그렇기 때문에 슬프지만<br>
named exports > default exports > named exports 과정을 거쳐
나의 소스들은 named exports로 통일 되었다.
