---
layout: post
comments: true
title: "[Typescript] JS to TS Project - 6. strict 타입 적용 (null 타입 오류)"
description: "[Typescript] JS to TS Project - 6. strict 타입 적용 (null 타입 오류)"
subject: typescript
categories: [ typescript ]
tags: [ typescript, tsconfig.json]
---

<hr>

[1. strict 옵션 적용](#section1)  
[2. strictFunctionTypes](#section2)  
[3. strictNullChecks 오류 해결 1_ if, 삼항연산자](#section3)  
[4. strictNullChecks 오류 해결 2_ non-null assertion](#section4)  
[5. strictNullChecks 오류 해결 3_옵셔널 체이닝 연산자](#section5)  
[5. strictNullChecks 오류 해결 4_DOM 유틸 함수(제네릭)](#section6)  
 

<hr>

## strict 옵션 적용 <a name="section1"></a>

타입스크립트 설정 파일 `tsconfig.json`에서 strict옵션을 추가한다.  
`"strict": true` 설정을 하면 아래 7개 옵션이 같이 설정된다.

[strict 공식 문서](https://www.typescriptlang.org/tsconfig#strict){: target="_blank"}

`tsconfig.json`
```json
{
    "compilerOptions": {
        "allowJs":true, // JS파일을 Typescript에서 인식해서 그냥 쓰게함
        "target": "ES5", // tsc로 compile후 TS파일을 JS파일로 변환될 때 원하는 JS버전
        "outDir": "./dist", // tsc로 compile후 변환된 JS파일의 경로 지정
        "moduleResolution": "node", // 모듈 (검색)해석 방식 설정
        // 컴파일에 포함될 라이브러리 파일 목록
        "lib": ["ES2015", "DOM", "DOM.Iterable"], // ES5에서 Promise문법을 이해 못 하기 때문
        "noImplicitAny": true,
        "esModuleInterop": true,
        // 타입 선언을 직접 선언할 때 파일을 넣을 폴더 설정
        "typeRoots": ["./node_modules/@types", "./types"],
        "strict": true, /* 모든 엄격한 타입-체킹 옵션 활성화 여부 */
        //strict 옵션들
        // "noImplicitAny": true, /* 'any' 타입으로 구현된 표현식 혹은 정의 에러처리 여부 */
        // "strictNullChecks": true, /* 엄격한 null 확인 여부 */
        // "strictFunctionTypes": true, /* 함수 타입에 대한 엄격한 확인 여부 */
        // "strictBindCallApply": true, /* 함수에 엄격한 'bind', 'call' 그리고 'apply' 메소드 사용 여부 */
        // "strictPropertyInitialization": true, /* 클래스의 값 초기화에 엄격한 확인 여부 */
        // "noImplicitThis": true, /* 'any' 타입으로 구현된 'this' 표현식 에러처리 여부 */
        // "alwaysStrict": true, /* strict mode로 분석하고 모든 소스 파일에 "use strict"를 추가할 지 여부 */
    },
    // 해당 프로젝트 폴더 기준으로 어디에 있는 TS파일을 compile시킬지 결정
    "include": ["./src/**/*"], // src폴더 밑에 있는 모든 파일들로 설정
    //  TS파일 compile제외 할 폴더
    "exclude": [
        "node_modules",
        "dist"
    ]
}
```

<hr>

## strictFunctionTypes <a name="section2"></a>

`strict`를 true값으로 바꾸면서 `strictFunctionTypes`에 관한 오류가 생겼다.

[strictFunctionTypes 공식 문서](https://www.typescriptlang.org/tsconfig#strictFunctionTypes){: target="_blank"}

![strictFunctionTypes에 의한 오류 확인 ](/assets/img/typescript/ts-js-to-ts-project29.png "strictFunctionTypes에 의한 오류 확인 ")

오류 확인을 하면 addEventListener에 콜백함수로 handleListClick이 들어가는데 이 인자 타입이 addEventListener에서 설정된 `Event`타입이 아니고 `MouseEvent`라 오류가 생겼다.  
handleListClick 함수의 인자를 MouseEvent에서 Event라 고치고 오류를 해결한다.

<hr>

## strictNullChecks 오류 해결 1_ if, 삼항연산자 <a name="section3"></a>

위에 오류를 해결해도 오류가 아직 있어서 확인해보니 `null`타입 오류 였다.  

`strict`를 true값으로 바꾸면서 `strictNullChecks`에 관한 오류가 생겼다.

[strictNullChecks 공식 문서](https://www.typescriptlang.org/tsconfig#strictNullChecks){: target="_blank"}

![null 타입 오류 확인 ](/assets/img/typescript/ts-js-to-ts-project30.png "null 타입 오류 확인 ")

해당 변수에 지정한 타입과 `null`이 들어올 수 있다는 것이다.  
`null`이 아닐 때만 작동이 되게 `if`문을 이용할 수 있다.

```typescript
// events
function initEvents() {
  if (!rankList) {
    return;
  }
  rankList.addEventListener('click', handleListClick);
}
```

같은 `null`오류를 `if`문대신 `삼항연산자`를 이용해도 된다.  
코드가 짧아지고 보기도 편하다.

```typescript
// before
selectedId = event.target.parentElement.id;

// after
selectedId = event.target.parentElement
    ? event.target.parentElement.id
    : undefined;
```

<hr>

## strictNullChecks 오류 해결 2_ non-null assertion <a name="section4"></a>

`null`오류를 `non-null assertion`을 통해 해결이 가능하다.  
`non-null assertion`은 해당 변수의 타입이 null이 아니라고 단정하는 타입 단언이다.  

```typescript
// before
deathsList.appendChild(li);

// non-null assertion 적용
deathsList!.appendChild(li);
```

non-null assertion을 이용하면 eslint에서 아래와 같은 경고를 준다.

![non-null assertion eslint 경고 ](/assets/img/typescript/ts-js-to-ts-project31.png "non-null assertion eslint 경고 ")

이유는 타입단언은 개발자 입장에서 타입을 단언해서 이용하는 것이라 예기치 못 하는 에러가 날 수도 있다는 것이다.

<hr>

## strictNullChecks 오류 해결 3_옵셔널 체이닝 연산자 <a name="section5"></a>

`null`오류를 `옵셔널 체이닝 연산자`을 통해 해결이 가능하다.  
`옵셔널 체이닝`은 null이나 undefined인 값이 반환되면, 즉시 중단하고 undefined를 반환하는 문법이다.

```typescript
// before
deathsList.appendChild(li);

// 옵셔널 체이닝 적용
deathsList?.appendChild(li);
```

위 코드는 아래와 같은 코드를 간결하게 한 것 이다.
```typescript
if (deathsList === null || deathsList === undefined) {
    return;
} else {
    deathsList.appendChild(li);
}
```
<hr>

## strictNullChecks 오류 해결 4_DOM 유틸 함수(제네릭) <a name="section6"></a>

DOM 유틸 함수 `null`오류를 `제네릭`을 통해 해결이 가능하다.  

```typescript
// before
// utils
function $(selector: string) {
  return document.querySelector(selector);
}
const rankList = $('.rank-list');

// 제네릭 적용
function $<T extends HTMLElement>(selector: string) {
  const element = document.querySelector(selector);
  return element as T;
}
const rankList = $<HTMLOListElement>('.rank-list');
```

많이 쓰는 HTMLElement를 default로 선언해서 사용 할 수도 있다.

```typescript
// before
function $<T extends HTMLElement>(selector: string) {
  const element = document.querySelector(selector);
  return element as T;
}
const deathsTotal = $('.deaths') as HTMLParagraphElement;

//default로 HTMLParagraphElement선언
function $<T extends HTMLElement = HTMLParagraphElement>(selector: string) {
  const element = document.querySelector(selector);
  return element as T;
}
// 제네릭을 안 붙여도 HTMLParagraphElement로 타입이 선언된다.
const deathsTotal = $('.deaths');

```
<hr>
