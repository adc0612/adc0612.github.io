---
layout: post
comments: true
title: "[Typescript] JS to TS Project - 5. API 함수 타입 정의"
description: "[Typescript] JS to TS Project - 5. API 함수 타입 정의"
subject: typescript
categories: [ typescript ]
tags: [ typescript, api, axios, chart.js, interface]
---

<hr>

[1. axios api 함수 반환 타입 정의](#section1)  
[2. 개발자도구 network탭 data확인](#section2)  
[3. interface선언 후 객체타입 지정](#section3)  
[4. 타입 선언 모듈화](#section4)  
[5. chart.js getContext 에러](#section5)  
 

<hr>

## axios api 함수 반환 타입 정의 <a name="section1"></a>

`axios`의 데이터를 반환하는 함수가 있다.  
이 함수의 return(반환)타입을 지정해야 한다.

```typescript
// api
function fetchCovidSummary() {
  const url = 'https://api.covid19api.com/summary';
  return axios.get(url);
}
```

함수위에 커서를 올리면 VSCode의 타입추론에 의하여 타입을 추천해준다.

![axios api return 함수 타입 추론](/assets/img/typescript/ts-js-to-ts-project24.png "axios api return 함수 타입 추론")

타입추론에 나온 타입을 쓴다.

```typescript
// api
function fetchCovidSummary(): Promise<AxiosResponse<any>> {
  const url = 'https://api.covid19api.com/summary';
  return axios.get(url);
}
```

<hr>

## 개발자도구 network탭 data확인 <a name="section2"></a>

axios api 반환값으로 promise객체가 오는데 타입은 `Promise<AxiosResponse<any>>`다.  
여기서 any를 promise안의 data의 타입이 뭔지보고 정의 해줘야한다.

원래있던 js파일로 사이트를 구동하고 브라우저에 들어가서 개발자도구를 연다.  
`Network`탭에서 `XHR`필터를 걸고 어떤 data를 반환하는지 본다.  

![개발자도구 network탭 data확인 ](/assets/img/typescript/ts-js-to-ts-project25.png "개발자도구 network탭 data확인 ")

<hr>

## interface선언 후 객체타입 지정 <a name="section3"></a>

![Countries object data확인 ](/assets/img/typescript/ts-js-to-ts-project26.png "Countries object data확인 ")

`Countries`안에 위와 같은 속성들이 객체로 들어있는 것을 볼 수 있다.  
안에 속성들을 따로 interface를 빼서 선언한다.

```typescript
interface Country {
  Country: string;
  CountryCode: string;
  Date: string;
  ID: string;
  NewConfirmed: number;
  NewDeaths: number;
  NewRecovered: number;
  Premium: any; //비어있어서 임시로 any선언
  Slug: string;
  TotalConfirmed: number;
  TotalDeaths: number;
  TotalRecovered: number;
}
```

![Global object data확인 ](/assets/img/typescript/ts-js-to-ts-project27.png "Global object data확인 ")

`Global`객체도 interface로 만들어 선언한다. 

```typescript
interface Global {
  Date: string;
  NewConfirmed: number;
  NewDeaths: number;
  NewRecovered: number;
  TotalConfirmed: number;
  TotalDeaths: number;
  TotalRecovered: number;
}
```

두 인터페이스를 넣어 하나의 인터페이스를 선언 후 넣어준다.

```typescript
interface CovidSummaryResponse {
  Countries: Country[];
  Date: string;
  Global: Global;
  Message: string;
}

// api
function fetchCovidSummary(): Promise<AxiosResponse<CovidSummaryResponse>> {
  const url = 'https://api.covid19api.com/summary';
  return axios.get(url);
}

```

<hr>

## 타입 선언 모듈화 <a name="section4"></a>

interface 선언 하는 것은 따로 파일로 빼는 것이 좋다.  
비지니스 로직 작성하는 파일과 별도로 따로 파일을 만들어서 타입 선언하는 파일을 만드는 것이 코드가 간결해지고 나중에 보기도 편하다.  

`src`폴더 밑에 `covid`폴더 생성 후 `index.ts`파일을 생성 후 interface관련 된 내용을 넣었다.

`covid/index.ts`
```typescript
interface Country {
  Country: string;
  CountryCode: string;
  Date: string;
  ID: string;
  NewConfirmed: number;
  NewDeaths: number;
  NewRecovered: number;
  Premium: any;
  Slug: string;
  TotalConfirmed: number;
  TotalDeaths: number;
  TotalRecovered: number;
}

interface Global {
  Date: string;
  NewConfirmed: number;
  NewDeaths: number;
  NewRecovered: number;
  TotalConfirmed: number;
  TotalDeaths: number;
  TotalRecovered: number;
}

// export로 이 인터페이스를 내보낸다.
export interface CovidSummaryResponse {
  Countries: Country[];
  Date: string;
  Global: Global;
  Message: string;
}
```

`app.ts`에서 해당 인터페이스를 파일에서 import한다.

```typescript
// 타입 import
import { CovidSummaryResponse } from './covid/index';
```

<hr>

## chart.js getContext 에러 <a name="section5"></a>

chart.js `getContext` 타입이 없다고 에러가 난다.

![chart.js getContext error ](/assets/img/typescript/ts-js-to-ts-project27.png "chart.js getContext error ")

구글에서 검색하면 에러 해결이 나온다.

[stackoverflow chart.js 'getContext' does not exist on type 'HTMLElement'](https://stackoverflow.com/questions/58218597/property-getcontext-does-not-exist-on-type-htmlelement/58218739){: target="_blank"}

```typescript
// 두가지 방법
// 1. 따로 변수선언해서 타입단언
  const canvas = $('#lineChart') as HTMLCanvasElement;
  const ctx = canvas.getContext('2d');
// 2. 바로 괄호치고 타입단언
  const ctx = ($('#lineChart') as HTMLCanvasElement).getContext('2d');
```

<hr>
