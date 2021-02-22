---
layout: post
comments: true
title: "[Typescript] 유틸리티 타입 (Utility Type) - 1. Pick & Omit"
description: "[Typescript] 유틸리티 타입 (Utility Type) - 1. Pick & Omit"
subject: typescript
categories: [ typescript ]
tags: [ typescript, Utility Type]
---

<hr>

## 유틸리티 타입 (Utility Type)?

유틸리티 타입은 타입스크립트내에서 제공하는 타입문법으로 이미 정의해 놓은 타입을 변환할 때 사용하기 좋다.  
유틸리티 타입을 꼭 쓰지 않더라도 기존의 인터페이스, 제네릭 등의 기본 문법으로 충분히 타입을 변화할 수 있지만, 유틸리티 타입을 쓰면 더 간편하게, 간결하게 타입을 정의 할 수 있다.  
유틸리티 타입이 많지만 3개만 소개할 예정이다.  

[타입스크립트 유틸리티 타입 공식 문서](https://www.typescriptlang.org/docs/handbook/utility-types.html){: target="_blank"}

<hr>

## Pick

픽(Pick) 타입은 특정 타입에서 몇 개의 속성을 선택(pick)하여 타입을 정의할 때 쓰인다.

밑에 코드를 보면 `Product` 인터페이스가 있고 `fetchProducts`함수는 Product 인터페이스 타입을 가지고 있는 변수를 return한다.

```typescript
interface Product {
    id: number,
    name: string;
    price: number;
    brand: string;
    stock: number;
}

// 상품 목록을 받아오는 API함수
function fetchProducts(): Promise<Product[]> {
    //...
}
```

Product인터페이스의 특정 속성만 인자로 가지는 함수를 구현하고 싶다.  
그럴러면 또 인터페이스를 만들어 아래와 같이 지정해야한다.

```typescript
interface ProductDetail {
    id: number,
    name: string,
    price: number,
}

// 특정 상품의 상세정보를 표시하는 함수
function displayProductDetail(shoppingItem: ProductDetail) {
    // ...
}
```

이때, 따로 인터페이스를 선언하지말고 `Pick`을 이용해 간단하게 Product 인터페이스에서  `id, name, price`만 가져올 수 있다.  
`Pick`을 사용해 타입별칭에 넣어줬다.

```typescript
// Pick 이용
type ProductDetail = Pick<Product, 'id' | 'name' | 'price'>;
function displayProductDetail(shoppingItem: ProductDetail) {
    // ...
}
```

<hr>

## Omit

오밋(Omit) 타입은 특정 타입에서 몇 개의 속성을 제거하여 타입을 정의할 때 쓰인다. (Pick의 반대 개념)  
인터페이스에서 제외할 속성을 지정하면 인터페이스에서 해당속성들을 제외한 속성들이 추가된다.  
밑에 코드는 `ProductDetail2`에 Product의 `brand, stock`을 제외하고 `id, name, price`만 추가했다.

```typescript
// Omit 이용
type ProductDetail2 = Omit<Product, 'brand' | 'stock'>;
function displayProductDetail2(shoppingItem: ProductDetail) {
    // ...
}

```

<hr>

