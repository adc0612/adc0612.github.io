---
layout: post
comments: true
title: "[Typescript] JS to TS Project - 1. 타입스크립트 프로젝트 환경 구성"
description: "[Typescript] JS to TS Project - 1. 타입스크립트 프로젝트 환경 구성"
subject: typescript
categories: [ typescript ]
tags: [ typescript]
---

<hr>


> 자바스크립트에서 타입스크립트로 바꾸는 배우는 테스트 프로젝트다.  
> 작업은 프론트엔드 프레임워크 (Vue, React 등)을 쓰지 않은 JS, html, css로만 이뤄진 프로젝트다.  
> 그래서 처음은 npm 으로 프로젝트 초기화와 타입스크립트도 설치를 해서 설정파일을 생성해서 작업한다.

[1. NPM 초기화](#section1)  
[2. 타입스크립트 라이브러리 설치](#section2)  
[3. 타입스크립트 설정 파일 생성](#section3)  
[4. 자바스크립트 파일을 타입스크립트 파일로 변환하기](#section4)  

<hr>

## NPM 초기화 (npm init-y) <a name="section1"></a>

프로젝트를 생성 후 프로젝트 root폴더에서 Terminal을 연 뒤 `npm init -y`입력하여 프로젝트를 초기화 한다.  
> 그냥 `npm init`을 입력하면 여러가지 질문에 답을 해야하므로 기본으로 빠르게 설치하기 위해 `npm init -y`를 입력한다.

```bash
npm init -y
```

입력 뒤 `package.json`파일이 생성된 것을 볼 수 있다.

<hr>

## 타입스크립트 라이브러리 설치 (npm i typescipt -D) <a name="section2"></a>

프로젝트 폴더에서 Terminal을 연 뒤`npm i typescript -D`로 타입스크립트 라이브러리를 설치한다.  
> 타입스크립트 라이브러리는 개발용(devDepencies) 라이브러리이므로 `-D`를 붙여서 설치한다.  
> 개발용(devDepencies) 라이브러리는 빌드하고 배포할 때 애플리케이션 코드에서 빠지게 된다.  

```bash
npm i typescript -D #축약형
```

설치 후 `package.json`파일 안에 `devDependencies`에 `typescript`가 추가된 것을 볼 수 있다.

<hr>

## 타입스크립트 설정 파일 생성 (tsconfig.json 파일 생성) <a name="section3"></a>

타입스크립트 설정을 지정할 파일을 생성한다.  
프로젝트 root 폴더에서 `tsconfig.json`파일을 생성한 후 필요한 옵션들을 입력한다.  

`tsconfig.json`
```json
{
    "compilerOptions": {
        "allowJs":true, // JS파일을 Typescript에서 인식해서 그냥 쓰게함
        "target": "ES5", // tsc로 compile후 TS파일을 JS파일로 변환될 때 원하는 JS버전
        "outDir": "./built", // tsc로 compile후 변환된 JS파일의 경로 지정
        "moduleResolution": "node", // 모듈 (검색)해석 방식 설정
        // 컴파일에 포함될 라이브러리 파일 목록
        "lib": ["ES2015", "DOM", "DOM.Iterable"] // ES5에서 Promise문법을 이해 못 하기 때문
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

> 와일드 카드 패턴  
> * : 해당 디렉토리의 모든 파일 검색  
> ? : 해당 디렉토리 안에 파일의 이름 중 한 글자라도 맞으면 해당  
> ** : 하위 디렉토리를 재귀적으로 접근(하위 디렉토리의 하위 디렉토리가 존재하는 경우 반복해서 접근)  

<hr>

## 자바스크립트 파일을 타입스크립트 파일로 변환하기 (tsc 명령어 설정) <a name="section4"></a>

js파일을 `src`폴더 안으로 옮기고 확장자를 `js`에서 `ts`로 변경했다. (에러가 엄청뜬다...)  
그리고 package.json에 편하게 compile하기위해 `npm run build`를 `tsc`로 설정했다.

`package.json`
```json
{
  "name": "project",
  "version": "1.0.0",
  "description": "최종 프로젝트 폴더입니다",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "tsc"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "typescript": "^4.2.2"
  }
}
```

<hr>

