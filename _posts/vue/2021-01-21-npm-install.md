---
layout: post
comments: true
title: "[Vue.js] npm install 명령어 정리"
description: "[Vue.js] npm install 명령어 정리"
subject: vue
categories: [ vue ]
tags: [ npm, node.js ]
---

<hr>

## NPM(Node Package Manager)  

NPM(Node Package Manager)은 명령어로 `javascript` 라이브러리를 설치하고 관리할 수 있는 `Package Mangaer`다. 
> 참고로 NPM은 Node.js 설치될 때 같이 설치 된다.
`npm install`명령어에는 지역(local) 설치와 전역(global) 설치 옵션이 있다.

<hr>

## NPM 지역 설치

npm 지역 설치는 옵션을 별도로 지정하지 않으면 지역으로 설치되며, 프로젝트 루트 디렉토리에 `node_modules`폴더가 자동 생성되고 그 안에 Package들이 설치된다.  
<b style='color:#e64545;'>지역으로 설치된 Package들은 해당 프로젝트 내에서만 사용 가능하다.</b>  
명령어는 아래와 같다.

```bash
npm install vue-router #--save-prod 생락
npm install vue-router --save-prod 
npm i vue-router #install를 i로 축약
npm i vue-router --save-prod
```

지역 설치 명령어의 경우 명령어 옵션으로 `--save-prod`를 붙이지 않아도 동일하다.  
또한, `install`대신 `i`를 사용해도 같다.  
그러므로 위 4개 명령어는 똑같은 결과를 나타낸다.

<hr>

### NPM 지역 설치 배포용 라이브러리

npm지역 설치를 할 때 해당 라이브러리가 배포용(dependencies)인지 개발용(devDepedencies)인지 꼭 구분해야한다.  

배포용(dependencies) 라이브러리는 `npm run build`로 빌드를 하면 최종 애플리케이션 코드 안에 포함된다.  

```bash
npm install vue-router --save-prod 
npm i vue-router #축약형
```

<hr>

### NPM 지역 설치 개발용 라이브러리

개발용(devDepencies) 라이브러리는 빌드하고 배포할 때 애플리케이션 코드에서 빠지게 된다.  

```bash
npm install eslint --save-dev
npm i eslint -D #축약형
```

<b style='color:#e64545;'>따라서, 최종 애플리케이션에 포함되어야 하는 라이브러리는 개발용 `-D`로 설치하면 안 된다.</b>

개발할 때만 사용하고 배포할 때는 빠져도 좋은 라이브러리 예시

{% highlight markdown %}
* `webpack` : 빌드 도구
* `eslint` : 코드 문법 검사 도구
* `imagemin` : 이미지 압축 도구
{% endhighlight %}

<hr>

## NPM 전역 설치

NPM 전역 설치는 위와 같이 프로젝트에서 사용할 라이브러를 불러올 때 사용하는 것이 아니라 시스템 레벨에서 사용할 javascript 라이브러리를 설치할 때 사용한다.

```bash
npm install gulp --global
npm i gulp -g #축약형
```

라이브러리 설치 후 명령어를 실행 창에 해당 라이브러리 이름을 입력했을 때 명령어를 인식한다.

```bash
gulp

```

<hr>

### NPM 전역 설치 경로

설치된 javascript 라이브러리는 어느 위치에서 해당 명령어를 실행했던지 간에 OS별로 아래와 같은 폴더 경로에 설치된다.

```bash
# window
%USERPROFILE%\AppData\Roaming\npm\node_modules

# mac
/usr/local/lib/node_modules
```