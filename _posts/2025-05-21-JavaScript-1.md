---
title: JavaScript 상식 정리
description: 함수 표현식, 화살표 함수, 객체 등..
author: bbungbbaeng
date: 2025-05-21 12:22:00 +0900
categories: [Front-End, JavaScript]
tags: [JavaScript]
pin: false
math: true
mermaid: true
---

해당 포스트에서는 JavaScript의 다양한 내용들을 정리해놓았다.  
보면서 복습하도록 하자.  

## **함수 표현식**
다른 언어들에서는 함수를 `특별한 동작을 하는 구조`로 취급하지만, 자바스크립트는 함수를 `특별한 종류의 값`으로 취급한다.  
<br>
먼저, 함수 선언을 통해 함수를 만들어보자.  

```bash
function gang() {
    alert("GANG");
}
``` 

<br>
함수 선언 방식 이외에, *함수 표현식(Function Expression)*을 사용해서 함수를 만들 수 있다.
함수 표현식을 통해 함수를 만들어보자.  

```bash
let gang = function() {
    alert("GANG");
}
```  

위 코드 블록에서 보다시피, 변수에 값을 할당하는 것처럼 함수가 변수에 할당되었다.  
<br>
자바스크립트에서 함수는 값이기 때문에 `alert`를 통해 함수 코드를 출력할 수도 있고, 변수를 복사해 다른 변수에 할당하는 것처럼 함수를 복사해 다른 변수에 할당할 수도 있다.  

> 함수 표현식의 끝에 왜 세미콜론 `;`이 붙을까?
> ```bash
> function gang() {
>     // ...
> }
> 
> let gang() = function() {
>     // ...
> };
> ```
> - `if { ... }`, `for { ... }`, `function gang() { ... }`과 같이 중괄호로 만든 코드 블록 끝엔 `;`이 없어도 된다.
> - 함수 표현식은 `let gang = ...;`과 같은 구문 안에서 값의 역할을 한다. *(코드 블록이 아니고 값처럼 취급되어 변수에 할당)* 즉, 함수 표현식에 쓰인 세미콜론은 함수 표현식 때문에 붙여진 게 아니라, 구문의 끝이기 때문에 붙여진다.
{: .prompt-tip }

<br>

## **함수 표현식 vs 함수 선언문**

함수 표현식과 선언문의 차이에 대해 알아보자.

### 1. 문법의 차이

- 함수 선언문 : 주요 코드 흐름 중간에 독자적인 구문 형태로 존재
```bash
// 함수 선언문
function sum(a, b) {
    return a + b;
}
```

- 함수 표현식 : 표현식이나 구문 구성 내부에 생성
```bash
// 함수 표현식
let sum = function(a, b) {
    return a + b;
};
```

<br>

### 2. 함수 생성의 차이
- 함수 선언문은 함수 선언문이 정의되기 전에도 호출이 가능  

자바스크립트는 스크립트를 실행하기 전인 준비 단계에서 전역에 생성된 함수 선언문을 찾고, 해당 함수를 생성한다. 스크립트가 실행되기 전 "초기화 단계"에서 함수 선언 방식으로 정의한 함수가 생성되는 것이다.  
  
즉, 스크립트 어디서든 함수 선언문으로 선언한 함수에 접근할 수 있는 것이다.
```bash
gang(); // GANG

function gang() {
    alert("GANG");
}
```

- 함수 표현식은 실제 실행 흐름이 해당 함수에 도달했을 때 함수를 생성

```bash
gang(); // error!

let gang = function() {
    alert("GANG");
}
```

<br>

### 3. 스코프 (Strict Mode의 경우)

- 코드 블록 내 위치한 함수 선언문은 블록 내 어디서든 접근할 수 있으나, 블록 밖에서는 함수에 접근 불가

```bash
let price = prompt("국밥의 가격을 알려주세요: ");

if (price < 5000) {
    function act1() {
        alert("혜자네요.");
    }
} else {
    function act2() {
        alert("창렬이네요.");
    }
}

act1(); // Error: act1 is not defined
act2(); // Error: act2 is not defined
```

- 함수 표현식은 블록 밖에서도 접근 가능

```bash
let price = prompt("국밥의 가격을 알려주세요: ");

if (price < 5000) {
    let act1 = function() {
        alert("혜자네요.");
    }
} else {
    let act2 = function() {
        alert("창렬이네요.");
    }
}

act1(); // 혜자네요.
act2(); // 창렬이네요.
```

물음표 연산자 `?`를 사용하면 위의 함수 표현식 코드를 아래와 같이 단순화할 수 있다.

```bash
let price = prompt("국밥의 가격을 알려주세요: ");

let act = (price < 5000) ?
    function() { alert("혜자네요."); } :
    function() { alert("창렬이네요."); };

act(); // 제대로 동작한다.
```
<br>

## **콜백 함수**

매개변수 3개가 있는 함수 `ask(question, yes, no)`를 작성해보자.

```bash
function ask(question, yes, no) {
    if (confirm(question)) yes();
    else no(); 
}

function enableDarkMode() {
    alert("다크 모드가 적용되었습니다.");
}

function disableDarkMode() {
    alert("라이트 모드로 전환되었습니다.");
}

// 함수 enableDarkMode와 disableDarkMode가 ask 함수의 인수로 전달됨.
ask("다크 모드를 활성화하시겠습니까?", enableDarkMode, disableDarkMode);
```

함수는 반드시 `question`을 해야 하고, 사용자의 질문에 따라 `yes()` 또는 `no()`를 호출한다.  
이 때, 함수 `ask`의 인수, `enableDarkMode`와 `disableDarkMode`은 **콜백 함수** 또는 **콜백**이라고 불린다.

<br>

함수를 함수의 인자로 전달하고, 필요하다면 인수로 전달한 그 함수를 "나중에 호출(called back)"하는 것이 콜백 함수의 개념이다. 위의 예시에선 사용자가 "yes"라고 대답하면 `enableDarkMode`가 콜백이 되고, "no"라고 대답하면 `disableDarkMode`가 콜백이 된다.

<br>

아래와 같이 함수 표현식을 사용하면 코드 길이가 짧아진다.

```bash
function ask(question, yes, no) {
    if (confirm(question)) yes();
    else no();
}

ask(
    "다크 모드를 활성화하시겠습니까?",
    function() { alert("다크 모드가 적용되었습니다."); },
    function() { alert("라이트 모드로 전환되었습니다."); }
);
```

`ask(...)` 내부 함수들처럼, 이름 없이 선언한 함수는 <u>익명 함수(anonymous function)</u>라고 부른다. 익명 함수는 변수에 할당된 것이 아니기 때문에 `ask` 바깥에서는 접근할 수 없다. 

<br>

## **화살표 함수**

### 1. 기본 문법


### 2. 본문이 여러 줄인 경우