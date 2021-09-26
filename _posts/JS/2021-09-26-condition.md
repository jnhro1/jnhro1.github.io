---
layout: post
title:  "[JS] 3. Condition"
subtitle:   "조건문"
categories: JS
comments: true
---

- 목차
  - [1. 조건문 if-else](#.조건문if-else)
  - [2. 조건문 switch](#.조건문switch)
  - [3. 연습문제](#.연습문제)


<br>

## 1. 조건문 if-else

````
let apple = 20;

if (apple >= 20) {
  console.log("very expensive");
} else if (apple < 5) {
  console.log("very cheap");
} else {
  console.log("nice");
}

console.log("done");
````

- 알고리즘에서 논리적 비교를 할 때 사용되는 조건식
- if, else if, else 키워드를 통해 구성되며, 조건식에 맞을 경우 중괄호 {} 내에 있는 명령문을 수행
- 단, 실행 문장이 단일 문장일 경우에는 {} 생략 가능


<br>

### 삼항 연산자

````
let apple = 10;

msg = apple > 20 ? "very expensive" : "nice";

console.log(msg);
````

- if-else를 대체하여 사용 가능
- 변수 = (조건식) ? 참일 때 값 : 거짓일 때 값

<br>
<br>

## 2. 조건문 switch

```
let day_number = 1;
let day = "";

switch (day_number) {
  case 0:
    day = "Sunday";
    break;
  case 1:
    day = "Monday";
    break;
  case 2:
    day = "TuesDay";
    break;
  case 3:
    day = "Wednesday";
    break;
  case 4:
    day = "Thursday";
    break;
  case 5:
    day = "Friday";
    break;
  case 6:
    day = "Saturday";
    break;
  default:
    day = "error";
    break;
}

console.log(day)
```


````
let browser = "Chrome";
let msg = "";

switch (browser) {
  case "Explorer":
    msg = "ActiveX installation required";
    break;
  //  braek 안걸리면 쭉 내려간다.
  // 변경, 라인에 대한 최소화 가능
  case "Chrome":
  case "Firefox":
  case "Safari":
  case "Opera":
    msg = "Supported browsers";
    break;
  default:
    msg = "Unsupported browsers";
    break;
}

console.log(msg);
````


- 표현식을 평가하여 그 값이 일치하는 case 문을 실행하는 조건문
- switch, case, break, default 키워드를 통해 구성되며, switch의 조건에 맞는 case 구문을 수행
- 일반적으로 하나의 case만 수행되도록 case 끝을 break로 끝맺음

<br>
<br>

## 3. 연습문제
- 조건문 switch를 이용하여 1~7 사이의 숫자를 입력 받으면 요일을 출력해주는 코드를 작성하시오
- 1(월요일) ~ 7(일요일)로 맵핑된다

```
const day = 3;
let weekend = "";

switch (day) {
  // 구현하기
  case 1:
    weekend = "월요일";
    break;
  case 2:
    weekend = "화요일";
    break;
  case 3:
    weekend = "수요일";
    break;
  case 4:
    weekend = "목요일";
    break;
  case 5:
    weekend = "금요일";
    break;
  case 6:
    weekend = "토요일";
    break;
  case 7:
    weekend = "일요일";
    break;
  default:
    weekend = "잘못 입력하셨습니다.";
    break;
}

console.log(weekend); // output : 수요일
```


<br>
<br>

## 나의 회고 🤫
if-elseif 구문과 switch-case 구문의 성능 차이가 궁금해져서 알아보았다. <br>
정답은 switch-case 문에서도 해당 경우일 때 바로 return 만 시켜준다면 실행 속도는 거의 비슷하다는 것! <br>
하지만 나의 예제처럼 변수에 값을 할당하고 마지막에 출력해주는 형태라면 if-else 구문의 실행 속도가 아주 미세하게 더 빨랐다.<br>
따라서 switch-case 문의 실행속도를 높이기 위해선 바로 리턴하도록 하는 것이 좋을 것 같다.<br>
근데 그러면 `return 변수;` 문장의 중복이 발생할텐데 클린코드 관점에서는 별로일 것 같고,,, 조율해봐야 하는 문제일 것 같다.<br>
(나라면 실행속도를 조금 뒤로 미루고 깔끔한 코드를 선택할 것 같다... 차이가 아주 미세하니까!)
- switch-case가 점프 테이블로 구성되어서 더 최적화가 좋다느니, 변수 3개까지는 if 가 더 좋다느니 아직 말이 많은 부분인 것 같긴 하다
- 어쨋든 전체 프로그램 성능에 엄청 큰 영향을 끼치진 않으니 의미적으로 더 좋은 조건문을 선택할 것!