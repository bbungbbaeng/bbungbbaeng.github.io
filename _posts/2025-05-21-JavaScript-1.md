---
title: JavaScript 개념 정리
description: 함수, 객체, 클래스 등..
author: bbungbbaeng
date: 2025-05-21 12:22:00 +0900
categories: [Front-End, JavaScript]
tags: [JavaScript]
pin: false
math: true
mermaid: true
---

해당 포스트에서는 JavaScript의 개념 및 내용들을 정리해놓았다.  
보면서 복습하도록 하자.  

## **함수 표현식**
다른 언어들에서는 함수를 `특별한 동작을 하는 구조`로 취급하지만, 자바스크립트는 함수를 `특별한 종류의 값`으로 취급한다.  
  
먼저, 함수 선언을 통해 함수를 만들어보자.

```javascript
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

> **왜 함수 표현식의 끝에 세미콜론 `;`이 붙을까?**
> ```bash
> function gang() {
>     // ...
> }
> 
> let gang = function() {
>     // ...
> };
> ```
> - `if { ... }`, `for { ... }`, `function gang() { ... }`과 같이 중괄호로 만든 코드 블록 끝엔 `;`이 없어도 된다.
> - 함수 표현식은 `let gang = ...;`과 같은 구문 안에서 값의 역할을 한다. *(코드 블록이 아니고 값처럼 취급되어 변수에 할당)* 즉, 함수 표현식에 쓰인 세미콜론은 함수 표현식 때문에 붙여진 게 아니라, 구문의 끝이기 때문에 붙여진다.
{: .prompt-tip }

<br>

## **함수 표현식 vs 함수 선언문**

함수 표현식과 선언문의 차이에 대해 알아보자.

### **문법의 차이**

- 함수 선언문 : 주요 코드 흐름 중간에 독자적인 구문 형태로 존재
```bash
// 함수 선언문
function sum(a, b) {
    return a + b;
}
```

<br>

- 함수 표현식 : 표현식이나 구문 구성 내부에 생성
```bash
// 함수 표현식
let sum = function(a, b) {
    return a + b;
};
```

<br>

### **함수 생성의 차이**
- 함수 선언문은 함수 선언문이 정의되기 전에도 호출이 가능  

자바스크립트는 스크립트를 실행하기 전인 준비 단계에서 전역에 생성된 함수 선언문을 찾고, 해당 함수를 생성한다. 스크립트가 실행되기 전 "초기화 단계"에서 함수 선언 방식으로 정의한 함수가 생성되는 것이다.  
  
  

즉, 스크립트 어디서든 함수 선언문으로 선언한 함수에 접근할 수 있는 것이다.
```bash
gang(); // GANG

function gang() {
    alert("GANG");
}
```

<br>

- 함수 표현식은 실제 실행 흐름이 해당 함수에 도달했을 때 함수를 생성

```bash
gang(); // error!

let gang = function() {
    alert("GANG");
}
```

<br>

### **스코프 (Strict Mode의 경우)**

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

<br>

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

<br>

물음표 연산자 `?`를 사용하면 위의 함수 표현식 코드를 아래와 같이 단순화할 수 있다.

```bash
let price = prompt("국밥의 가격을 알려주세요: ");

let act = (price < 5000) ?
    function() {alert("혜자네요.");} :
    function() {alert("창렬이네요.");};

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

  

함수를 함수의 인자로 전달하고, 필요하다면 인수로 전달한 그 함수를 "나중에 호출(called back)"하는 것이 콜백 함수의 개념이다. 위의 예시에선 사용자가 "yes"라고 대답하면 `enableDarkMode`가 콜백이 되고, "no"라고 대답하면 `disableDarkMode`가 콜백이 된다.

  

아래와 같이 함수 표현식을 사용하면 코드 길이가 짧아진다.

```bash
function ask(question, yes, no) {
    if (confirm(question)) yes();
    else no();
}

ask(
    "다크 모드를 활성화하시겠습니까?",
    function() {alert("다크 모드가 적용되었습니다.");},
    function() {alert("라이트 모드로 전환되었습니다.");}
);
```

`ask(...)` 내부 함수들처럼, 이름 없이 선언한 함수는 <u>익명 함수(anonymous function)</u>라고 부른다. 익명 함수는 변수에 할당된 것이 아니기 때문에 `ask` 바깥에서는 접근할 수 없다. 

<br>

## **화살표 함수**

### **기본 문법**

함수 표현식보다 단순하고 간결한 방법으로 함수를 만들 수 있다. 바로 **화살표 함수(arrow function)**을 사용하는 것이다.  

```bash
let sum = (a, b) => a + b;

/*
위의 화살표 함수는 아래 함수의 축약 버전이다.

let sum = function(a, b) {
    return a + b;
};
*/

alert(sum(1, 2)); // 3
```

<br>

인수가 하나밖에 없다면, 인수를 감싸는 괄호를 생략할 수 있다.
```bash
let double = n => n * 2;

alert(double(3)); // 6
```

<br>

인수가 하나도 없을 땐, 괄호를 비워놓으면 된다. 다만, 이 때 괄호를 생략할 수는 없다.

```bash
let gang = () => alert("GANG");
```

<br>

화살표 함수는 함수 표현식과 같은 방법으로 사용할 수 있는데, 아래 예시와 같이 함수를 동적으로 만들 수 있다.

```bash
let price = prompt("국밥의 가격을 알려주세요: ");

let act = (price < 5000) ?
    () => {alert("혜자네요.");} :
    () => {alert("창렬이네요.");};

act();
```

<br>

### **본문이 여러 줄인 경우**

평가해야 할 표현식이나 구문이 여러 개인 경우 중괄호 안에 코드를 넣어주어야 한다. 그리고 `return` 지시자를 사용해 명시적으로 결과값을 반환해 주어야 한다.

```bash
let sum = (a, b) => {
    let result = a + b;
    return result;
};

alert(sum(1, 2)); // 3
```

<br>

## **객체**

자바스크립트에는 여덟 가지 자료형이 존재한다.  
- `Number`
- `BigInt`
- `String`
- `Null`
- `Undefined`
- `Boolean`
- `Symbol`
- `Object`  


이 중 일곱 개는 오직 하나의 데이터(문자열, 숫자 등)만 담을 수 있어 '원시형(primitive type)'이라 부른다.  

  

반면에 객체형은 키로 구분된 데이터 집합이나 복잡한 개체(entity)와 같은 다양한 데이터를 담을 수 있다.  
객체는 중괄호 `{...}`를 이용해 만들 수 있다. 중괄호 안에는 '`키(key) : 값(value)`' 쌍으로 구성된 속성을 여러 개 넣을 수 있는데, `키`에는 문자형, `값`에는 모든 자료형이 허용된다.

  

빈 객체를 만드는 방법은 두 가지가 있다.  

```bash
let user = new Object();    // '객체 생성자' 문법
let user = {};              // '객체 리터럴' 문법
```

<br>

### **리터럴과 속성**

중괄호 `{...}` 안에는 '`키(key) : 값(value)`' 쌍으로 구성된 속성이 들어간다.

```bash
let user = {        // 객체
    name: "Yumin",  // 키: "name", 값: "Yumin"
    age: 25         // 키: "age", 값: 25
};
```

`콜론(:)`을 기준으로 왼쪽에는 키가, 오른쪽에는 값이 위치한다. 속성 키는 속성 '이름' 혹은 '식별자'라고도 부른다.

<br>

점 표기법(dot notation)을 이용하면 속성 값을 읽을 수 있다.

```bash
// 속성 값 얻기
alert(user.name);   // Yumin
alert(user.age);    // 25
```

<br>

`delete` 연산자를 이용하면 속성을 삭제할 수 있다.

```bash
delete user.age;
```

<br>

여러 단어를 조합해 속성 이름을 만든 경우엔 속성 이름을 따옴표로 묶어줘야 한다.

```bash
let user = {
    name: "Yumin",
    age: 25,
    "so dumb": true
};
```

<br>

마지막 속성의 끝은 쉼표로 끝날 수 있다. 이런 쉼표를 'trailing(길게 늘어지는)' 또는 'hanging(매달리는)' 쉼표라고 부른다.

```bash
let user = {
    name: "Yumin",
    age: 25,
};
```

<br>

> **상수 객체는 수정될 수 있다.**  
> `const`로 선언된 객체는 수정될 수 있다.
> ```bash
> const user = {
>    name: "Yumin"
>};
>
>user.name = "bbungbbaeng"; // 가능
>
>alert(user.name); // bbungbbaeng
> ```
>
> - 5번째 줄에서 오류가 일어날 것처럼 보일 수 있지만, 그렇지 않다. `const`는 `user`의 값을 고정하지만, 그 내용은 고정하지 않는다.  
> - `const`는 `user = ...`를 전체적으로 설정하려고 할 때만 오류가 발생한다.
{: .prompt-tip }

<br>

### **대괄호 표기법**

여러 단어를 조합해 속성 키를 만든 경우, 점 표기법을 사용하여 속성 값을 읽을 수 없다.

```bash
// 문법 오류 발생
user.so dumb = true
```

점 표기법은 키가 '유효한 식별자'인 경우에만 사용할 수 있고, 유효한 식별자에는 공백이 없어야 한다. 또한, 숫자로 시작하지 않아야 하며 `$`와 `_`를 제외한 특수문자가 없어야 한다.

<br>

키가 유효한 변수 식별자가 아닌 경우에는 점 표기법 대신 '대괄호 표기법(square bracket notation)'을 사용할 수 있다. 대괄호 표기법은 키에 어떤 문자열이 있던지 상관없이 동작한다.

```bash
let user = {}

// set
user["so dumb"] = true;

// get
alert(user["so dumb"]); // true

// delete
delete user["so dumb"];
```

대괄호 표기법 안에서 문자열을 사용할 때는 문자열을 따옴표로 묶어줘야 한다는 점을 주의해야 한다.

<br>

대괄호 표기법을 사용하면 문자열 뿐만 아니라 모든 표현식의 평가 결과를 속성 키로 사용할 수 있다.

```bash
let key = "so dumb";

user[key] = true;
```

<br>

이를 응용하면 코드를 유연하게 작성할 수 있다.

```bash
let user = {
    name: "Yumin",
    age: 25
};

let key = prompt("뭐가 궁금해", "name / age");

// 변수로 접근
alert(user[key]);
```

<br>

### **계산된 속성명**

객체를 만들 때, 객체 리터럴 안에 속성 키가 대괄호로 둘러싸여 있는 경우 이를 **계산된 속성명(computed property name)**라고 부른다.

```bash
let drink = prompt("어떤 음료를 구매하시겠습니까?");

let bag = {
    [drink]: 3,
};

alert(bag.soju); // drink에 "soju"가 할당되었다면 3이 출력된다.
```

위의 예시에서 `[soju]`는 속성 이름을 변수 `drink`에서 가져오겠다는 것을 의미한다. 사용자가 프롬프트 대화창에 `soju`를 입력했다면, `bag`에는 `{soju: 3}`이 할당되었을 것이다.

<br>

대괄호 표기법을 사용한다면 아래와 같은 표현식도 가능하다.

```bash
let drink = 'soju';
let bag = {
    [soju + "beer"]: 5      // bag.sojubeer = 5
};
```

<br>

### **단축 속성명**

```bash
function makeUser(name, age) {
    return {
        name: name,
        age: age,
    };
}

let user = makeUser("Yumin", 25);
alert(user.name); // Yumin
```

위 예시의 속성들은 이름과 값이 변수의 이름과 동일하다. 이렇게 변수를 사용해 속성을 만드는 경우, **단축 속성명(property shorthand)**을 사용하면 코드를 짧게 줄일 수 있다.

<br>

`name: name` 대신 `name`만 적어주어도 속성을 설정할 수 있다.

```bash
function makeUser(name, age) {
    return {
        name,   // name: name과 동일
        age,    // age: age와 동일
    };
}
```

<br>

한 객체에서 일반 속성과 단축 속성을 함께 사용하는 것도 가능하다.

```bash
let user = {
    name,
    age: 30,
};
```

<br>

### **속성 이름의 제약사항**

변수 이름(키)에는 `for`, `let`, `return`과 같은 예약어를 사용하면 안되지만, 객체 속성에는 이런 제약이 없다.

```bash
let obj = {
    for: 1,
    let: 2,
    return: 3,
};

alert(obj.for + obj.let + obj.return); // 6
```

<br>

이와 같이 속성 이름에는 특별한 제약이 없다. 어떤 문자형, 심볼형 값도 속성 키가 될 수 있고, 문자형이나 심볼형에 속하지 않는 값들은 문자열로 자동 형 변환된다.

```bash
let obj = {
    0: "test"
};

// 숫자 0은 문자열 "0"으로 변환되기 때문에 두 alert는 같은 속성에 접근한다.
alert(obj["0"]); // test
alert(obj[0]); // test
```

<br>

이와 같이, 객체 속성 키에 쓸 수 있는 문자열에는 제약이 없지만, `__proto__`라는 예외가 존재한다.

```bash
let obj = {};
obj.__proto__ = 5;
alert(obj.__proto__); // [object Object]
```

원시값 `5`를 할당했는데도 무시된 것을 확인할 수 있다.

<br>

### **in 연산자로 속성 존재 여부 확인하기**

자바스크립트 객체의 중요한 특징 중 하나는, 다른 언어와 달리 존재하지 않는 속성에 접근하려 해도 에러가 발생하지 않고 `undefined`를 반환한다는 것이다.  

  

이런 특징을 응용하면 속성 존재 여부를 쉽게 확인할 수 있다.

```bash
let user = {};

alert(user.noSuchProperty === undefined); // true
```

<br>

이렇게 `undefined`와 비교하는 것 이외에도 연산자 `in`을 사용하면 속성 존재 여부를 확인할 수 있다.

```bash
let user = {name: "Yumin", age: 25}

alert("age" in user);   // true
alert("gang" in user);  // false
```

`in` 왼쪽에는 반드시 속성 이름이 와야 한다. 

<br>

대부분의 경우, 일치 연산자를 사용해서 속성 존재 여부를 알아내는 방법(`=== undefined`)은 잘 동작하지만, 가끔은 이 방법이 실패할 때가 존재한다. 이럴 때 `in`을 사용하면 속성 존재 여부를 판별할 수 있다.

```bash
let obj = {
    test: undefined
};

alert(obj.test); // 값이 'undefined'이므로, alert 창에는 undefined가 출력된다. 그러나 속성 test는 존재한다.
alert("test" in obj); // true
```

`obj.test`는 실제 존재하는 속성이다. 따라서 `in` 연산자는 정상적으로 `true`를 반환한다.

<br>

### **for.. in 반복문**

`for.. in` 반복문을 사용하면 객체의 모든 키를 순회할 수 있다.

```bash
let user = {
    name: "Yumin",
    age: 25,
    isAdmin: true
};

for (let key in user) {
    alert(key);         // name, age, isAdmin
    alert(user[key]);   // Yumin, 25, true
}
```

위의 코드를 실행하면 객체 `user`의 모든 속성이 출력된다. 여기서, 반복 변수(looping variable)을 선언(`let key`)했다는 점에 주의해야 한다.

<br>

### **객체 정렬 방식**

자바스크립트에서 객체는 '특별한 방식'으로 정렬된다. 정수 속성은 자동으로 정렬되고, 그 외의 속성은 객체에 추가한 순서 그대로 정렬된다.

```bash
let codes = {
    "82": "한국",
    "1": "미국",
    "81": "일본",
    "86": "중국" 
};

for (let code in codes) {
    alert(code); // 1, 81, 82, 86
}
```

<br>

반면, 키가 정수가 아닌 경우에는 작성된 순서대로 속성이 나열된다.

```bash
let user = {
    name: "Yumin",
    surname: "bbungbbaeng",
    age: 25
};

for (let prop in user) {
    alert(prop); // name, surname, age
}
```

<br>

## **메서드와 this**

객체는 사용자(user), 주문(order)와 같이 실제 존재하는 개체(entity)를 표현하고자 할 때 생성된다. 사용자는 현실에서도 장바구니에 담기, 물건 선택하기, 로그인하기 등의 행동을 한다. 이와 마찬가지로 사용자를 나타내는 객체도 특정한 행동을 할 수 있다. 이를 위해, 자바스크립트에서는 객체의 속성에 함수를 할당해 객체에게 행동할 수 있는 능력을 부여한다.

  

### **메서드 생성**

```bash
let user = {
    name: "Yumin",
    age: 25
};

user.sayGang = function() {
    alert("GANG");
};

user.sayGang(); // GANG
```

함수 표현식으로 함수를 만들고, 객체 속성 `user.sayGang`에 함수를 할당해주었다. 이렇게 객체 속성에 할당된 함수를 **메서드(method)**라고 부른다. 

<br>

메서드는 아래와 같이 이미 정의된 함수를 이용해서 만들 수도 있다.

```bash
let user = {
    name: "Yumin",
    age: 25
};

function sayGang() {
    alert("GANG");
}

// 선언된 함수를 메서드로 등록
user.sayGang = sayGang;

user.sayGang(); // GANG
```

<br>

### **메서드 단축 구문**

```bash
// 아래 두 객체는 동일하게 작동한다.
user = {
    sayGang: function() {
        alert("GANG");
    }
};

user = {
    sayGang() {     // "sayGang: function()"과 동일
        alert("GANG");
    }
};
```

위처럼 `function`을 생략해도 메서드를 정의할 수 있다.

<br>

### **this**

메서드 내부에서 `this` 키워드를 사용하면 객체에 접근할 수 있다. 이 때, '점(.) 앞'의 `this`는 메서드를 호출할 때 사용된 객체를 나타낸다.

```bash
let user = {
    name: "Yumin",
    age: 25

    sayName() {
        // 'this'는 '현재 객체'를 나타낸다.
        alert(this.name);
    }
};

user.sayName(); // Yumin
```

<br>

`this`를 사용하지 않고 외부 변수를 참조해 객체에 접근하는 것도 가능하다.

```bash
let user = {
    name: "Yumin",
    age: 25

    sayName() {
        // 'this' 대신 'user'를 이용했다.
        alert(user.name);
    }
};
```

<br>

그러나, 외부 변수를 사용해 객체를 참조하면 예상치 못한 에러가 발생할 수 있다. `user`를 복사해 다른 변수에 할당(`admin = user`)하고, `user`는 전혀 다른 값으로 덮어썼다고 가정해보자.

```bash
let user = {
    name: "Yumin",
    age: 25

    sayName() {
        alert(user.name);   // Error: Cannot read property 'name' of null
    }
};

let admin = user;
user = null;    // user를 null로 덮어쓴다.

admin.sayName();    // 에러 발생
```

`sayGang()`은 원치 않는 값(`null`)을 참조하게 된다.

<br>

### **자유로운 this**

자바스크립트에서는 다른 프로그래밍 언어와 달리 모든 함수에 `this`를 사용할 수 있다. 그렇기 때문에 아래와 같이 코드를 작성해도 문법 에러가 발생하지 않는다.

```bash
function sayName() {
    alert(this.name);
}
```

<br>

동일한 함수라도 다른 객체에서 호출했다면 `this`가 참조하는 값이 달라진다.

```bash
let user = {name: "Yumin"};
let admin = {name: "bbungbbaeng"};

function sayName() {
    alert(this.name);
}

// 별개의 객체에서 동일한 함수를 사용한다.
user.f = sayGang;
admin.f = sayGang;

// 'this'는 '점(.) 앞'의 객체를 참조하기 때문에 각각의 'this' 값이 달라진다.
user.f();   // Yumin (this == user)
admin.f();  // bbungbbaeng (this == admin)
```

<br>

### **this가 없는 화살표 함수**

화살표 함수는 일반 함수와 달리 '고유한' `this`를 가지지 않는다. 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 '평범한' 외부 함수에서 `this` 값을 가져온다. 아래 예시에서 함수 `arrow()`의 `this`는 외부 함수 `user.sayHi()`의 `this`가 된다.

```bash
let user = {
    firstName = "Yumin",

    sayName() {
        let arrow = () => alert(this.firstName);
        arrow();
    }
};

user.sayName(); // Yumin
```

별개의 `this`가 만들어지는 건 원하지 않고, 외부에 있는 `this`를 이용하고 싶은 경우 화살표 함수가 유용할 수 있다.

<br>

## **참조에 의한 객체 복사**