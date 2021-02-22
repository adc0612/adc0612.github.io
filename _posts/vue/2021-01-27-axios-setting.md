---
layout: post
comments: true
title: "[Vue.js] axios 설정 및 설치"
description: "axios 설정 및 설치"
subject: vue
categories: [ vue ]
tags: [ vue, axios ]
---

<hr>

## axios 설명

axios는 HTTP 클라이언트 라이브러리로써, 비동기 방식으로 HTTP 데이터 요청을 실행한다.  
내부적으로 AXIOS는 직접적으로 XMLHttpRequest 를 다루지 않고 “AJAX 호출”을 할 수 있다.

ajax를 한마디로 정의하자면 JavaScript를 사용한 비동기 통신, 클라이언트와 서버간에 XML 데이터를 주고받는 기술이다.  
ajax는 html 페이지 전체가아닌 일부분만 갱신할수 있도록 XML HttpRequest객체를 통해 서버에 request를 한다.   
이 경우 Json이나 xml형태로 필요한 데이터만 받아 갱신하기 때문에 그만큼의 자원과 시간을 아낄 수 있습니다.

axios를 이용해 로그인이나 회원가입에 필요한 유저데이터를 받아오고 사용할 예정이다.

<hr>

## axios 설치

npm으로 간단하게 설치한다.

```bash
npm i axios
```

<hr>

## api 폴더 생성

axios로 post요청이나 get요청은 다른 파일에서도 사용할 예정으로 공통된 function으로 지정해 놓는게 좋다.

`src`밑에 `api`폴더를 생성하고 `index.js`파일을 만들어 axios요청을 할 api들을 만든다.

<hr>

## api 작성

회원가입에 쓰일 api를 작성한다.  
받은 userData를 axios를 이용해 server로 넘긴다.

```javascript
import axios from 'axios';

// 회원가입 data post
function registerUser(userData) {
    const url = 'http://localhost:3000/signup';
  // 첫번재 인자 보낼 URL
  // 두번재 인자 보낼 data
    return axios.post(url, userData);
}

// 필요한 파일에서 import 할 수 있게 export로 registerUser function을 내보낸다. 
export { registerUser };
```

서버주소 `http://localhost:3000/`는 대부분 하나의 URL을 지정한다.  
이러한 것은 공통분모로 사용 할 수 있게 한다.

axios.create를 이용해 기본 옵션들을 설정할 수 있다.

```javascript
import axios from 'axios';

// axios.crate를 이용해 기본 옵션들을 설정할 수 있다.
// 거의 백앤드 서버에 접속하는 URL은 하나로 지정해서 하기 떄문에 baseURL값으로 집어 넣는다.
// 그럼 post할 떄 axios.post(URL,data)대신 instance.post(data)로 post요청을 할 수 있다.
const instance = axios.create({
  baseURL: 'http://localhost:3000/',
});

// 회원가입 data post
function registerUser(userData) {
  //   const url = 'http://localhost:3000/signup';
  // 첫번재 인자 보낼 URL
  // 두번재 인자 보낼 data
  //   return axios.post(url, userData);
  // http://localhost:300/는 위에 선언해놓은 instance에서 바로 연결해주고 옆에 붙여줄 signup만 URL에 인자로 넣어주면 된다.
  return instance.post('signup', userData);
}

// 로그인 기능 data post
function loginUser(userData) {
  return instance.post('login', userData);
}

export { registerUser, loginUser };
```

<hr>

## .env 파일 설정

`.env`파일은 환경변수로 설정하는 파일로 `키=값`으로 설정해서 사용할 수 있다.  
`서버 URL`같은 프로젝트에서 많이 사용 될 공통된 값을 편하게 사용하기 위해서다.

프로젝트 폴더 최상단인 root에 .env파일을 만든다.  
vue-cli 3.x부터 변수에 `VUE_APP_`을 붙은 변수들은 자동으로 가져올 수 있다.

`.env`
```bash
VUE_APP_API_URL = http://localhost:3000/
```

`index.js`
```javascript
import axios from 'axios';

// axios.crate를 이용해 기본 옵션들을 설정할 수 있다.
// 거의 백앤드 서버에 접속하는 URL은 하나로 지정해서 하기 떄문에 baseURL값으로 집어 넣는다.
// 그럼 post할 떄 axios.post(URL,data)대신 instance.post(data)로 post요청을 할 수 있다.
const instance = axios.create({
  // baseURL: 'http://localhost:3000/',
  // 위에 주소를 .env파일에서 공통으로 지정
  baseURL: process.env.VUE_APP_API_URL,
});

// 회원가입 data post
function registerUser(userData) {
  //   const url = 'http://localhost:3000/signup';
  // 첫번재 인자 보낼 URL
  // 두번재 인자 보낼 data
  //   return axios.post(url, userData);
  // http://localhost:300/는 위에 선언해놓은 instance에서 바로 연결해주고 옆에 붙여줄 signup만 URL에 인자로 넣어주면 된다.
  return instance.post('signup', userData);
}

// 로그인 기능 data post
function loginUser(userData) {
  return instance.post('login', userData);
}

export { registerUser, loginUser };
```


<hr>

## .env 파일 종류

* .env: 개발할 때와 배포할 때 상관없이 공통으로 써야 될 환경변수들을 .env파일에 정의한다.  
같은 변수면 `.env.development`와 `.env.production`이 우선으로 되므로 덮어 씌여짐

* .env.development: 개발용(test: npm run serve)으로 local에서 테스트용으로 쓸 때 사용할 변수들을 지정하면 된다.  
.env보다 우선순위를 가진다.

* .env.production: 배포(production: npm run build)할 때 쓸 변수들을 저장.  
.env보다 우선순위를 가진다.


위와 같은 환경파일을 이용해 API_URL은 개발할 때 쓰는 주소와 배포 할 때 쓰는 주소가 다르므로 development, production각각 정의한다.

<hr>

