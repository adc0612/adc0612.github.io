---
layout: post
comments: true
title: "[Typescript] 타입 호환 (type compatibility)"
description: "[Typescript] 타입 호환 (type compatibility)"
subject: typescript
categories: [ typescript ]
tags: [ typescript, type compatibility]
---

<hr>

## 타입 호환 (type compatibility)

타입 호환이란 타입스크립트 코드에서 특정 타입이 다른 타입에 잘 맞는지를 의미한다. 

### 인터페이스 (interface)

```typescript
// 1. interface
interface Burger {
    name: string;
    ingredients: string;
}

interface Candy {
    name: string;
}

let burger: Burger;
let candy: Candy;

// burger = candy; // 호환 불 가능
candy = burger; // 호환가능
```

burger에는 candy가 가진 속성과 타입이 부족하므로 호환이 `안 된다`.  
candy에는 burger가 가진 속성과 타입이 부족하므로 호환이 `된다!`.

### 함수 (function)

```typescript
// 2. function
let oneNum = function (a: number) {
    // ...
}

let twoNum = function (a: number, b: number) {
    // ...
}

// oneNum = twoNum // 
twoNum = oneNum // 
```

oneNum에는 인자가 하나 들어가있어 인자를 두 개 넘기는 twoNum은 호환이 `안 된다`.  
twoNum에는 인자가 두개가 들어가있어 인자를 한 개 넘기는 oneNum은 호환이 `된다`.

### 제네릭 (Generic)

```typescript
// 3. Generic
interface Empty<T> {
    // ...
} 
let empty1: Empty<string>;
let empty2: Empty<number>;
// 제네릭에 들어가는 타입은 다르지만 인터페이스 내부는 비어있으므로 호환이 됨!
empty1 = empty2  
empty2 = empty1
```

제네릭을 가지고 있는 `Empty`인터페이스를 받아 타입을 다르게 두 변수에 넣었다.  
인터페이스 내부는 비어있으므로 둘은 호환이 된다.  

```typescript
interface NotEmpty<T>{
    data: T;
}
let notEmpty1: NotEmpty<string>;
let notEmpty2: NotEmpty<number>;
// 제네릭에 들어가는 다른 타입이 각 속성에 들어가기 때문에 호한이 안 됨
// notEmpty1 = notEmpty2;
// notEmpty2 = notEmpty1;
```

여기서는 제네릭을 가지고 있는 `NotEmpty`인터페이스에 제네릭 타입을 가지고 data속성을 추가해서 넣었다.  
이 인터페이스를 받아 두 변수를 선언한 후 호환 체크를 하니 호환이 안 된다고 나온다.  
다른 타입이 각 속성에 들어가기 때문에 호한이 안 된다.

<hr>

