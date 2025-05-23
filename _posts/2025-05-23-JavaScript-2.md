---
title: JavaScript 브라우저 정리
description: DOM, 이벤트, 리소스 로딩 등..
author: bbungbbaeng
date: 2025-05-22 20:43:00 +0900
categories: [Front-End, JavaScript]
tags: [JavaScript]
pin: false
math: true
mermaid: true
---

해당 포스트에서는 브라우저 내 페이지에 대한 개념 및 내용들을 정리해놓았다.  
보면서 복습하도록 하자.

## **브라우저 환경**

자바스크립트가 돌아가는 플랫폼은 **호스트(host)**라고 불린다. 각 플랫폼은 해당 플랫폼에 특정되는 기능을 제공하는데, 자바스크립트에서는 이를 호스트 환경(host environment)이라고 부른다.  

아래 그림은 호스트 환경이 웹 브라우저일 때 사용할 수 있는 기능을 개괄적으로 보여준다.

![호스트 환경]({{ site.google_drive }}1WFnYNn0Q9abyYDYadPOIV2g_MMwm01JB&sz=w1000){: .w-50 .normal} 

최상단에는 `window`라 불리는 '루트' 객체가 있는데, `window` 객체는 2가지 역할을 한다.  

1. 자바스크립트 코드의 전역 객체
2. '브라우저 창(browser window)'을 대변하고, 이를 제어하는 메서드를 제공  

아래 예시에선 `window` 객체를 전역 객체로 사용하고 있다.

```bash
function sayGang() {
    alert("GANG");
}

// 전역 함수는 전역 객체(window)의 메서드이다.
window.sayGang();
```

<br>

아래 예시에서는 `window` 객체가 브라우저 창을 대변하고 있으며, 이를 이용해 창의 높이를 출력한다.

```bash
alert(window.innerHeight);  // 창 내부(inner window) 높이
```

<br>

### **문서 객체 모델(DOM)**

**문서 객체 모델(Document Object Model, DOM)**은 웹 페이지 내의 모든 콘텐츠를 객체로 나타내준다.  

`document` 객체는 페이지의 '기본 진입점' 역할을 한다. `document` 객체를 이용해 페이지 내 그 무엇이든 변경할 수 있고, 원하는 것을 만들 수도 있다.

```bash
document.body.style.background = "red";

setTimeOut(() => document.body.style.background = "", 1000);
```

<br>

> **스타일링을 위한 CSSOM**  
> 
> CSS 규칙과 스타일시트(stylesheet)는 HTML과 다른 구조를 가진다. 따라서 CSS 규칙과 스타일시트를 객체로 나타내고 이 객체를 어떻게 읽고 쓸 수 있을지에 대한 CSS 객체 모델(CSS Object Model, CSSOM)이 존재한다.
{: .prompt-tip }

<br>

### **브라우저 객체 모델(BOM)**

**브라우저 객체 모델(Browser Object Model, BOM)**은 문서 이외의 모든 것을 제어하기 위해 브라우저(호스트 환경)가 제공하는 추가 객체를 나타낸다.

```bash
alert(location.href);   // 현재 URL을 보여준다.

if (confirm("위키피디아 페이지로 가시겠습니까?")) {
    location.href = "https://wikipedia.org";
    // 새로운 페이지로 넘어간다.
}
```

<br>

## **DOM 트리**

HTML을 지탱하는 것은 태그(tag)이다.  

문서 객체 모델(DOM)에 따르면, 모든 HTML 태그는 객체이다. 태그 하나가 감싸고 있는 '자식' 태그는 중첩 태그(nested tag)라고 부른다. 태그 내의 문자(text) 역시 객체이다.  

이런 모든 객체는 자바스크립트를 통해 접근할 수 있고, 페이지를 조작할 때 이 객체를 사용한다.  

아래 예시를 실행하면 `<body>`가 3초간 붉은색으로 변경된다.  

```bash
document.body.style.background = 'red';
setTimeOut(() => document.body.style.background = '', 3000);
```

`document.body`는 `<body>` 태그를 객체로 나타낸 것이다.  

<br>

### **DOM 예제**

간단한 예제를 통해 DOM 구조에 대해 알아보자.

```bash
<!DOCTYPE HTML>

<html>
    <head>
        <title>사슴에 관하여</title>
    </head>
    <body>
        사슴에 관한 진실.
    </body>
</html>
```

![DOM 트리 예제]({{ site.google_drive }}1Vx853sXPRJupAxSWuPXaI-2FwnFUULg5&sz=w1000){: .w-50 .normal}  

트리에 있는 노드들은 모두 객체이다.  

태그는 요소 노드(element node)이고, 트리 구조를 구성한다. `<html>`은 루트 노드가 되고, `<head>`와 `<body>`는 루트 노트의 자식이 된다.  

요소 내의 문자는 텍스트(text) 노드가 된다. 텍스트 노드는 문자열만 담으며, 자식 노드를 가질 수 없고, 트리의 끝에 위치한 잎 노드(leaf node)가 된다.  

위 그림에서 `<title>` 태그는 `"사슴에 관하여"`라는 텍스트 노드를 자식으로 갖는다.  

텍스트 노드에 있는 특수문자를 주목해보자.  

- 새 줄(new line): `↵` (자바스크립트에서는 `\n`으로 표시)
- 공백(space): `␣`  

새 줄과 공백은 글자나 숫자처럼 항상 유효한 문자로 취급된다. 따라서 이 두 특수문자는 텍스트 노드가 되고, DOM의 일부가 된다.  

텍스트 노드 생성에는 두 가지 예외가 존재한다.

1. `<head>` 이전의 공백과 새 줄은 무시된다.
2. 모든 콘텐츠는 `<body>` 안쪽에 있어야 하므로, `</body>` 뒤에 무언가를 넣더라도 그 콘텐츠는 자동으로 `<body>` 안쪽으로 옮겨진다. 따라서, `</body>` 뒤엔 공백이 있을 수 없다.  

<br>

> **문자열 양 끝 공백과, 공백만 있는 텍스트 노드는 개발자 도구에서 보이지 않는다.**
>
> DOM을 다룰 때 키게 되는 브라우저 개발자 도구에서는 문자 맨 앞이나 끝쪽의 공백과 태그 사이의 새 줄 때문에 만들어지는 비어있는 텍스트 노드가 나타나지 않는다.  
> 
> 공백이나 새 줄이 만들어내는 공간은 HTML 문서가 브라우저 상에 어떻게 표현되는지에 대해서 대개는 영향을 끼치지 않기에, 개발자 도구는 이러한 방식으로 화면을 덜 차지하게 만들어졌다.
{: .prompt-tip }

<br>

### **자동 교정**

기형적인 HTML을 만나면 브라우저는 DOM 생성 과정에서 HTML을 자동으로 교정한다.  

예를 들어, 가장 최상위 태그는 항상 `<html>`이어야 하는데, 문서에 `<html>` 태그가 없는 경우, 문서 최상위에 이를 자동으로 넣어준다. 따라서 DOM에는 `<html>`에 대응하는 노드가 항상 있다. 이는 `<body>`에도 같은 방식으로 적용된다.  

DOM 생성 과정에서 브라우저는 문서에 있는 에러, 닫는 태그가 없는 에러 등을 자동으로 처리한다. 

```bash
// 닫는 태그가 없는 경우

<p>안녕하세요
<li>엄마
<li>그리고
<li>아빠
```

이렇게 태그의 짝이 안 맞아도 브라우저는 태그를 읽고 자동으로 빠진 부분을 채워넣는다. 따라서 최종 결과물은 정상적인 DOM이 된다.

![DOM 트리 예제 2]({{ site.google_drive }}1d8F93IN3U4MYnTFlLhz8Ybsr3Xrf8sWR&sz=w1000){: .w-50 .normal}  

<br>

### **기타 노드 타입**

요소와 텍스트 노드 외에도 다양한 노드 타입들이 존재한다.  

예를 들어, 주석도 노드가 된다.

```bash
<!DOCTYPE HTML>
<html>
    <body>
        사슴에 관한 진실.
        <ol>
            <li>사슴은 똑똑합니다.</li>
            <!-- comment -->
            <li>그리고 잔꾀를 잘 부리죠!</li>
        </ol>
    </body>
</html>
```

![DOM 트리 예제 2]({{ site.google_drive }}12Ey5Nn_SUXBeFi0yQzFiqpLWYvlSZRe1&sz=w1000){: .w-50 .normal}  

주석은 화면 출력물에 영향을 주지 않지만, HTML에 무언가 있다면 반드시 DOM 트리에 추가되어야 한다는 규칙 때문에 DOM에 추가된 것이다.  

<u>HTML 안의 모든 것은 (심지어 그것이 주석이더라도) DOM을 구성한다.</u>   

HTML 문서 최상단에 위치하는 `<!DOCTYPE ...>` 지시자 또한 DOM 노드가 된다. 이 노드는 DOM 트리의 `<html>` 바로 위에 위치한다.  

문서 전체를 나타내는 `<document>` 객체 또한 DOM 노드이다.

<br>

## **브라우저 이벤트 소개**

이벤트(event)는 무언가 일어났다는 신호이다. 모든 DOM 노드는 이런 신호를 만들어낸다. (이벤트는 DOM에만 한정되지는 않는다)  

자주 사용되는 유용한 DOM 이벤트를 살펴보자.  

**마우스 이벤트**  
- `click` : 요소 위에서 마우스 왼쪽 버튼을 눌렀을 때(터치스크린의 경우 탭) 발생한다.
- `contextmenu` : 요소 위에서 마우스 오른쪽 버튼을 눌렀을 때 발생한다.
- `mouseover`와 `mouseout` : 마우스 커서를 요소 위로 움직였을 때, 커서가 요소 밖으로 움직였을 때 발생한다.
- `mousedown`과 `mouseup` : 요소 위에서 마우스 왼쪽 버튼을 누르고 있을 때, 마우스 버튼을 뗄 때 발생한다.
- `mousemove` : 마우스를 움직일 때 발생한다.  

**폼 요소 이벤트**
- `submit` : 사용자가 `<form>`을 제출할 때 발생한다.
- `focus` : 사용자가 `<input>`과 같은 요소에 포커스할 때 발생한다.  

**키보드 이벤트**  
- `keydown`과 `keyup` : 사용자가 키보드 버튼을 누르거나 뗄 때 발생한다.

**문서 이벤트**
- `DOMContentLoaded` : HTML이 전부 로드 및 처리되어 DOM 생성이 완료되었을 때 발생한다.

**CSS 이벤트**
- `transitioned` : CSS 애니메이션이 종료되었을 때 발생한다.

<br>

### **이벤트 핸들러**

이벤트에 반응하려면 이벤트가 발생했을 때 실행되는 함수인 핸들러(handler)를 할당해야 한다.  

핸들러는 사용자의 행동에 어떻게 반응할지를 자바스크립트 코드로 표현한 것이다. 

#### **HTML 속성**

HTMl 안의 `on<event>` 속성에 핸들러를 할당할 수 있다.

아래의 예시에선 `input` 태그의 `onclick` 속성에 `click` 핸들러를 할당하는 것을 볼 수 있다.

```bash
<input value="클릭해주세요." onclick "alert('클릭!')" type="button">
```

버튼을 클릭하면 `onclick` 안의 코드가 활성화 된다.  

여기서 주의해야 할 것은 속성값 내에서 사용된 따옴표이다. 속성값 전체가 큰따옴표로 둘러싸여 있기 때문에 작은 따옴표로 둘러쌓인 모습을 볼 수 있다. `onclick="alert("클릭!")"`과 같이 속성값 내부에 또 큰따옴표를 쓰면 코드가 작동하지 않는다.  

긴 코드를 HTML 속성값으로 사용하는 것은 추천되지 않는다. 만약 코드가 길다면, 함수를 만들어서 이를 호출하는 방법이 추천된다.  

<br>

아래의 코드에선, 버튼을 클릭하면 함수 `countRabbits()`이 호출된다.  

```bash
<script>
    function countRabbits() {
        for (let i = 1; i <= 3, i++) {
            alert('토끼 ${i}마리');
        }
    }
</script>

<input value="토끼를 세봅시다!" onclick="countRabbits()" type="button">
```

HTML 속성은 대·소문자를 구분하지 않기 때문에, `ONCLICK`은 `onClick`이나 `onCLICK`과 동일하게 작동한다. 하지만 속성값은 대개 `onclick` 같이 소문자로 작성한다.

<br>

#### **DOM 속성**

DOM 속성 `on<event>`를 사용해도 핸들러를 할당할 수 있다.  

아래의 코드는 `elem.onclick`을 사용한 예시이다.

```bash
<input id="elem" type="button" value="클릭해주세요.">
<script>
    elem.onclick = function() {
        alert('감사합니다.');
    };
</script>
```

핸들러를 HTML 속성을 사용해 할당하면, 브라우저는 속성값을 이용해 새로운 함수를 만든다. 그리고 생성된 함수를 DOM 속성에 할당한다.  

따라서 DOM 속성을 사용해 핸들러를 만든 위 예시는 HTML 속성을 사용해 만든 바로 위쪽 예시와 동일하게 작동한다.

<br>

아래의 두 예시는 동일하게 작동한다.

1. HTML만 사용하는 방법
```bash
<input type="button" onclick="alert('클릭!')" value="클릭해주세요.">
```
2. HTML과 자바스크립트를 함께 사용하는 방법
```bash
<input type="button" id="button" value="클릭해주세요.">
<script>
    button.onclick = function() {
        alert('클릭!');
    };
</script>
```

두 예시의 유일한 차이점은, 첫 번째 예시에서는 HTML 속성을 사용해 `button.onclick`을 초기화하고, 두 번째 예시에서는 스크립트를 사용한다는 것이다.

<br>

`onclick` 속성은 단 하나밖에 없기 때문에, 복수의 이벤트 핸들러를 할당할 수 없다.

아래 예시와 같이 핸들러를 하나 더 추가하면, 기존 핸들러는 덮어씌워진다.

```bash
<input type="button" id="elem" onclick="alert('이전')" value="클릭해주세요.">
<script>
    elem.onclick = function() {     // 기존에 작성된 핸들러를 덮어썼다.
        alert('이후');              // 따라서 이 경고창만 보이게 된다.
    };
</script>
```

핸들러를 제거하고 싶다면 `elem.onclick = null`과 같이 `null`을 할당하면 된다.

<br>

### **this로 요소에 접근하기**

핸들러 내부에 쓰인 `this`의 값은 핸들러가 할당된 요소이다.

아래 예시의 `this.innerHTML`에서 `this`는 `button`이므로, 버튼을 클릭하면 버튼 안의 콘텐츠가 `alert` 창에 출력된다.

```bash
<button onclick="alert(this.innerHTML)">클릭해주세요.</button>
```

<br>

### **자주 하는 실수**

이벤트를 다룰 때는 아래 주의사항을 항상 염두에 두어야 한다.

이미 존재하는 함수를 직접 핸들러에 할당하는 예시를 살펴보자.

```bash
function sayGang() {
    alert('GANG');
}

elem.onclick = sayGang;
```

이 때 함수는 `sayGang`처럼 할당해야 한다. `sayGang()`을 할당하면 동작하지 않는다.

```bash
// 올바른 방법
button.onclick = sayGang;

// 틀린 방법
button.onclick = sayGang();
```

`sayGang()` 같이 괄호를 덧붙이는 것은 함수를 호출하겠다는 것을 의미한다. 위 예시의 마지막 줄처럼 `sayGang()`을 속성에 할당하면 함수 호출의 결과값이 할당된다. 함수 `sayGang`이 아무것도 반환하지 않는다면 `onclick` 속성엔 `undefined`가 할당되므로 이벤트가 원하는 대로 동작하지 않는다.

<br>

그런데, HTML 속성값에는 괄호가 있어야 한다.

```bash
<input type="button" id="button" onclick="sayGang()">
```

브라우저는 속성값을 읽고, 속성값을 함수 본문으로 하는 핸들러 함수를 만들기 때문에 이런 차이가 발생한다. 

브라우저는 `onclick` 속성에 새로운 함수를 할당한다.

```bash
button.onclick = function() {
    sayGang();  // 속성값
};
```

<br>

`setAttribute`로 핸들러를 할당해선 안된다.

따라서 아래의 코드는 동작하지 않는다.

```bash
// <body>를 클릭하면 에러가 발생한다.
// 속성은 항상 문자열이기 때문에, 함수가 문자열이 되어버리기 때문이다.

document.body.setAttribute('onclick', function() {alert(1)});
```

<br>

<u>DOM 속성은 대·소문자를 구분한다.</u>

핸들러 할당 시 `elem.onclick`은 괜찮지만, `elem.ONCLICK`은 안된다. DOM 속성은 대·소문자를 구분하기 때문이다.

<br>

### **addEventListener**

HTML 속성과 DOM 속성을 이용한 이벤트 핸들러 할당 방식엔 근본적인 문제가 존재한다. 하나의 이벤트에 복수의 핸들러를 할당할 수 없다는 문제이다.

버튼을 클릭하면 버튼을 강조하면서 메시지를 보여주고 싶다고 해보자.

이 때, 두 개의 이벤트 핸들러가 필요할 것인데, 기존 방법으로는 속성이 덮어씌워진다는 문제가 존재한다.

```bash
input.onclick = function() {alert(1);}
// ...
input.onclick = function() {alert(1);}  // 이전 핸들러를 덮어쓰게 된다.
```

<br>

웹 표준에 관여하는 개발자들은 오래전부터 이 문제를 인지하고, `addEventListener`와 `removeEventListener`라는 특별한 메서드를 이용해 핸들러를 관리하자는 대안을 제시했다. 

문법은 아래와 같다.

```bash
element.addEventListener(event, handler, [options]);
```

- `event` : 이벤트 이름 (ex. `click`)
- 'handler` : 핸들러 함수
- `options` : 아래 속성을 갖는 객체

    - `once` : `true`이면 이벤트가 트리거 될 때 리스너가 자동으로 삭제된다.
    - `capture` : 어느 단계에서 이벤트를 다뤄야 하는지를 알려주는 속성이다. 호환성 유지를 위해 `options`를 객체가 아닌 `false/true`로 할당하는 것이 가능한데, 이는 `{capture: false/true}와 동일하다.
    - `passive`: `true`이면 리스너에서 지정한 함수가 `preventDefault()`를 호출하지 않는다.
  
<br>

핸들러 삭제는 `removeEventListener`로 한다.

```bash
element.removeEventListener(event, handler, [options]);
```

<br>

> **삭제는 동일한 함수만 할 수 있다.**
>
> 핸들러를 삭제하려면 핸들러 할당 시 사용한 함수를 그대로 전달해주어야 한다.
>
> 아래와 같이 이벤트를 할당하고 삭제하면, 원하는 대로 작동하지 않는다.
>
> ```bash
> elem.addEventListener("click", () => alert('감사함둥'));
> // ...
> elem.removeEventListener("click", () => alert('감사함둥'));
> ```
> `removeEventListener`를 사용했지만, 핸들러는 지워지지 않는다. `removeEventListener`가 `addEventListener`를 사용해 할당한 함수와 다른 함수를 받고 있기 때문이다. 함수는 똑같이 생겼지만, 그럼에도 불구하고 다른 함수이기 때문에 이런 문제가 발생한다.
>
> 위 예시를 제대로 고치면 아래와 같다.
>
> ```bash
> function handler() {
>     alert('감사함둥');
> } 
>
> elem.addEventListener("click", handler);
> // ...
> elem.removeEventListener("click", handler);
> ```
>
> 변수에 핸들러 함수를 저장해 놓지 않으면 핸들러를 지울 수 없다는 것을 주의해야 한다. 이렇게 하지 않으면, `addEventListener`로 할당한 핸들러를 불러올 수 없다.
{: .prompt-danger}

<br>

`addEventListener`를 여러 번 호출하면 아래와 같이 핸들러를 여러 개 붙일 수 있다.

```bash
<input id="elem" type="button" value="클릭해주세요."/>

<script>
    function handler1() {
        alert('감사함둥');
    }

    function handler2() {
        alert('정말 감사함둥');
    }

    elem.onclick = () => alert("반갑슴둥");
    elem.addEventListener("click", handler1);   // 감사함둥
    elem.addEventListener("click", handler2);   // 정말 감사함둥
</script>
```

<br>

> **어떤 이벤트는 `addEventListener`를 써야만 동작한다.**
>
> DOM 속성에 할당할 수 없는 이벤트가 몇몇 존재한다. 이런 이벤트는 무조건 `addEventListener`를 써야 한다. 
>
> 문서를 읽고 DOM 트리 생성이 완료되었을 때 트리거 되는 이벤트인 `DOMContentLoaded`가 대표적인 예시이다.
>
> ```bash
> // 이 alert 창은 뜨지 않는다.
> document.onDOMContentLoaded = function() {
>     alert("DOM이 완성되었습니다.");
> }
> ```
>
> ```bash
> // 이 alert 창은 제대로 뜬다.
> document.addEventListener("DOMContentLoaded", function() {
>     alert("DOM이 완성되었습니다.");
> });
> ```
>
> 이처럼 `addEventListener`는 좀 더 범용적이다.
{: .prompt-warning}

<br>

### **이벤트 객체**

이벤트를 제대로 다루려면 어떤 일이 일어났는지 상세히 알아야 한다. `click` 이벤트가 발생했다면 마우스 포인터가 어디에 있는지, `keydown` 이벤트가 발생했다면 어떤 키가 눌렸는지 등에 대한 상세한 정보가 필요하다.

이벤트가 발생하면 브라우저는 **이벤트 객체(event object)**라는 것을 만든다. 여기에 이벤트에 관한 상세한 정보를 넣은 다음, 핸들러에 인수 형태로 전달한다.

아래는 이벤트 객체로부터 포인터 좌표 정보를 얻어내는 예시이다.

```bash
<input type="button" value="클릭해주세요." id="elem>

<script>
    elem.onclick = function(event) {
        // 이벤트 타입과 요소, 클릭 이벤트가 발생한 좌표를 보여준다.
        alert(event.type + "이벤트가 " + event.currentTarget + "에서 발생했습니다.");
        alert("이벤트가 발생한 곳의 좌표는 event.clientX + ":" + event.clientY + "입니다.");
    };
</script>
```

<br>

`event` 객체에서 지원하는 속성 중 일부는 다음과 같다.

- `event.type` : 이벤트 타입, 위의 예시에선 `"click"`.
- `event.currentTarget` : 이벤트를 처리하는 요소이다. 화살표 함수를 통해 핸들러를 만들거나 다른 곳에 바인딩하지 않은 경우에는 `this`가 가리키는 값과 같다. 화살표 함수를 사용했거나 함수를 다른 곳에 바인딩한 경우에는 `event.currentTarget`를 사용해 이벤트가 처리되는 요소 정보를 얻을 수 있다.
- `event.clientX / event.clientY` : 포인터 관련 이벤트에서, 커서의 상대 좌표(모니터 기준 좌표가 아닌, 브라우저 화면 기준 좌표)이다.

<br>

> **이벤트 객체는 HTML 핸들러에서도 접근할 수 있다.**
>
> HTML에서 핸들러를 할당한 경우에도 아래와 같이 `event` 객체를 사용할 수 있다.
>
> ```bash
> <input type"button" onclick="alert(event.type)" value="이벤트 타입">
> ```
>
> 브라우저는 속성을 읽고 `function(event) {alert(event.type)}` 같은 핸들러를 만들어내기 때문이다. 생성된 핸들러 함수의 첫 번째 인수는 `"event"`라 불리고, 함수 본문은 속성값을 가져온다.
{: .prompt-tip}

<br>

### **객체 형태의 핸들러와 handleEvent**

`addEventListener`를 사용하면 함수뿐만 아니라 객체를 이벤트 핸들러로 할당할 수 있다. 이벤트가 발생하면 객체에 구현한 `handleEvent` 메서드가 호출된다.

```bash
<button id="elem">클릭 좀</button>

<script>
    let obj = {
        handleEvent(event) {
            alert(event.type + "이벤트가 " + event.currentTarget + "에서 발생했습니다.");
        }
    };

    elem.addEventListener('click', obj);
</script>
```

보다시피 `addEventListener`가 인수로 객체 형태의 핸들러를 받으면 이벤트 발생 시 `obj.handleEvent(event)`가 호출된다.

<br>

클래스를 사용할 수도 있다.

```bash
<button id="elem">클릭 좀</button>

<script>
    class Menu {
        handleEvent(event) {
            switch(event.type) {
                case 'mousedown':
                    elem.innerHTML = "마우스 버튼을 눌렀습니다.";
                    break;
                case 'mouseup':
                    elem.innerHTML += " 그리고 버튼을 뗐습니다.";
                    break;
            }
        }
    }

    let menu = new Menu();
    elem.addEventListener('mousedown', menu);
    elem.addEventListener('mouseup', menu);
</script>
```

위 예시에선 하나의 객체에서 두 개의 이벤트를 처리하고 있다. 이 때 주의할 점은 `addEventListener`를 사용할 때는 요소에 타입을 정확히 명시해 주어야 한다는 점이다. 위 예시에서 `menu` 객체는 오직 `mousedown`과 `mouseup` 이벤트에만 응답하고, 다른 타입의 이벤트에는 응답하지 않는다.

<br>

`handleEvent` 메서드가 모든 이벤트를 처리할 필요는 없다. 이벤트 관련 메서드를 `handleEvent`에서 호출해서 사용할 수도 있다.

```bash
<button id="elem">클릭 좀</button>

<script>
    class Menu() {
        handleEvent(event) {
            // mousedown -> onMousedown
            let method = 'on' + event.type[0].toUpperCase() + event.type.slice(1);
            this[method](event);
        }

        onMousedown() {
            elem.innerHTML = "마우스 버튼을 눌렀습니다.";
        }

        onMouseUp() {
            elem.innerHTML += " 그리고 버튼을 뗐습니다.";
        }
    }

    let menu = new Menu();
    elem.addEventListener('mousedown', menu);
    elem.addEventListener('mouseup', menu);
</script>
```

이벤트 핸들러가 명확히 분리되었기 때문에 코드 변경이 원활해졌음을 알 수 있다.

<br>

## **이벤트 루프와 매크로태스크, 마이크로태스크**

브라우저 측 자바스크립트 실행 흐름은 Node.js와 마찬가지로 **이벤트 루프**에 기반한다.

따라서 이벤트 루프가 어떻게 동작하는지 알고 있어야 최적화나 올바른 아키텍쳐 설계가 가능해진다.

### **이벤트 루프**

이벤트 루프는 태스크가 들어오길 기다렸다가 태스크(작업)가 들어오면 이를 처리하고, 처리할 태스크가 없는 경우에는 잠드는, 끊임없이 돌아가는 자바스크립트 내 루프이다.

자바스크립트 엔진이 돌아가는 알고리즘을 일반화하면 다음과 같다.

1. 처리해야 할 태스크가 있는 경우: 
   - 먼저 들어온 태스크부터 순차적으로 처리한다.
2. 처리해야 할 태스크가 없는 경우: 
   - 잠들어 있다가 새로운 태스크가 추가되면 다시 1로 돌아간다.

바로 이 알고리즘이 우리가 브라우저를 사용해 인터넷을 서핑할 때 돌아가는 알고리즘이다. 이렇게 자바스크립트 엔진은 대부분의 시간 동안 아무런 일도 하지 않고 쉬고 있다가 스크립트나 핸들러, 이벤트가 활성화될 때만 돌아간다.

<br>

자바스크립트 엔진을 활성화하는 대표적인 태스크는 다음과 같다.
