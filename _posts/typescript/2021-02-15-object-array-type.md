---
layout: post
comments: true
title: "[Typescript] 타입스크립트 Object 배열 타입 "
description: "[Typescript] 타입스크립트 Object 배열 타입 "
subject: typescript
categories: [ typescript ]
tags: [ typescript, function]
---

<hr>

## Object 배열 타입 선언 

JS Object 배열 선언 

```javascript
let todoItems;

function showCompleted() {
  return todoItems.filter(item => item.done);
}
```

TS Object 배열 선언 

```typescript
let todoItems: object[];;

function showCompleted() {
  return todoItems.filter(item => item.done);
}
```

이렇게 `object[]`로 선언도 가능하다.  
하지만 object의 속성 타입이 안 정해져 있어 에러가 난다. 

![TS Object Array 오류](/assets/img/typescript/ts-object-array1.png "TS Object Array 오류")

object배열은 속성 타입까지 정해줘야 에러없이 진행된다.

```typescript
let todoItems: { id: number; title: string; done: boolean }[];

function showCompleted() {
  return todoItems.filter(item => item.done);
}
```

<hr>

## 함수 Object 반환 타입 선언 

JS 함수 Object 반환 선언 

```javascript
// api
function fetchTodoItems() {
  const todos = [
    { id: 1, title: '안녕', done: false },
    { id: 2, title: '타입', done: false },
    { id: 3, title: '스크립트', done: false },
  ];
  return todos;
}
```

TS 함수 Object 반환 선언

이렇게 `object[]`로 선언도 가능하다.  
하지만 object의 속성 타입이 안 정해져 있어 에러가 난다. 

```typescript
// api
function fetchTodoItems(): object[] {
  const todos = [
    { id: 1, title: '안녕', done: false },
    { id: 2, title: '타입', done: false },
    { id: 3, title: '스크립트', done: false },
  ];
  return todos;
}
```

이렇게 object의 속성 타입까지 지정한다.

```typescript
// api
function fetchTodoItems(): { id: number; title: string; done: boolean }[] {
  const todos = [
    { id: 1, title: '안녕', done: false },
    { id: 2, title: '타입', done: false },
    { id: 3, title: '스크립트', done: false },
  ];
  return todos;
}
```

<hr>

## 인터페이스 (interface) 활용

같은 object의 속성 타입을 계속 길게 써주는 불편함이 있다.  
그것을 해결하기 위해 interface를 가지고 같은 object의 속성을 지정한다.

```typescript
interface Todo {
  id: number;
  title: string;
  done: boolean;
}

let todoItems: Todo[];

// api
function fetchTodoItems(): Todo[] {
  const todos = [
    { id: 1, title: '안녕', done: false },
    { id: 2, title: '타입', done: false },
    { id: 3, title: '스크립트', done: false },
  ];
  return todos;
}
```
<hr>
