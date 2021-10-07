---
layout: post
title: "[JS] 6. Basic Object"
subtitle: "객체"
categories: JS
comments: true
---

- 목차
  - [1. method](#.method)
  - [2. Number](#.Number)
  - [3. String](#.String)
  - [4. 문자열 변환](#.문자열변환)

<br>

## 1. method

### 함수의 표현

```
function add1(x, y) {
  return x + y;
}

const add2 = function (x, y) {
  return x + y;
};

const add3 = (x, y) => x + y;


const add4 = add1
console.log(add4(1,3)) //4
console.log(add1 == add2) //false 선언된 메모리 영역이 다르기 때문
console.log(add1 == add4) //true 참조하는 주솟값이 같기 때문

console.log(Object.getOwnPropertyDescriptors(add1))
console.log(Object.getOwnPropertyDescriptors(add2))
console.log(Object.getOwnPropertyDescriptors(add3)) //다른 선언식보다 짧다
console.log(Object.getOwnPropertyDescriptors(add4)) //name[value]: 'add1'
```

- 다양한 방법으로 함수 정의가 가능하며, 함수 표현식처럼 함수를 정의하여 변수에 저장 가능

<br>

### 함수 저장

```
let list = [
  "jnh",
  25,
  function hello() {
    console.log("hello");
  },
];

let obj = {
    name: 'jnh',
    age: 25,
    hello() {
        console.log("hello")
    }
}

function hello() {
    console.log("hello")
}

hello() //hello
obj.hello() //hello
list[2]() //hello

console.log(typeof hello) //function
console.log(typeof obj.hello) //function
console.log(typeof list[2]) //function
```

- 배열의 요소(element) 혹은 객체의 속성(property)에 함수를 정의하여 저장 가능

<br>

### method

```
let obj = {
    name: 'jnh',
    age: 25,
    hello() {
        console.log("hello")
    }
}
```

- 객체에 저장된 값이 함수인 경우, 이를 메서드라고 부른다.

<br>

### method 변경

```
function hello() {
    console.log("hello")
}

function hi() {
    console.log("hi")
}

let obj = {
    name: "jnh",
    age: 25,
    func: hello
}

hello()
hi()
obj.func()

obj.func = hi //hello
obj.func() //hi
// 함수 주소값 변경 가능하다
console.log(hi === obj.func) // true
```

- 객체 내 초기 선언된 함수를 다른 함수로 변경

<br>

### this

```
let user = {name:"jnh"}
let admin = {name:"admin"}

function hello() {
    console.log("hello " + this.name)
}

user.func = hello
admin.func = hello

user.func()
admin.func()

// 또다른 호출 방법
user["func"]()
admin["func"]()
```

- 메서드의 객체 내부의 속성(property) 값을 접근할 수 있는 지시자
- `this로 접근하면 객체의 메모리가 가리키는 주소로 이동하기 때문에 같은 객체 내의 다른 요소로 접근 가능하다.`
- 객체별로 동일한 속성을 갖고 있지만 다른 데이터를 핸들링 할 수 있는 것이 oop의 장점

<br>
<br>

## 2. Number

- js에서 일반적인 숫자는 64비트 형식의 IEEE-754 표준 기반 형태로 저장되는 자료형
- 10진수 외에도 16진수, 2진수, 8진수의 다양한 진수 사용
- 16진수 표기 : 0xFF
- 8진수 표기 : 0o71
- 2진수 표기 : 0b1101
- 대표 상수 값 : `[MAX|MIN]_VALUE,` `[MAX|MIN]_SAFE_INTEGER`, `[POSITIVE|NEGATIVE]_INFINITY`, `NaN`
- 대표 메서드
- 문자열로 변환 : `Number.toString()`
- 특정 자리수까지 제한하여 숫자 표현 : `Number.toFixed()`, `Number.toPrecision()`
- 타입 확인 : `Number.isNaN()`, `Number.isFinite()`

<br>

### 지수 표기법

```
let billion1 = 1000000000;
let billion2 = 1e9;
let us = 1e-6;

console.log(billion1); //1000000000
console.log(billion2); //
console.log(us); //0.000001
```

- 아주 큰 숫자나 아주 작은 숫자를 표기하기 위해 지수 표기법(e)으로 0의 개수를 대체 표기 가능

<br>

### 진법 표기

```
console.log(0x0f) //15
console.log(0o17) //15
console.log(0b1111) //15
```

- 진법 표기를 지원하기 위해 0x(16진수), 0o(8진수), 0b(2진수) 로 N 진수 표기 가능

<br>

### Number 상수 값

```
console.log(Number.MAX_VALUE) //1.7976931348623157e+308
console.log(Number.MIN_VALUE) //5e-324

console.log(Number.MAX_SAFE_INTEGER) //9007199254740991
console.log(Number.MIN_SAFE_INTEGER) //-9007199254740991

console.log(Number.POSITIVE_INFINITY) //Infinity
console.log(Number.NEGATIVE_INFINITY) //-Infinity

console.log(Number.NaN) //NaN
```

- 지수로 표기되는 양수 최대/최소 값: `Number.MAX_VALUE`, `Number.MIN_VALUE`
- 안전하게 표기되는 최대(양수)/최소(음수) 값 : `Number.MAX_SAFE_INTEGER`, `Number.MIN_SAFE_INTEGER`
- 무한대 양수/음수 값: `Number.POSITIVE_INFINITY`, `Number.NEGATIVE_INFINITY`
- 부동 소수점 산술에서 정의되지 않거나 표현할 수 없는 값으로 해석될 수 있는 숫자 데이터 유형: `Number.NaN`

<br>

### 대표 메서드 - 형 변환

```
let us = 1e-6
console.log(us.toString()) //0.000001
console.log(typeof us.toString()) //string
console.log(typeof String(us)) //string
console.log(typeof (us + "")) //string
```

- Number to String: `Number.toString()`, `String(Number)`, `Number+""` 를 통해 변환 가능

<br>

### 대표 메서드 - 자리 수 표현

```
let num1 = 234.0;
let num2 = 123.456;

console.log(num1-num2) //110.544
console.log((num1 - num2).toFixed(3)) //110.544
console.log((num1 - num2).toPrecision(3)) //111 (정수가 이미 3자리 다 차지하여 소수점 미출력)
```

- 소수의 자리 수 길이를 제한 : `Number.toFixed(pos)`
- 정수와 소수의 자리 수를 합한 길이로 제한 : `Number.toPrecision(pos)`

<br>

### 대표 메서드 - Number 자료형 확인

```
console.log(Number.isNaN(0.123)) // false
console.log(!Number.isNaN(0.123 / "hello")) // false

// 유한수인지
console.log(Number.isFinite(123)) // true
console.log(Number.isFinite(Infinity)) // false
console.log(Number.isFinite("hello")) // false
```

- 부동 소수점 산술에서 정의되지 않거나 표현할 수 없는 값(NaN)인지 확인 :` Number.isNan()`
- 정상적인 유한수인지 확인 : `Number.isFinite()`

<br>

### 대표 메서드 - 정수와 실수 형 변환

```
console.log(Number.parseInt("123.123")) // 123
console.log(Number.parseInt("123문자")) // 123
console.log(parseInt("123문자")) // 123 사실상 같다!
console.log(Number.parseFloat("123.23em")) // 123.23
```

- 정수로 변환하는 방법(N 진수로 명시적 변환도 가능): `Number.parseInt()`
- 실수로 변환하는 방법 : `Number.parseFloat()`

<br>
<br>

## 3. String

- 텍스트 길이에 상관없이 문자열 형태로 저장되는 자료형
- js 에서는 글자 하나만 저장할 수 있는 char 자료형이 없다.
- js 에서 문자열은 페이지 인코딩 방식과 상관없이 항상 utf-16 형식을 따른다
- 문자열 길이 : `String.length`
- 문자열 접근 : `String.charAt(index)`, `String.charCodeAt(index)`
- 문자열 검색 : `String.indexOf()`, `String.lastIndexOf()`, `String.includes()`, `String.startsWith()`
- 문자열 변환 : `String.toUpperCase()`, `String.toLowerCase()`
- 문자열 치환 : `String.replace()`
- 문자열 추출 : `String.slice()`, `String.substring()`, `String.substr()`
- 문자열 분할 : `String.split()`

<br>

### 정의 방법

- "hello", 'hello', String()
- 문자열과 변수 혼합 표현 방법 : 역 따옴표

<br>

### 문자 표기

```
console.log("line\nfeed") //개행
console.log("line\rfeed") // 개행
console.log("line\\feed") // \ 출력
console.log("line\tfeed") // line	feed
console.log("line\u{1F60D}feed") // line😍feed
```

- Line feed(\n), Carriage return(\r), Backslash(\\), Tab(\t), Unicode(\u{})

<br>

### 문자열 길이

```
let str = `hello
world
!!!`

let str2 = "hello\nworld\t!!\\"

console.log(str.length) //15
console.log(str2.length) //15
// 개행도 포함된다
```

- `String.length`

<br>

### 문자 접근

```
let str = "hello world"

console.log(str.charAt(4)) //o
console.log(str.charCodeAt(4)) //111 해당 문자의 아스키코드 값
console.log(str[4]) //o
```

- 문자열 내 개별 문자 접근 방법 : `String.charAt(index)`, `String.charCodeAt(index)`, `String[index]`

<br>

### 문자열 검색

```
let str = "hello world!!"

console.log(str.indexOf("l")) //2
console.log(str.indexOf("l", 4)) //9 4번째 이후부터 찾아라
console.log(str.lastIndexOf("l")) //9 뒤에서부터 찾아줘

console.log(str.includes("hello")) //true
console.log(str.includes("Hello")) //false 대소문자 구분한다.
console.log(str.includes("ello")) //true

console.log(str.startsWith("ello")) //false
console.log(str.startsWith("ello", 1)) //true 첫번째부터 찾아라

console.log(str.endsWith("world")) //false
console.log(str.endsWith("d!!")) //true
```

- 문자열 검색(index) : `String.indexOf(substr, pos)`, `String.lastIndexOf(substr, pos)`
- pos : 어디서부터 찾을지
- 문자열 검색(bool) : `String.includes(substr, pos)`, `String.startsWith(substr, pos)`, `String.endsWith(substr, pos)`

<br>

### 문자열 대소문자 변환

```
let str = "helLo world!!"

console.log(str.toUpperCase()) //HELLO WORLD!!
console.log(str.toLowerCase()) //hello world!!
```

- 대소문자 변환 : `String.toUpperCase()`, `String.toLowerCase()`
  <br>
  <br>

## 4. 문자열 변환

### 문자열 치환

```
let text = "HelLo world!!!"
let changed_text = ""

changed_text = text.replace("world", "earth")
console.log(changed_text) //Hello earth!

changed_text = text.replace("!", "?")
console.log(changed_text) //Hello world?!! 하나만 변경

// 전부다 바꾸고 싶다면? > 정규식 활용
changed_text = text.replace(/!/g, "?")
console.log(changed_text) //HelLo world???

// 대소문자 구분없이 전부다 바꾸고싶다면?
changed_text = text.replace(/l/gi, "T")
console.log(changed_text) //HeTTo worTd!!!
```

- 처음 만나는 요소 문자열 치환(치환된 문자열 반환) : `String.replace(origin_str, change_str)`
- 정규 표현식 활용 문자열 치환 : 치환 문자열에 청규 표현식 기입 > /치환문자열/g(전체):i(대소문자 구분X)

<br>

### 문자열 추출

```
let text = "HelLo world!!!"

console.log(text.slice(0, 5)) //HelLo
console.log(text.slice(4)) //o world!!!
console.log(text.slice(-4)) //d!!!

console.log(text.(2, 6)) //lLo
console.log(text.slice(2, 6)) //lLo
// 둘이 결과 값 같다.

console.log(text.substring(6, 2)) //lLo 내부적으로 알아서 바껴서 돈다.
console.log(text.slice(6, 2)) //돌지 않는다.
// 보통은 알고리즘적으로 실행 못하게 하기 위해 slice 를 지향한다.

console.log(text.substr(2, 6)) //lLo wo
console.log(text.substr(-5, 3)) //ld!
```

- 위치 기반 문자열 추출 : `String.slice(start, end)`, `String.substring(start, end)`
- 길이 기반 문자열 추출 : `String.substr(start, length)`

<br>

### 문자열 분할

```
let fruits = "apple banana melon"

let result = fruits.split(" ")
console.log(result) //[ 'apple', 'banana', 'melon' ]

let fruits2 = "apple,banana,melon"
let result2 = fruits2.split(",")
console.log(result2) //[ 'apple', 'banana', 'melon' ]

// 문자 하나하나 끊으려면?
let text = "hello"
let result3 = text.split("")
console.log(result3) //[ 'h', 'e', 'l', 'l', 'o' ]

// 문자 하나하나 끊는데 3개만 필요해
let result4 = text.split("", 3)
console.log(result4) //[ 'h', 'e', 'l' ]
```

- 배열로 문자열 분할 : `String.split(Separator, limit)`

<br>
<br>

## 나의 회고 🤫

메서드와 함수를 단순히 비슷한 것이라고 여기고 있었는데 메서드는 객체 내에 정의된 함수에 한한다는 내용을 알게 되었다. <br>
또한 this가 해당 객체의 메모리 주소 값을 가르키면서 해당 객체의 다른 속성에 접근할 수 있다는 내부적인 사실도 알게 되었다. <br>
number와 string에 관련된 다양한 함수를 써보면서 다양한 내장함수들의 기능을 알아두면 좋을 것 같다!
