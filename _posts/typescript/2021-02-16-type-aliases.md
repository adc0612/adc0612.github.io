---
layout: post
comments: true
title: "[Typescript] 타입 별칭 (Type Aliases) "
description: "[Typescript] 타입 별칭 (Type Aliases) "
subject: typescript
categories: [ typescript ]
tags: [ typescript, function]
---

<hr>

## 타입 별칭 (Type Aliases)

타입 별칭은 특정 타입이나 인터페이스를 참조할 수 있는 타입 변수를 의미한다.  
string, number와 같은 간단한 타입 뿐만 아니라 interface 레벨의 복잡한 타입에도 별칭을 부여할 수 있다.  

```typescript
// MyString타입은 string을 뜻함 
type MyString = string;
let str:MyString = 'sdsdsd';

type TodoObj = {id: string; title: string; done: boolean};
function getTodo(todo:TodoObj) {
    
}
```

## 타입 별칭의 특징

타입 별칭은 새로운 타입 값을 하나 생성하는 것이 아니라 정의한 타입에 대해 나중에 쉽게 참고할 수 있게 이름을 지정하는 것과 같다.  
VSCode에서 프리뷰 상태로 타입 별칭과 인터페이스 차이가 있다.

타입 별칭 VSCode 프리뷰는 마우스만 올려도 type에 무슨 속성과 타입이 있는지 볼 수 있다.

![타입 별칭 VSCode 프리뷰](/assets/img/typescript/ts-type-aliases3.png "타입 별칭 VSCode 프리뷰")


인터페이스 VSCode 프리뷰는 마우스만 올리면 인터페이스인지만 보인다.

![interface VSCode 프리뷰1](/assets/img/typescript/ts-type-aliases1.png "interface VSCode 프리뷰1")

하지만 alt를 누르면 인터페이스도 속성과 타입이 보이고 해당 인터페이스로 이동이 된다.

![interface VSCode 프리뷰2](/assets/img/typescript/ts-type-aliases2.png "interface VSCode 프리뷰2")

<hr>

## 타입 별칭 VS 인터페이스

타입 별칭과 인터페이스의 가장 큰 차이점은 타입의 확장 가능 / 불가능 여부다.  
인터페이스는 확장이 가능한데 반해 타입 별칭은 확장이 불가능하다.  
따라서, 가능한한 `type` 보다는 `interface로` 선언해서 사용하는 것을 추천한다.

<hr>
