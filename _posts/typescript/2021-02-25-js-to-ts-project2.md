---
layout: post
comments: true
title: "[Typescript] JS to TS Project - 2. 점진적인 타입 선언"
description: "[Typescript] JS to TS Project - 2. 점진적인 타입 선언"
subject: typescript
categories: [ typescript ]
tags: [ typescript, enum]
---

<hr>

[1. 명시적인 any 선언](#section1)  
[2. 확실한 변수 타입 선언](#section2)  
[3. enum 활용](#section3)  
[4. DOM 함수 타입](#section4)  

<hr>

## 명시적인 any 선언 <a name="section1"></a>

`tsconfig.json`파일에 `noImplicitAny`값을 `true`로 추가하고 저장한다.

![noImplicitAny값 추가](/assets/img/typescript/ts-js-to-ts-project1.png "noImplicitAny값 추가")

저장 후 error가 많아졌음을 볼 수 있다.  
에러를 해결해주기위해 일단은 모든 에러나는 변수의 타입을 `any`라고 선언한다.  
선언하면 에러가 없어지는것을 볼 수 있다. (밑 스크린샷 노란색 하이라이트 참고)

![화살표함수 인자 타입에러](/assets/img/typescript/ts-js-to-ts-project2.png "화살표함수 인자 타입에러")

하지만 위 스크린샷에 빨간색 하이라이트 부분과 같은 화살표 함수 인자에는 선언해도 에러가 나는 것을 볼 수 있다.  
저런 `괄호를 벗긴 화살표 함수 인자`들은 괄호로 한 번 묶어준다음 선언해주면 에러가 해결되는 것을 볼 수 있다.

![화살표함수 인자 타입에러 해결](/assets/img/typescript/ts-js-to-ts-project3.png "화살표함수 인자 타입에러 해결")

<hr>

## 확실한 변수 타입 선언 <a name="section2"></a>

변수를 살펴보고 확실한 타입이면 선언한다.  
string이 들어가면 string, number가 들어가면 number, date면 Date  
Object는 안에 속성의 타입도 정해야하기 때문에 잘 살펴보고 하는 것이 좋다.  

<hr>

## enum 활용 <a name="section3"></a>

변수에 들어가는 값이 한정되어 있으면 enum을 선언해서 사용하는 것이 좋다.

![enum 선언 전](/assets/img/typescript/ts-js-to-ts-project4.png "enum 선언 전")

여기서 status의 인자로는 confirmed, recovered, deaths값 밖에 들어가지 않는다.  
그래서 이것을 묶어 enum으로 선언 후 사용한다.

![enum 선언 후](/assets/img/typescript/ts-js-to-ts-project5.png "enum 선언 후")

enum으로 선언하면 status를 사용하는 곳에 에러가 생긴다.  
왜냐하면 이전에는 string으로 status값을 전달했는데 enum으로 바꾸었으니 에러가 난다.  

![enum 선언 후 에러](/assets/img/typescript/ts-js-to-ts-project6.png "enum 선언 후 에러")

enum의 속성값으로 바꿔서 해결한다.

![enum 선언 후 에러 해결](/assets/img/typescript/ts-js-to-ts-project7.png "enum 선언 후 에러 해결")

enum으로 선언하고 난 뒤 좋은점은 해당 enum을 쓸 때 enum안에 있는 속성들이 자동완성이 되어 손 쉽게 칠 수 있고, 안에 들어있는 속성들을 모두 볼 수 있다.  

![enum 선언 후 자동완성](/assets/img/typescript/ts-js-to-ts-project8.png "enum 선언 후 자동완성")

<hr>

## DOM 함수 타입 <a name="section4"></a>

DOM 타입은 해당되는 HTML element를 html파일에서 해당 클라스를 찾고 태그종류에 따라 타입을 선언한다.  
예로 아래 DOM함수를 html에서 찾아본다. 

![HTMLelement태그 선언 전](/assets/img/typescript/ts-js-to-ts-project9.png "HTMLelement태그 선언 전")

위에 선언된 클라스들을 html파일에서 찾았다.

![html파일에서 클라스 찾기](/assets/img/typescript/ts-js-to-ts-project10.png "html파일에서 클라스 찾기")

각 태그에 맞게 타입을 단언 했다.

![HTMLelement태그 선언 후](/assets/img/typescript/ts-js-to-ts-project11.png "HTMLelement태그 선언 후")

<hr>

