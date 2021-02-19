---
layout: post
comments: true
title: "[Typescript] 타입 추론 (type inference)"
description: "[Typescript] 타입 추론 (type inference)"
subject: typescript
categories: [ typescript ]
tags: [ typescript, generics]
---

<hr>

## 타입 추론 (type inference)

타입스크립트 파일내에서 변수를 선언하면 VSCode 내부적으로 language server가 돌아가며 변수의 타입을 추론한다.  
아무값을 안주고 변수를 선언하면 VSCode의 language server가 아래와 같이 `any`라 추론한다.  

![변수 초기 타입 추론](/assets/img/typescript/ts-type-inference1.png "변수 초기 타입 추론")

그리고 안에 값을 넣고 커서를 올려보면 값에 따라 타입을 추론해서 보여준다.

```typescript
let a = 10; //number
let b = 'str'; //string
```

<hr>

## 인터페이스와 제네릭을 이용한 타입 추론

인터페이스와 제네릭을 선언했다.  
새 객체를 선언 후 해당 인터페이스를 넣고 타입을 넣으면 자동으로 추론이 돼서 보여지는 것을 볼 수 있다.

```typescript
// 인터페이스와 제네릭을 이용한 타입 추론
interface BurgerSet<T> {
    main: T;
    size: number;
}

let set1: BurgerSet<string> = {
    main: 'cheeseBurger',
    size: 2,
}
```

![인터페이스와 제네릭을 이용한 타입 추론](/assets/img/typescript/ts-type-inference2.png "인터페이스와 제네릭을 이용한 타입 추론")

<hr>

## 인터페이스와 제네릭의 extends

인터페이스와 제네릭을 선언한 후 그 인터페이스를 상속하면 제네릭에 선언한 타입을 상속한 인터페이스에서도 같은 타입으로 추론이 돼서 보여진다. 

![인터페이스와 제네릭의 extends 타입 추론](/assets/img/typescript/ts-type-inference3.png "인터페이스와 제네릭의 extends 타입 추론")

<hr>

## Best Common Type 추론 방식

타입을 추론할 때 변수 안에 있는 data기준으로 추론한다.   
만약 한 변수안에 여러개의 타입이 있으면 `가장 근접한 타입`을 추론한다.  
그것이 `Best Common Type`이다.

![Best Common Type 추론](/assets/img/typescript/ts-type-inference4.png "Best Common Type 추론")

<hr>
