---
layout: post
comments: true
title: "[Typescript] 타입스크립트 기본 타입 "
description: "[Typescript] 타입스크립트 기본 타입 "
subject: typescript
categories: [ typescript ]
tags: [ typescript]
---

<hr>

[1. String](#ts-basic-1)  
[2. Number](#ts-basic-2)  
[3. Boolean](#ts-basic-3)  
[4. Array](#ts-basic-4)  
[5. Tuple](#ts-basic-5)  
[6. Any](#ts-basic-6)  
[7. Object](#ts-basic-7)  

<hr>

## String <a name="ts-basic-1"></a>

문자열 타입 선언

```typescript
// string 선언
let str:string = 'hello';
```

<hr>

## Number <a name="ts-basic-2"></a>

숫자 타입 선언

```typescript
// 숫자 선언
let num:number = 10;
```

<hr>

## Boolean <a name="ts-basic-3"></a>

boolean 타입 선언

```typescript
// boolean 선언
let isLoggedIn: boolean = false;
```

<hr>

## Array <a name="ts-basic-4"></a>

배열 타입 선언

```typescript
// 배열 선언
// 숫자 배열
let arr: Array<number> = [1, 2, 3];
let items: number[] = [1, 2, 3];
// string 배열
let strArr: Array<string> = ['a','b','c'];
let itemArr: string[] = ['a','b','c'];
```

<hr>

## Tuple <a name="ts-basic-5"></a>

튜플은 배열과 다르게, 여러 타입의 값들을 구겨넣을 수 있는 복합 타입이다.  
타입 표현은, 대괄호 안에 원하는 타입과 개수를 넣고싶은 만큼 넣으면 된다.  
값의 초기화는 배열과 동일하다.  

```typescript
// 튜플
// 배열 index마다 타입정할 수 있음
let address: [string, number] = ['seoul', 100];
```

<hr>

## Any <a name="ts-basic-6"></a>

단어 의미 그대로 모든 타입에 대해서 허용한다는 의미를 갖고 있습니다.  
기존에 자바스크립트로 구현되어 있는 웹 서비스 코드에 타입스크립트를 점진적으로 적용할 때 활용하면 좋은 타입입니다. 

```typescript
// 튜플
// 배열 index마다 타입정할 수 있음
let str: any = 'hi';
let num: any = 10;
let arr: any = ['a', 2, true];
```

<hr>

## Object <a name="ts-basic-7"></a>

Object(객체) 타입 선언

```typescript
// 객체
let obj: object = {};
// 객체 속성마다 타입정할 수 있음
let person: {name: string, age: number} = {
    name: 'alter',
    age: 10,
}
```

<hr>



