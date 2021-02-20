---
layout: post
comments: true
title: "[Typescript] 타입 가드 (type guard)"
description: "[Typescript] 타입 가드 (type guard)"
subject: typescript
categories: [ typescript ]
tags: [ typescript, type guard]
---

<hr>

## 타입 가드로 주는 편의성

```typescript
 interface Burger {
     name: string;
     ingredients: string;
 }

 interface Fries {
     name: string;
     size: number;
 }

 function introduce(): Burger | Fries {
     return {name: 'Cheese Burger', size: 3, ingredients: 'Cheese'}
 }
 
 let order = introduce();
//  console.log(order.ingredients); //공통되지않은 속성 접근 불가능
//  console.log(order.size); //공통되지않은 속성 접근 불가능
```

위 코드에 두 인터페이스가 있고 함수에는 리턴값으로 두 인터페이스의 유니언 타입으로 지정했다.  
변수에 그 함수를 호출하고 리턴값으로 받았다.  
두 인터페이스의 유니언 값이므로 Fries의 size와 Burger의 ingredients를 접근하려 했지만, 타입추론으로 된 타입값이 유니언 값이라 공통된 속성 name만 접근가능했다.

![유니언은 공통된 속성만 접근 가능한 것을 보여주는 화면](/assets/img/typescript/ts-type-guard1.png "유니언은 공통된 속성만 접근 가능한 것을 보여주는 화면")

그래서 타입 단언을 활용해 각 속성을 접근할 때 해당 타입들을 지정했다.  

```typescript
// 타입 단언 이용
if ((order as Burger).ingredients) {
    let ingredients = (order as Burger).ingredients;
    console.log(ingredients);
}else if ((order as Fries).size){
    let size = (order as Fries).size;
    console.log(size);
}
```

하지만 이렇게 되면 코드가 길어지므로 이럴 때 `타입 가드`를 이용한다.

<hr>

## 타입 가드

```typescript
// 함수에 타입 가드 정의
// target이 Burger interface면 true 반환
// target이 Fries interface면 false 반환
function isBurger(target: Burger | Fries): target is Burger {
    return (target as Burger).ingredients !== undefined;
}
```

타입 가드를 이용해 target이 Burger 인터페이스면 `true`를 Fries 인터페이스면 `false`를 반환하게 한다.  
그러면 일일히 각각 타입 체크할 때마다 타입 단언을 이용 안해도 된다.   

```typescript
if (isBurger(order)) {
    console.log(order.ingredients); //Burger Interface
} else {
    console.log(order.size); //Fries Interface
}
```

<hr>
