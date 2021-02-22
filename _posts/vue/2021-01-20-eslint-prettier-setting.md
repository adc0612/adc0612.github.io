---
layout: post
comments: true
title: "[Vue.js] ESLint, Prettier 설정"
description: "[Vue.js] ESLint, Prettier 설정"
subject: vue
categories: [ vue ]
tags: [ vue, ESLint, Prettier]
---

<hr>

## ESLint Overlay 끄기 (vue.config.js)

Vue Cli 3.x 이후로는 ESLint에러가 화면에 overlay로 뒤덮이는 현상이 발생한다.  
에러를 해결하지 않으면 아무것도 할 수 없으므로 생산성을 위해 옵션을 꺼준다.  
프로젝트 최상단에서 `vue.config.js`라는 파일을 생성한다.  
아래 코드를 작성 후 저장하고 서버를 끄고(ctrl + c) 다시 키면(npm run serve) overlay기능이 꺼져있는것을 볼 수 있다.  
ESLint에러는 화면에는 안 나타나지만 terminal창에는 보인다.

{% highlight js %}
module.exports = {
    devServer: {
        overlay: false
    }
};
{% endhighlight %}

![vue.config.js파일 수정](/assets/img/vue/vue-create3.png "vue.config.js파일 수정")

<hr>

## ESLint

ESLint는 JavaScript 코드에서 발견 된 문제 패턴을 식별하기위한 정적 코드 분석 도구다. JavaScript에서 발생하는 에러의 가능성을 잡아주는 도구다.

[ESLint 공식 홈페이지](https://eslint.org/){: target="_blank"}

ESLint 설정은 프로젝트 최상단에 있는 `.eslint.js`파일에서 설정 가능하다.   

<b>`.eslint.js`파일 수정시 꼭 서버를 닫고 다시 켜줘야 한다. (설정파일들은 대부분 수정하면 서버를 닫고 켜야 적용이 된다.)</b>  

{% highlight js %}
module.exports = {
  root: true,
  env: {
    node: true
  },
  extends: ["plugin:vue/essential", "eslint:recommended", "@vue/prettier"],
  parserOptions: {
    parser: "babel-eslint"
  },
  rules: {
    "no-console": "off", //console.log()를 빨간줄 없이 사용 가능하게 함
    // "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
    // "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off"
  },
  overrides: [
    {
      files: [
        "**/__tests__/*.{j,t}s?(x)",
        "**/tests/unit/**/*.spec.{j,t}s?(x)"
      ],
      env: {
        jest: true
      }
    }
  ]
};
{% endhighlight %}

<hr>

## Prettier

Prettier는 코드 포맷터(Code Formatter)역할로 작성한 코드를 정해진 코딩 스타일을 따르도록 변환해주는 도구다.  
Indenting, 개행, 콤마 자동 찍기, 세미콜론 자동 찍기 등등

[Prettier 공식 홈페이지](https://prettier.io/){: target="_blank"}

prettier설정은 프로젝트 최상단에 있는 `.prettierrc`파일을 만들어 설정 가능하다.  

<b style="color:#e64545;">하지만 ESLint와 Prettier는 충돌이 날 수 있기 때문에 따로 설정하는 것보다 `.eslint.js` 파일안에서 ESLint 설정과 Prettier 설정을 같이 하는 것이 좋다.</b>

`.eslint.js`파일에 prettier 옵션 추가

{% highlight js %}
module.exports = {
  root: true,
  env: {
    node: true
  },
  extends: ["plugin:vue/essential", "eslint:recommended", "@vue/prettier"],
  parserOptions: {
    parser: "babel-eslint"
  },
  rules: {
    "no-console": "off", //console.log()를 빨간줄 없이 사용 가능하게 함
    // "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
    // "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off"

    // prettier옵션
    "prettier/prettier": ['error', {
      singleQuote: true, // 따옴표는 홑따옴표를 사용한다.
      semi: true,  // 모든 구문에 세미콜론을 붙인다.
      useTabs: false, // Tab을 사용 안 한다.
      tabWidth: 2, // 들여쓰기 할 때, 기본 폭을 설정한다.
      trailingComma: 'all', // 객체, 배열 등에서 항상 마지막에 , 를 붙인다.
      printWidth: 80, // 한 줄에서 wrap 이 되는 기준의 글자 수를 정한다.
      bracketSpacing: true, // 객체 리터럴의 괄호 사이에 공백을 출력
      arrowParens: 'avoid', //화살표 함수에서 단일 파라미터에 괄호를 붙일지("always") 말지("avoid")에 대한 여부를 결정한다.
    }]
  },
  overrides: [
    {
      files: [
        "**/__tests__/*.{j,t}s?(x)",
        "**/tests/unit/**/*.spec.{j,t}s?(x)"
      ],
      env: {
        jest: true
      }
    }
  ]
};
{% endhighlight %}

<hr>

## ESLint Plugin 설치

vscode extenstions에 들어가서 ESLint 검색 후 ESLint plugin을 설치한다.

![ESLint Plugin 설치](/assets/img/vue/vue-create4.png "ESLint Plugin 설치")

설치 후 ,를 제거하거나 에러를 생성하면 아래와 같이 <b style="color: red;">빨간색 밑줄</b>이 그어지고 에러설명이 뜨는 것을 볼 수 있다.

![ESLint Plugin 에러 확인](/assets/img/vue/vue-create5.png "ESLint Plugin 에러 확인")

<hr>

## ESLint Plugin 설정

`Windows: ctrl + ,` `mac: cmd + ,`을 치면 vscode setting에 들어갈 수 있다. 아니면 맨 위에 탭에 `File -> preferences -> setting`에 들어가면 된다.

![vscode 설정 위치](/assets/img/vue/vue-create6.png "vscode 설정 위치")

설정 안 검색창에 `eslint`라고 검색하면 밑에 `Eslint: Validate`를 찾을 수 있다.  바로 밑에 `settings.json`을 클릭한다. 

![vscode eslint validate](/assets/img/vue/vue-create7.png "vscode eslint validate")

`settings.json`파일에 밑에 eslint.validate관련된 문구를 추가하면 된다.

![vscode eslint validate settings.json 수정](/assets/img/vue/vue-create8.png "vscode eslint validate settings.json 수정")

<hr>

## Prettier Plugin 해제

ESLint안에 Prettier를 설정했으므로 만약 `Prettier Plugin`이 vscode에 설치 되어있으면 `사용 안 함(작업영역)`을 해주면 해당 프로젝트내에서는 vscode에서 제공하는 prettier plugin기능을 끌 수 있다.

![prettier plugin 해제](/assets/img/vue/vue-create9.png "prettier plugin 해제")

다시 `vscode setting`에 들어가서 `format on save`를 검색한다. `format on save`옵션을 체크 해제 해준다.

![format on save 해제](/assets/img/vue/vue-create10.png "format on save 해제")

`format on save`를 해제하는 이유는 만약 이 옵션이 되어있으면 vscode안에 있는 `beutify`나 다른 plugin이 파일 저장할 때 자동으로 code를 서식에 맞게 정리하므로 eslint안에 지정했던 `prettier`설정으로 되어있는지 확인이 어려워서다.

<hr>

## .eslintrc.js 파일에 따로 설정하는 이유

`.eslintrc.js`파일에 따로 설정하는 이유는 프로젝트를 다른 곳에서 `clone`해서 작업 할 때 eslint와 prettier설정이 IDE설정에 따라 바뀔 수 있으므로 따로 설정해주면 좋다.