---
layout: post
title: "[CSS] 9. transition"
subtitle: "9. transition"
categories: CSS
tags: css
comments: true
date: "2021-09-14"
related_posts:
published: true
---

# 9. transition

### transition (전환) 개요

- 전제 조건 : (A -> B) 로 시간차를 두고 전환될 때
- 한 순간에 바뀌는 것이 아닌 시간차를 두고 스르르륵 변경될 때
- 시간이란 개념이 새롭게 등장한다.


<br>

### property, transition-duration

```
<div class="box>
</div>

.box {
    width: 300px;
    height: 80px;
    background-color: green;
    font-size: 50px;
    color: white;

    transition-property: all;
    transition-duration: 2s;
    // 모든 property가 2초동안 천천히 변경됨.
    // 마우스를 올리거나 내릴 때 둘 다 적용

    transition-property: background-color;
    transition-duration: 2s;
    // background-color 만 천천히 변함 (다른요소는 휙 바뀐다.)
}

.box:hover {
    background-color: red;
    width: 200px;
    color: black;

    // .box 가 아닌 여기서 선언된다면?
    transition-property: all;
    transition-duration: 2s;
    // 마우스르 올릴 땐 동작하지만 마우스를 내릴 땐 적용되지 않는다.
}
```

- A -> B 로 변경된다 === A의 css 에서 B의 css 로 변경된다.
- 모든 css 가 변경되는 것이 아닌 특정 css 만 변경된다. : transition-property
- 얼마만큼의 시간동안 변경된다 : transition-duration
- `transition-property: none` 아무것도 변경하지 않겠다.
- `transition-property : all` 가지고 있는 모든 css를 변경하겠다.
- `transition-property : margin-right, color `
- `<time>` 자료형 : s (초), ms (0.5s === 500ms)
- `transition-duration: 500ms`
- `transition-duration: 2s`

<br>


### delay, transition-timing-function

```
<div class="box>
</div>

.box {
    width: 300px;
    height: 80px;
    background-color: green;
    font-size: 50px;
    color: white;

    transition-property: height;
    transition-duration: 2s;
    transition-delay: 1s;
    transition-timing-function: linear // 등속도로 변경된다.
}

.box:hover {
    background-color: red;
    height: 200px;
    width: 200px;
    color: black;
}
```

- transition-delay
- 무언가 transition 되는 것을 지연하는 것
- `<time>` 사용
- transition-timing-function : 순차적으로 변경되는 부분을 똑같은 속도 or 처음 빠르게 or 뒤에 빠르게
- `transition-timing-function: ease` defaault
- ease, ease-in, ease-out, ease-in-out, linear, cubic-bazier(0.2, -2, -.8, 2)

<br>

### transition (shorthand)

```
<div class="box>
</div>

.box {
    width: 300px;
    height: 80px;
    background-color: green;
    font-size: 50px;
    color: white;

    transition-property: height;
    transition-duration: 2s;
    transition-delay: 1s;
    transition-timing-function: linear // 등속도로 변경된다.

    transition: height 2s linear 1s // shorthand
    transition: height 2s 1s linear // 뒤에는 순서 바뀌어도 동작은 하지만 위의 순서가 더 좋다.
    // 무조건 첫번째는 property (어떤 속성이 변하는지 가시적으로 보이니까)
    // property > duration > timing-function > delay
}

.box:hover {
    background-color: red;
    height: 200px;
    width: 200px;
    color: black;
}
```


- 대부분 shorthand 로 작성한다.
- 순서가 중요하다 > time 이 2개라서
- duration > delay 순서로 적용된다.
- `transition: margin-left 4s ease-in-out 1s`

<br>

### transition + trnasform 활용 예

```
<div class="box>
안녕~
</div>

.box {
    width: 100px;
    height: 100px;
    background-color: orage;
    border-radius: 30px;

    tansition: all 3s ease-in-out;
}

.box:hover {
    transform: rotate(360deg) translate(30px, 30px)
    trasform-origin: bottom right;
}
```

<br><br>


## 나의 회고 🤫
transform, transition은 내가 순수하게 css를 짜면서 활용할 기회가 별로 없이, 다른 코드를 유지보수할 때 볼 일이 많았다.<br>
그래서 항상 제대로 공부하지 않고, 이거겠거니~ 하고 넘겼었는데 오늘 제대로 알고 갈 수 있어서 좋았다!<br>
이제 transform, trnasition을 마주쳐도 겁먹지 않고 이해하고 활용할 수 있겠다<br>
신난다...!!!!<br>
이렇게 조금씩 성장하는 걸수도...?(뿌듯)😳😳