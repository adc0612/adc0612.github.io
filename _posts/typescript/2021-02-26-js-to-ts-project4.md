---
layout: post
comments: true
title: "[Typescript] JS to TS Project - 4. 외부 라이브러리 모듈화 (axios, chart.js, Definitely Typed)"
description: "[Typescript] JS to TS Project - 4. 외부 라이브러리 모듈화 (axios, chart.js, Definitely Typed)"
subject: typescript
categories: [ typescript ]
tags: [ typescript, axios, chart.js, Definitely Typed]
---

<hr>

[1. axios 설치](#section1)  
[2. chart.js 설치](#section2)  
[3. chart.js import 에러](#section3)  
[4. Definitely Typed](#section4)  
[5. 타입선언 라이브러리가 제공되지 않는 외부 라이브러리 대처 방법 ](#section5)  

<hr>

## axios 설치 <a name="section1"></a>

[axios 공식 문서](https://github.com/axios/axios){: target="_blank"}

지금 프로젝트의 axios를 이용하여 데이터를 불러오고 있다.  
이 axios는 `index.html`에서 cdn서버를 이용해 불러오고 있다.  
이것을 axios 설치 후 타입스크립트내에서 모듈화를 진행 할 것이다.

먼저 axios를 아래 명령어로 설치한다.

```bash
npm i axios
```

> axios는 개발이후 배포할 때도 쓰일 라이브러리이므로 -D옵션을 안붙이고 설치한다.

`package.json`을 보면 `dependencies`에 axios가 있는 것을 볼 수 있다.

![package.json에서 axios설치 확인](/assets/img/typescript/ts-js-to-ts-project16.png "package.json에서 axios설치 확인")

설치 후 타입스크립트 파일 내에서 import해서 들고온다.  

```typescript
// 라이브러리 로딩
// import 변수명 from '라이브러리 이름';
// 변수, 함수 import 문법
// import {} from '파일 상대 경로';
// axios 로딩
import axios from 'axios';
```

<hr>

## chart.js 설치 <a name="section2"></a>

[chart.js 공식 문서](https://www.chartjs.org/){: target="_blank"}

자바스크립트로 차트를 만들어주는 오픈소스 api다.
chart.js도 아래 명령어로 설치한다.

```bash
npm i chart.js
```

`package.json`을 보면 `dependencies`에 axios와 함께 `chart.js`가 있는 것을 볼 수 있다.

![package.json에서 chart.js설치 확인](/assets/img/typescript/ts-js-to-ts-project17.png "package.json에서 chart.js설치 확인")

설치 후 타입스크립트 파일 내에서 import해서 들고온다.  
그러면 axios와 달리 에러가 나는것을 볼 수 있다.

<hr>

## chart.js import 에러 <a name="section3"></a>

`axios`와 달리 chart.js는 타입스크립트에서 바로 import해올 때 에러가 발생한다.  
`alt`키를 누른 후 axios를 누르면 axios를 정의해놓은 곳으로 이동이 된다.  
가서 보면 `index.d.ts`파일로 타입을 잘 정의해 놓은 것을 볼 수 있다.  
`chart.js`정의해놓은 곳으로 가면 `index.d.ts`파일이 없고 js파일만 있는것을 볼 수 있다.

![axios와 chart.js index.d.ts파일 차이](/assets/img/typescript/ts-js-to-ts-project19.png "axios와 chart.js index.d.ts파일 차이")

하지만 `chart.js`는 `index.d.ts`파일이 없어 타입정의가 안 되어 있으므로 해당 에러를 발생시킨다.

![chart.js import 에러](/assets/img/typescript/ts-js-to-ts-project18.png "chart.js import 에러")

에러를 읽거나 모르면 구글에서 검색하면 해결 방법이 나온다.  
에러를 해결하기위해 type이 정의 되어있는 chart.js를 설치하면 된다.

```bash
npm i @types/chart.js -D
```

하지만 설치 후에는 다른 에러를 보여준다.

![chart.js import 에러2](/assets/img/typescript/ts-js-to-ts-project20.png "chart.js import 에러2")

구글에 에러를 검색해서 찾았다.

[Import * as Chart from ‘chart.js’로 해야 하는 이유](https://stackoverflow.com/questions/56238356/understanding-esmoduleinterop-in-tsconfig-file){: target="_blank"}

chart.js import문을 타입단언을 이용해 바꾸면 에러가 해결된다.

```typescript
import * as Chart from 'chart.js';
```

<hr>

## Definitely Typed <a name="section4"></a>

[Definitely Type 공식 Github](https://github.com/DefinitelyTyped/DefinitelyTyped){: target="_blank"}

많은 js 라이브러리를 타입스크립트에서 사용하기 위해 타입을 정의해 놓은 것이 `Definitely Typed`라는 것이다.  
위에 있는 `chart.js`처럼 `@types`붙여있는것은 `Definitely Typed`에서 타입이 정의해져 있는 것이라 생각하면 된다.

[타입 정의가 제공되는 오픈소스 라이브러리 검색 사이트(타입스크립트 공식 사이트)](https://www.typescriptlang.org/dt/search?search=){: target="_blank"}

위 사이트에 가서 원하는 js라이브러리를 치면 타입스크립트에서 어떻게 이용할 수 있는지 찾아볼 수 있다.  
이렇게 `chart.js`를 입력하면 방금 설치했던 `@types/chart.js`를 설치하라 나온다.  

![타입정의가 제공되는 오픈소스 라이브러리 검색 사이트에서 chart.js 검색](/assets/img/typescript/ts-js-to-ts-project21.png "타입정의가 제공되는 오픈소스 라이브러리 검색 사이트에서 chart.js 검색")

<hr>

## 타입선언 라이브러리가 제공되지 않는 외부 라이브러리 대처 방법 <a name="section5"></a>

타입선언 라이브러리가 제공되지 않는 외부 라이브러리는 따로 직접 타입을 선언해 줄 수 있다.  
먼저 `tsconfig.json`에서 아래 속성을 추가한다.

```json
"typeRoots": ["./node_modules/@types", "./types"]
```

![tsconfig.json에서 typeRoots속성 추가](/assets/img/typescript/ts-js-to-ts-project22.png "tsconfig.json에서 typeRoots속성 추가")

`typeRoots` 속성에서 지정한 경로는 타입 선언을 직접 선언할 때 파일을 넣을 폴더경로다.  
`./node_modules/@types`는 안 정해도 기본으로 타입스크립트가 타입선언 라이브러리를 참고하는 경로이다.  
`./types`는 프로젝트 root에 폴더를 새로 만들어 안에 외부 라이브러리들의 직접 타입을 선언 할 수 있다.

예를 들어 `chart.js`의 타입을 선언하려면 `./types`밑에 `chart.js`폴더를 생성한다.  
`chart.js`폴더 생성 후 안에 `index.d.ts`파일을 생성한다.

![./types/chart.js/index.d.ts경로](/assets/img/typescript/ts-js-to-ts-project23.png "./types/chart.js/index.d.ts경로")

`index.d.ts`에서 원하는 타입으로 선언해주면 된다.

`index.d.ts`
```typescript
declare module 'chart.js' {
  //....
}

```

<hr>
