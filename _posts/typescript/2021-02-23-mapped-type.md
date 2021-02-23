---
layout: post
comments: true
title: "[Typescript] 맵드 타입 (Mapped Type)"
description: "[Typescript] 맵드 타입 (Mapped Type)"
subject: typescript
categories: [ typescript ]
tags: [ typescript, Mapped Type]
---

<hr>

## 맵드 타입 (Mapped Type)

맵드 타입은 기존에 정의되어 있는 타입을 새로운 타입으로 변환해 주는 문법이다.  
Javascript의 `map()` API 함수를 타입에 적용한 것과 같은 효과를 가진다.

<hr>

## Javascript map 함수

아래는 Javascript의 map API 함수 예제이다.

```javascript
let arr = [{ id: 1, value: 'Germany'}, { id: 2, value: 'France'}, { id: 3, value: 'Britain'}];
let result = arr.map(function(item) {
  return item.value;
});
console.log(result); // ['Germany', 'France', 'Britain'];
```

<hr>

## 맵드 타입의 기본 문법

맵드 타입의 문법은 Javascript의 `for..in`반복문과 비슷하다.

```javascript
// Javascript의 for..in 반복문
let strArr = ['a', 'b', 'c'];
for (let key in strArr){
    console.log(key); // 배열의 index (0, 1 ,2)
    console.log(strArr[key]); // 배열의 value (a, b, c)
}
```

타입스크립트의 맵드타입 `기본 문법`

```typescript
{ [ P in K ] : T }
{ [ P in K ] ? : T } // optional parameter이용
```

<hr>

## 맵드 타입 예제

```typescript
// 유니언 타입으로 정의
type Burgers = 'Cheese' | 'Veg' | 'Fish';

// 맵드 타입이용해서 모든 속성을 string으로 변경
type BurgerStr = {[K in Burgers]: string};

const burgerSet1: BurgerStr = {
    Cheese: 'Goda',
    Veg: 'Onion',
    Fish: 'Pollack',
}

// 맵드 타입이용해서 모든 속성을 boolean으로 변경
type BurgerBool = {[K in Burgers]: boolean};

const burgerSet2: BurgerBool = {
    Cheese: true,
    Veg: false,
    Fish: true,
}
```

<hr>