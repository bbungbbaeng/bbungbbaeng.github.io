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

![호스트 환경]({{ site.google_drive }}1WFnYNn0Q9abyYDYadPOIV2g_MMwm01JB&sz=w1000){:height="300" .left}