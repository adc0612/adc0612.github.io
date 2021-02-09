---
layout: post
comments: true
title: "타입스크립트가 무엇인가? 왜 써야하는가? "
description: "타입스크립트가 무엇인가? 왜 써야하는가? "
subject: typescript
categories: [ typescript ]
tags: [ typescript]
---

<hr>

## 타입스크립트가 무엇인가?

타입스크립트는 한 문단으로 표현하면 JS에 타입을 부여한 언어다.  
타입스크립트는 JS의 확장된 언어라고 볼 수 있고 JS의 SuperSet(상위 집합)언어라고 볼 수 있다.  

<hr>

## 타입스크립트를 왜 써야하는가?

[* 정적 타입 검사자](#ts-1)  
[* 코드 자동 완성과 가이드](#ts-2) 

### 정적 타입 검사자 <a name="ts-1"></a>

JS는 오류를 코드 상에서 아닌 실행 후 콘솔 창에서 오류를 확인 할 수 있다. 하지만 타입스크립트는 코드치면서 확인이 가능하다. 프로그램을 실행시키지 않으면서 코드의 오류를 검출하는 것을 정적 검사라고 한다. 정적 타입 검사자인 TypeScript는 프로그램을 실행시키기 전에 값의 종류를 기반으로 프로그램의 오류를 찾는다. 

다음은 Javascript 코드다.  
string객체인 ""를 number객체인 0 이랑 비교하는 것은 대부분 프로그래밍 언어에서 오류로 표출하고 실행이 안된다.  
하지만 Javascript는 실행이 된다.  
JavaScript의 동일 연산자는 `(==)` 인수를 강제로 변환하여, 예기치 않은 동작을 유발합니다.  

```javascript
if ("" == 0) { 
}
```

이 코드를 타입스크립트에 쓰고, VSCode에서 확인하면 빨간줄이 쳐지며 오류를 확인할 수 있다.

![타입스크립트 오류 표출화면1](/assets/img/typescript/ts-intro1.png "타입스크립트 오류 표출화면1")

JavaScript는 또한 존재하지 않는 프로퍼티의 접근을 허용한다.  
밑에 코드에는 teacher라는 property가 있지만 없는 property인 teach를 접근한다.  

```javascript
const ppl = { student: 5, teacher: 2 };
const tot = ppl.student * ppl.teach;
```

이 코드도 타입스크립트에 쓰고, VSCode에서 확인하면 빨간줄이 쳐지며 오류를 확인할 수 있다.

![타입스크립트 오류 표출화면2](/assets/img/typescript/ts-intro2.png "타입스크립트 오류 표출화면2")


### 코드 자동 완성과 가이드 <a name="ts-2"></a>

타입스크립트는 개발 툴에서 코드 자동 완성과 가이드를 제대로 제공한다. (Visual Studio Code기준)  
아래 코드는 개발자가 `sum()`함수의 결과를 예상하고 `total 타입`을 `number`라고 가정한 상태에서 `number`의 API인 `toString()`를 작성하려 한다.  
JS는 코드를 작성 할 때 `total`이라는 변수의 타입이 `number`라는 것을 인지 못 한다.  
그래서 API를 일일이 작성해야한다. 이렇게 되면 오타가 날 확률도 높다.

![JS 샘플코드](/assets/img/typescript/ts-intro3.png "JS 샘플코드")

하지만 타입스크립트는 `sum()`함수의 결과인 `total 타입`을 `number`로 인식하고 있어, `number`의 API들을 보여주며 자동완성 해준다.

![TS 샘플코드](/assets/img/typescript/ts-intro4.png "TS 샘플코드")

<hr>



