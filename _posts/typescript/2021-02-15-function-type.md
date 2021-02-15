---
layout: post
comments: true
title: "[Typescript] 타입스크립트 함수 타입 "
description: "[Typescript] 타입스크립트 함수 타입 "
subject: typescript
categories: [ typescript ]
tags: [ typescript, function]
---

<hr>

## 함수 선언 

함수 선언

```typescript
//  함수의 파라미터에 타입을 정의하는 방식
function sum(a:number, b:number) {
    return a + b;
}

// 함수의 반환 값에 타입을 정의하는 방식
function printNum():number {
    return 10;
}

// 함수의 타입을 정의하는 기본 방식
function subtract(a:number, b:number):number {
    return a - b;
}

```

<hr>

## 함수 - Parameter 제한하는 특성

타입스크립트에서 함수 선언시 함수의 파라미터(인자)를 제한하는 특성을 가지고 있다.  
JS에서는 인자를 2개 받는 함수를 선언해도 3개를 넣어도 받아들이는 유연함을 가지고 있다.  

![JS 함수 선언](/assets/img/typescript/ts-function1.png "JS 함수 선언")

하지만 타입스크립트는 오류로 잡아내 준다.

![TS 함수 선언](/assets/img/typescript/ts-function2.png "TS 함수 선언")


<hr>

## 함수 - Optional Parameter

타입스크립트에서 함수 선언시 함수의 파라미터(인자)로 `?`물음표를 넣으면 optional parameter로 지정이 가능하다.  
optional parameter는 parameter로 넣어도 되고 안 넣어도 에러로 안되게 한다.

Optional parameter 미지정

![TS 함수 Optional parameter 미지정](/assets/img/typescript/ts-function3.png "TS 함수 Optional parameter 미지정")

Optional parameter 지정

![TS 함수 Optional parameter 지정](/assets/img/typescript/ts-function4.png "TS 함수 Optional parameter 지정")

<hr>
