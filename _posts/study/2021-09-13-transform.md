---
layout: post
title: "[CSS] 8. transform"
subtitle: "8. transform"
categories: study
tags: css
comments: true
date: "2021-09-13"
related_posts:
published: true
---

# 8. transform

### transform (변형) 개요

- 원래 형태가 있는 그대로의 요소를 크기를 키우거나, 회전하거나 하는 것
- 기존이 갖고 있던 공간 자리는 그대로 유지한다.
- 다른 css 를 사용하여 요소를 변경하면 box 모델이 변경되지만 transform은 box 영역은 작아지지 않는다.
- 원본의 layout (width, height) 는 변경되지 않는다.
- 함수 표기법을 이용한다. `<transform-function>` 
- 여러 개의 함수 사용 가능 (오른쪽에서 왼쪽으로 함수 순차 적용)
- transform: none; (default)


<br>

### 크기 - scale

```
<img id="bolt">

#bolt {
    width: 400px;
    transform: scale(0.5)
    // 레이아웃은 400이지만 보이는 이미지 크기는 200
    // trnasform: scale(1.5, 0.5)
    // transform: scaleX(0.5)
    // transform: scaleY(1.5)
}
```

- `<transform-function>` x, y, z축 (3차원)
- (x, y)
- scale() : 2D로 크기를 조절 (3D x)
- scale(sx), scale(x,y) 매개값은 `<number>` (int, float)
- scaleX(0,5)
- scaleY(0.5)


<br>


### 회전 - rotate

```
<img id="bolt">

#bolt {
    width: 400px;
    transform: rotate(0.5turn) scale(0.5)
}
```


- scale은 값이 1 or 2개 가능하지만 rotate는 하나임
- 값은 `<angle>` 각도
- deg(45도), grad, red, turn
- `transform: rotate(45deg)` 45도 회전
- `transform: rotate(180deg)` 180도 회전
- `transform: rotate(-90deg)` -90도 회전

<br>

### 이동 - translate

```
<img id="bolt">

#bolt {
    width: 400px;
    transform: translate(100px, 0);
    trnasform: trnaslateX(150px); // x축으로만 150px
    trnasform: trnaslateY(150px); // y축으로만 150px
    trnasform: trnaslateX(-150px); // x축으로만 -150px
    trnasform: trnaslateX(50%, 30%); // 본인 가로의 50%, 세로의 30%만큼 이동
}
```


- 이동을 담당
- trnaslate(100px, 200px)
- trnaslate(200px) 하나만 입력하면 trnaslate(200px, 0px) 과 같다.
- `<number>, <percentage>`
- trnaslateX()
- translateY()

<br>

### 기울이기 - skew

```
<img id="bolt">

#bolt {
    width: 400px;
    transform: skewX(20deg) // 90deg 하면 완전히 찌부되서 안보인다. 좌우로 늘어난다.
    trnasform: skewY(45deg); // 위아래로 늘어난다.
    trnasform: skew(45deg, 45deg); // skewX(90deg) 와 같아서 완전 찌부
}
```


- `<angle>` deg, turn 사용 가능
- skew(ax) == skey(ax, 0)
- skew(ax, ay)
- skewX, skewY


<br>

### 기준점 - transform-origin

```
<img id="bolt">

#bolt {
    width: 400px;
    transform: scale(1.3);
    trnasform-origin: top left;
}
```


- 원점을 옮길 수 있다.
- 완전 별도의 property 이다. 
- like background-origin
- `trnasform-origin: center` (default)
- `trnasform-origin: top left` // 원점의 위치를 꼭지점으로 이동시킨다.
- `trnasform-origin: 50px 50px`
- 되도록 center로 두고 사용하는게 좋다.

<br>


## 나의 회고 🤫

평소에 사용할 기회가 별로 없어서 transform에 대해 잘 몰랐는데, 오늘 제대로 배울 수 있었다.<br>
주요 개념들은 기억해뒀다가 코드를 볼 때 활용할 수 있도록 해야겠다.