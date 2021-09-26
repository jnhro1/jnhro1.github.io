---
layout: post
title:  "[JS] 4. Iteration"
subtitle:   "반복문"
categories: JS
comments: true
---

- 목차
  - [1. 반복문 for](#.반복문for)
  - [2. 반복문 while](#.반복문while)
  - [3. 반복문 제어](#.반복문제어)
  - [4. 연습문제](#.연습문제)

<br>

## 1. 반복문 for

````
for (let i = 0; i < 3; i++) {
  console.log(i); // 0 1 2
}

for (let i = 10; i < 0; i++) {
  console.log(i); // 아무 수행도 안한다.
}

// 선언부를 앞에서 미리 함
let num = 0;
for (; num < 2; ) {
  console.log(num);
  // 증감식이 없기 때문에 계속 값이 같음 > 무한루프
  num++;
  // 증감식의 역할을 내부에서 해줄 수 있다.
}
````

- 선언문(Init Expression), 조건문(Test Expression), 증감문(Update Expression) 형태로 이루어진 반복문
- 조건문이 fail이 되기 전까지 코드 블록을 계속적으로 반복 수행
- 선언문, 조건문, 증감문 자리에 공백 입력 가능


<br>

### 2중 for문

````
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(`${i} + ${j} = ${i + j}`)
  }
}

0 + 0 = 0
0 + 1 = 1
0 + 2 = 2
1 + 0 = 1
1 + 1 = 2
1 + 2 = 3
2 + 0 = 2
2 + 1 = 3
2 + 2 = 4
````
<br>

### for...in 반복문

```
// Syntax
for (key in object) {
    // code block to be executed
}
```


````
const person = { name: "nahyun", age: 25, height: 160 };

for (let x in person) {
    // person 객체의 key 값들을 순차적으로 출력
  console.log(person[x]);
}
//
nahyun
25
160
````


- 객체의 key, value 형태를 반복하여 수행하는데 최적화 된 유형
- 첫번째부터 마지막까지, 객체의 키 개수만큼 반복

<br>

### for...of 반복문

```
// Syntax
for (variable of iterable) {
    // code block to be executed
}
```

````
let lang = "helloworld"
let text = ""

for (let x of lang) {
    console.log(x)
    text += x
}

console.log(text)

// h
// e
// l
// l
// o
// w
// o
// r
// l
// d
//helloworld
````

- Collection 객체 자체가 Symbol.iterator 속성(property)를 가지고 ㅣㅇㅆ어야 동작 가능한 유형
- ES6에 새로 추가된 Collection 기반의 반복 구문

<br>
<br>

## 2. 반복문 while

```
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
  // 0 1 2
}

let j = 0;
do {
  console.log(j);
  j++;
} while (j < 3);
// 0 1 2

let k = 4;
do {
  console.log(k);
  k++;
} while (k < 3);
// 조건이 false 여도 최초 1회 수행된다

```

- 조건문이 참일 때 코드 블록을 계속해서 반복 수행하는 반복문
- for 문에 비해 선언문과 증감문 없이 loop 를 수행하며, 무한 loop 등 수행 시 많이 사용
- 조건문을 코드 블록보다 아래로 옮긴 do...while 반복문도 존재 (최초 한번 수행이 필요할 때 많이 사용)
- 변수값의 update가 중요하다. 안하면 무한루프!
- while 보다는 for 사용 지향

<br>
<br>

## 3. 반복문 제어

### break

````
let text = ""

for (let i = 0; i < 10; i++){
    if (i == 3) break
    text += i
}

console.log(text)
// 012
````


- 반복문 수행 시 코드 블록을 탈출할 때 사용되는 식별자
- 다중 반복문일 경우 가장 안쪽의 반복문을 종료
- Label을 통하여 다중 반복문을 한번에 종료 가능
- Label : 반복문 앞에 콜론과 함께 쓰이는 식별자

<br>

### continue

````
let text = ""

for (let i = 0; i < 10; i++){
    if (i == 3) continue
    text += i
}

console.log(text)
// 012456789
````

- 반복문 수행 시 코드 블록 실행을 해당 라인에서 중지하고, 블록 코드를 종료 시킨 후 반복문 내 명시된 조건 판단
- 특정 조건에 뒷부분을 skip 하고 싶을 때 사용

<br>

### Label

```
for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(i + "+" + j + " = " + (i + j));
    break;
  }
}
// j === 0 일때만 break 되어 i 반복문 수행
// 0+0 = 0
// 1+0 = 1
// 2+0 = 2

end: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    console.log(i + "+" + j + " = " + (i + j));
    break end;
  }
}

//0+0 = 0
```

- 프로그램 내 특정 영역을 지정하여 별도 이름을 붙이는 식별자
- break 와 continue 를 사용하는 반복문 안에서만 사용 가능하며, break 나 continue 지시자 위에 있어야 한다.
- c언어의 goto 역할
- 2중 for 문에서 전체 반복문에서 종료하고 싶을 때
- 가독성과 로직을 망친다는 말이 있어서 되도록 사용 자제하자

<br>
<br>

## 4. 연습문제
### 문제 1 
- 반복문 for를 이용하여 0부터 10까지의 정수 중 짝수의 합을 구한 뒤 출력해주는 코드를 작성하시오


```
const UNTIL_NUM = 10;
let sum = 0;

// 구현 필요
// 1방법
for (let i = 0; i <= UNTIL_NUM; i+=2){
    sum += i
}

// 2방법
for (let i = 0; i <= UNTIL_NUM; i++) {
  if (i % 2 === 0) {
    sum += i;
  }
}

console.log(sum); // output: 30
```
<br>

### 문제 2
- 반복문 for 2개를 이용하여 2~9단까지 출력해주는 코드를 작성하시오

````
for (let i = 2; i <= 9; i++) {
  //구현 필요
  for (let j = 1; j <= 9; j++) {
    console.log(`${i} X ${j} = ${i * j}`);
  }
}

//
2 X 1 = 2
2 X 2 = 4
2 X 3 = 6
2 X 4 = 8
...
9 X 7 = 63
9 X 8 = 72
9 X 9 = 81
````


<br>
<br>

## 나의 회고 🤫
break 의 범위를 항상 헷갈려했는데 `다중 반복문` 일 경우에만 해당 블록에서 하나 나간다는 사실을 알게 되었다!!<br>
코딩 학원에서 수업할 때 애들에게 늘 애매하게 대답해줬는데 이제 확실히 알려줄 수 있게 되었다 🤩<br>
또 중첩된 반복문 전체에서 나갈 수 있는 label의 존재도 처음 알게 되어서 신기했다.(사용은 지양하라고 했지만...)