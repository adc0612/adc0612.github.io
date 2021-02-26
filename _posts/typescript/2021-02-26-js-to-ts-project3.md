---
layout: post
comments: true
title: "[Typescript] JS to TS Project - 3. 프로젝트 개발 환경 구성 (babel, eslint, prettier)"
description: "[Typescript] JS to TS Project - 3. 프로젝트 개발 환경 구성 (babel, eslint, prettier)"
subject: typescript
categories: [ typescript ]
tags: [ typescript, babel, eslint, prettier ]
---

<hr>

[1. babel, eslint, prettier 설치](#section1)  
[2. 바벨 (babel)](#section2)  
[3. Prettier](#section3)  
[4. ESLint](#section4)  

<hr>

## babel, eslint, prettier 설치 <a name="section1"></a>

아래 명령어로 babel, eslint, prettier를 설치한다.

```bash
npm i -D @babel/core @babel/preset-env @babel/preset-typescript @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint prettier eslint-plugin-prettier
```

설치 후 `package.json`에 `devDependencies`에서 보면 typescript와 설치한 목록들이 보인다.  
> babel, eslint, prettier는 typescript와 같이 배포할 때는 쓰이지 않고 개발할 때만 쓰이므로, `-D`가 붙여서 설치되고 `devDependencies`안에 들어있다.

![devDependencies에서 설치 파일들 확인](/assets/img/typescript/ts-js-to-ts-project12.png "devDependencies에서 설치 파일들 확인")

<hr>

## 바벨 (babel) <a name="section2"></a>

[babel 공식 사이트](https://babeljs.io/docs/en/){: target="_blank"}

최신 자바스크립트문법이 최대한 많은 브라우저내에서 작동 할 수 있게 변환해주는 `transcompiler`다.  
자바스크립트와 타입스크립트의 preset도 설치 했다.

![babel preset](/assets/img/typescript/ts-js-to-ts-project13.png "babel preset")

<hr>

## Prettier <a name="section3"></a>

[Prettier 공식 홈페이지](https://prettier.io/){: target="_blank"}

Prettier는 코드 포맷터(Code Formatter)역할로 작성한 코드를 정해진 코딩 스타일을 따르도록 변환해주는 도구다.  
Indenting, 개행, 콤마 자동 찍기, 세미콜론 자동 찍기 등등

prettier설정은 프로젝트 최상단에 있는 `.prettierrc`파일을 만들어 설정 가능하다.  

<b style="color:#e64545;">하지만 ESLint와 Prettier는 충돌이 날 수 있기 때문에 따로 설정하는 것보다 `.eslint.js` 파일안에서 ESLint 설정과 Prettier 설정을 같이 하는 것이 좋다.</b>

<hr>

## ESLint <a name="section4"></a>

### ESLint 설명

[ESLint 공식 홈페이지](https://eslint.org/){: target="_blank"}

ESLint는 ECMAScript 코드에서 문제점을 검사하고 일부는 더 나은 코드로 정정하는 `린트 도구` 중의 하나다.  
코드의 가독성을 높이고 잠재적인 오류와 버그를 제거해 단단한 코드를 만드는 것이 목적이다.

![eslint preset](/assets/img/typescript/ts-js-to-ts-project14.png "eslint preset")

타입스크립트의 린트 도구로 `TSLint`도 있지만 성능 때문인지 공식적으로 `ESLint`를 쓰라 권고된다. 

### ESLint와 Prettier 설정 파일 (.eslintrc.js)

프로젝트 root 레벨에서 `.eslintrc.js`파일을 만든 후 ESLint와 Prettier 설정을 입력한다.  
> .eslintrc.{js, json, yml }등이 있지만 js 선호한다. 왜냐하면 주석도 되고 다른파일과 병합도 쉽다.  
> `.eslintrc.js`파일에 따로 설정하는 이유는 프로젝트를 다른 곳에서 `clone`해서 작업 할 때 eslint와 prettier설정이 IDE설정에 따라 바뀔 수 있으므로 따로 설정해주면 좋다.

```javascript
// .eslintrc.js
module.exports = {
  root: true,
  // env는 사전 정의된 전역 변수 사용을 정의한다.
  // 당연한 얘기지만 browser, node 설정을 하지 않을경우 console,require 같은 사전 정의된 전역변수 환경에 있는 static 메서드를 인식할 수 없어서 에러가 발생한다.
  env: {
    browser: true,
    node: true,
  },
  // extends는 추가한 플러그인에서 사용할 규칙을 설정한다.
  // 플러그인은 일련의 규칙 집합이며, 플러그인을 추가하여도 규칙은 적용되지 않는다.
  // 규칙을 적용하기 위해서는 추가한 플러그인 중, 사용할 규칙을 추가해주어야 적용이 된다.
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/eslint-recommended',
    'plugin:@typescript-eslint/recommended',
  ],
  // Plugin 목록
  plugins: ['prettier', '@typescript-eslint'],
  // ESLint에는 프로젝트에서 사용하는 규칙을 수정할 수 있다.
  rules: {
    // Prettier 설정
    'prettier/prettier': [
      'error', // 밑에 내용들을 안 하면 error를 표시해준다.
      {
        singleQuote: true, // 따옴표는 홑따옴표를 사용한다.
        semi: true, // 모든 구문에 세미콜론을 붙인다.
        useTabs: false, // Tab을 사용 안 한다.
        tabWidth: 2, // 들여쓰기 할 때, 기본 폭을 설정한다.
        printWidth: 80, // 한 줄에서 wrap 이 되는 기준의 글자 수를 정한다.
        bracketSpacing: true, // 객체 리터럴의 괄호 사이에 공백을 출력
        arrowParens: 'avoid', // 화살표 함수에서 단일 파라미터에 괄호를 붙일지("always") 말지("avoid")에 대한 여부를 결정한다.
      },
    ],
  },
  // parserOptions은 ESLint 사용을 위해 지원하려는 Javascript 언어 옵션을 지정할 수 있습니다.
  parserOptions: {
    // Typescript 구문 분석을 위해 사용되는@typescript-eslint/parser
    parser: '@typescript-eslint/parser',
  },
};

```

### .eslintignore 파일

이 파일에는 eslint 적용하지 않을 폴더나 파일을 설정할 수 있다.  
프로젝트 root에 `.eslintignore`파일을 생성해서 eslint 적용하지 않을 폴더나 파일명을 넣어준다.

`eslintignore`
```bash
# eslint 적용 안 할 폴더 및 파일 설정
node_modules
```

### VSCode ESLint 플러그인 관련 설정

vscode extenstions에 들어가서 ESLint 검색 후 ESLint plugin을 설치한다.

![ESLint Plugin 설치](/assets/img/vue/vue-create4.png "ESLint Plugin 설치")

`ctrl + shift + p / cmd + shift + p`키를 이용하여 명령어 실행 창 표시한다.

실행 창에 `Open Settings (JSON)`입력 후 선택한다.

VSCode 사용자 정의 파일인 `settings.json`파일의 내용에 아래와 같이 ESLint 플러그인 관련 설정 추가한다. 

```json
    "eslint.validate": [
        "vue",
        "javascript",
        "javascriptreact",
        "typescript",
        "typescriptreact",
    ],
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "eslint.workingDirectories": [
        {
            "mode": "auto"
        }
    ],
```

![settings.json 수정](/assets/img/typescript/ts-js-to-ts-project15.png "settings.json 수정")

ESLint안에 Prettier를 설정했으므로 만약 `Prettier Plugin`이 vscode에 설치 되어있으면 `사용 안 함(작업영역)`을 해주면 해당 프로젝트내에서는 vscode에서 제공하는 prettier plugin기능을 끌 수 있다.

![prettier plugin 해제](/assets/img/vue/vue-create9.png "prettier plugin 해제")

다시 `vscode setting`에 들어가서 `format on save`를 검색한다. `format on save`옵션을 체크 해제 해준다.

![format on save 해제](/assets/img/vue/vue-create10.png "format on save 해제")

`format on save`를 해제하는 이유는 만약 이 옵션이 되어있으면 vscode안에 있는 `beutify`나 다른 plugin이 파일 저장할 때 자동으로 code를 서식에 맞게 정리하므로 eslint안에 지정했던 `prettier`설정으로 되어있는지 확인이 어려워서다.

<hr>

