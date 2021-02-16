---
layout: post
comments: true
title: "[Typescript] 타입스크립트 인터페이스 (interface) "
description: "[Typescript] 타입스크립트 인터페이스 (interface) "
subject: typescript
categories: [ typescript ]
tags: [ typescript, function]
---

<hr>

## 인터페이스

인터페이스는 상호 간에 정의한 약속 혹은 규칙을 의미한다.

User라는 인터페이스를 지정하고 들어갈 속성과 타입을 정해놓고 타입에 써주면, 인터페이스에 있는 그대로 타입이 부여된다.

```typescript
interface User {
    age: number;
    name: string;
};

// 변수에 interface활용
let seho: User = {
    age: 33,
    name: 'seho',
}
```

## Option 속성

`Option(?)` 속성을 이용하면 인터페이스를 사용할 때 인터페이스에 정의되어 있는 속성을 모두 다 꼭 사용하지 않아도 된다.

```typescript
// 옵션(?) 활용
// name은 사용해도 되고 안 해도 된다.
interface User2 {
    age: number;
    name?: string;
};

let tak: User2 = {
    age: 25,
}

```

<hr>

## 함수의 인자를 정의하는 인터페이스

인터페이스를 이용해 들어오는 인자의 타입을 지정할 수 있다.

```typescript
interface User {
    age: number;
    name: string;
};

// 함수의 인자를 정의하는 인터페이스
function getUser(user: User) {
    console.log(user);
}

let titan = {
    name: 'titan',
    age: 30,
}

getUser(titan);
```

하지만 인터페이스를 이용해 인자로 받아 사용할 때 항상 인터페이스의 속성 갯수와 인자로 받는 객체의 일치시키지 않아도 된다.  
다시 말해, 인터페이스에 정의된 속성, 타입의 조건만 만족한다면 객체의 속성 갯수가 더 많아도 상관 없다는 의미다.  
또한, 인터페이스에 선언된 속성 순서를 지키지 않아도 된다. 

```typescript
// job속성 추가
let capt = {
    job: 'captain',
    name: 'capt',
    age: 50,
}

getUser(capt); // 에러 없이 인자로 잘 들어감

// 에러 발생
let irobot = {
    job: 'navigator',
    age: 120,
}

getUser(irobot); //Property 'name' is missing in type '{ job: string; age: number; }' but required in type 'User'.
```

<hr>

## 함수 구조를 정의하는 인터페이스

함수 구조를 인터페이스로 지정할 수 있다.  

```typescript
// 함수 구조를 정의하는 인터페이스
interface SumFunction {
    (a:number, b:number): number;
}

let sumFunct: SumFunction;

sumFunct = function(a:number, b:number): number {
    return a + b;
}
```

<hr>

## 인덱싱 방식을 정의하는 인터페이스

배열을 정의할 때 인터페이스로 지정할 수 있다.

```typescript
// 인덱싱 방식을 정의하는 인터페이스
interface StringArray {
    [index: number]: string;
}

let strarr:StringArray;
strarr = ['a','b','c'];
strarr[0] = 't'; // t
// strarr[0] = 10; // Type 'number' is not assignable to type 'string'.
```
<hr>

##  Dictionay 패턴

인터페이스로 Object를 정의할 때 속성을 따로 지정하지않고 key type과 value type만 지정할 수 있다.

```typescript
// 일반 패턴
interface User {
    age: number;
};

// Dictionay 패턴 
interface StringRegexDictionary {
    [key: string]: RegExp;
}

let regexObj:StringRegexDictionary = {
    cssFile: /\.css$/,
    jsFile: /\.js$/,
};

regexObj['cssFile'] =/\.scss$/;
// regexObj['cssFile'] = 'a'; //Type 'string' is not assignable to type 'RegExp'.
```

<hr>

## 인터페이스 확장

인터페이스도 extends로 확장 할 수 있다.  
Person 인터페이스를 정의하고 Developer는 Person을 확장해서 정의했다.  
그래서 captain은 Developer의 인터페이스를 이용함에 불구하고 Person의 name, age도 써줘야 한다.  

```typescript
// 인터페이스 확장
interface Person {
    name: string;
    age: number;
}

interface Developer extends Person{
    language: string;
}

let captain:Developer = {
    language: 'ts',
    age: 20,
    name: 'captain',
}
```

<hr>
