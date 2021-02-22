---
layout: post
comments: true
title: "[Typescript] 유틸리티 타입 (Utility Type) - 2. Partial"
description: "[Typescript] 유틸리티 타입 (Utility Type) - 2. Partial"
subject: typescript
categories: [ typescript ]
tags: [ typescript, Utility Type]
---

<hr>

## Partial

파셜(Partial) 타입은 특정 타입의 `부분 집합`을 만족하는 타입을 정의한다.

밑에 코드를 보면 `Product` 인터페이스가 있고 `Product`안 속성하나만 들어가도 `updateProductItem`함수의 인자로 넘길려한다.  
그래서 `UpdateProduct`를 `Product`의 `Partial`로 정의했다.

```typescript
interface Product {
    id: number,
    name: string;
    price: number;
    brand: string;
    stock: number;
}

// Partial 이용
type UpdateProduct = Partial<Product>;
// 특정 상품정보를 업데이트하는 함수 (모든속성은 optional)
function updateProductItem(produtItem: UpdateProduct) {
    // ...
}
```

`UpdateProduct` 타입별칭에는 아래와 같은 속성과 타입이 들어가있다.  
모든 속성이 `Optional Parameter`로 되어있다.

![UpdateProduct 속성과 타입](/assets/img/typescript/ts-utility-type1.png "UpdateProduct 속성과 타입")

<hr>

## Partial 구현

Partial을 단계별로 구현해봤다.  

### 1. Optional Parameter

일단 속성들을 `Optional Parameter`로 구성했다.

```typescript
// 1.Optional Parameter
type UpdateProduct1 = {
    id?: number,
    name?: string;
    price?: number;
    brand?: string;
    stock?: number;
}
```

### 2. key로 속성을 연결

코드를 원본 `Product`의 의존 하기위해 `Product`인터페이스를 들고와서 배열의 key를 속성으로 들고와서 연결했다.

```typescript
// 2. key로 속성을 연결
type UpdateProduct2 = {
    id?: Product['id'],
    name?: Product['name'];
    price?: Product['price'];
    brand?: Product['brand'];
    stock?: Product['stock'];
}
```

### 3. for..in으로 구성

Javascript의 `for..in`반복문 비슷하게 구현을 할 수 있다.  

```typescript
// 3. for..in으로 구성
// Javascript에서 배열안 key값들을 접근하는 for..in반복문 같은 것 
type UpdateProduct3 = {
    [P in 'id' | 'name' | 'price' | 'brand' | 'stock']?: Product[P];
}
```

### 4. for..in keyof 이용

`keyof`를 이용해 Product인터페이스에 key값들만 가져와서 연결했다.

```typescript
// 4. for..in keyof 이용
type UpdateProduct4 = {
    [P in keyof Product]?: Product[P];
}
```

### 5. 제네릭 이용

위에 같이 Product 인터페이스를 쓰면 Product 인터페이스에만 `한정`되기 때문에 `제네릭`으로 선언했다.

```typescript
// 5. 제네릭 이용
type PartialImplement<T> = {
    [P in keyof T]?: T[P];
}
```

실제로 `Partial`에 커서를 올려보면 위와 같은 코드로 구성 되어있다.

![Partial 코드 구성](/assets/img/typescript/ts-utility-type2.png "Partial 코드 구성")

<hr>

