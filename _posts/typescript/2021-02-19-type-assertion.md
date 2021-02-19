---
layout: post
comments: true
title: "[Typescript] 타입 단언 (type assertion)"
description: "[Typescript] 타입 단언 (type assertion)"
subject: typescript
categories: [ typescript ]
tags: [ typescript, generics]
---

<hr>

## 타입 단언 (type assertion)

타입스크립트의 타입 추론 기능은 매우 강력하지만 어쩔 수 없는 한계가 존재한다.  
타입 단언은 타입스크립트 컴파일러가 타입을 실제 런타임에 존재할 변수의 타입과 다르게 추론하거나 너무 보수적으로 추론하는 경우에 프로그래머가 수동으로 컴파일러한테 특정 변수에 대해 타입 힌트를 주는 것이다.  

```typescript
// 타입 단언 type-assertion
let a;
a = 'str';
a = 20;
let b = a; // a는 any이므로 b도 any로 추론
```

위의 코드에서 b는 a를 받으므로 any를 추론한다.  
하지만 a가 string값으로 올 것을 프로그래머가 확신할 때 `as`를 이용해 타입을 지정할 수 있다.

```typescript
let b = a as string; //수동으로 a를 string으로 지정
```

<hr>

## 타입 단언 활용

타입 단언은 주로 DOM api를 조작할 때 많이 사용된다.  
아래 코드를 보면 `HTMLDivElement | null`타입이 추론 되어있어서 에러를 표출한다.

```typescript
let div = document.querySelector('div'); // div: HTMLDivElement | null
```

그럴때는 타입 단언을 이용하여 `HTMLDivElement`타입을 부여한다.  

```typescript
let div = document.querySelector('div') as HTMLDivElement; 
```


<hr>
