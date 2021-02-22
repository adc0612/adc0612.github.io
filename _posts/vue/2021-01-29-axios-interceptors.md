---
layout: post
comments: true
title: "[Vue.js] Axios Interceptor"
description: "Axios Interceptor"
subject: vue
categories: [ vue ]
tags: [ vue, axios, Axios Interceptor]
---

<hr>

## axios interceptor

서버에서 요청이나 받는 응답을 처리하기 전에 추가 로직을 넣을 수 있는 것이 `axios interceptor`다.

[Axios Interceptor 공식 문서](https://github.com/axios/axios#interceptors){: target="_blank"}

서버 요청에 로직 추가 `axios.interceptors.request.use`와 서버 응답에 로직 추가 `axios.interceptors.response.use` 함수를 그대로 복사 해온다.

두 함수가 `axios`가 붙어 있는데 지금은 `instance`를 붙여서 axios를 사용중이므로 `instance`로 바꿔줘야 한다.

interceptor의 정의가 길어지기 때문에 분리를 해준다.  
`api/`폴더밑에 `common/`폴더 생성 후 `interceptor.js`를 만든다.

store를 `store/index.js`에서 import 해주고 `config.headers.Authorization` 안에 넣어준다.

`api/common/interceptor.js`
```javascript
import store from '@/store/index';

export function setInterceptors(instance) {
  // Add a request interceptor
  instance.interceptors.request.use(
    function(config) {
      // Do something before request is sent
      config.headers.Authorization = store.state.token;
      return config;
    },
    function(error) {
      // Do something with request error
      return Promise.reject(error);
    },
  );

  // Add a response interceptor
  instance.interceptors.response.use(
    function(response) {
      // Any status code that lie within the range of 2xx cause this function to trigger
      // Do something with response data
      return response;
    },
    function(error) {
      // Any status codes that falls outside the range of 2xx cause this function to trigger
      // Do something with response error
      return Promise.reject(error);
    },
  );
  return instance;
}
```

`api/index.js`에 돌아와 코드를 수정한다.  
`store/index.js`에서 import 해주고 `config.headers.Authorization` 안에 넣어줬으므로, 임시로 붙여둔 headers의 Authorization을 지워준다.

`createInstance()`함수를 만들고 기존 instance 선언한 것을 넣어주고 export한 setInterceptors() 인자로 instance를 넣고 return한다.  
그리고 새로운 instance를 만들어 `createInstance()`함수랑 연결한다.

```javascript
import axios from 'axios';
import { setInterceptors } from './common/interceptors';

function createInstance() {
  // axios.crate를 이용해 기본 옵션들을 설정할 수 있다.
  // 거의 백앤드 서버에 접속하는 URL은 하나로 지정해서 하기 떄문에 baseURL값으로 집어 넣는다.
  // 그럼 post할 떄 axios.post(URL,data)대신 instance.post(data)로 post요청을 할 수 있다.
  // axios post나 get 호출을 할 때 instance에 정의한것을 가지고 수행을한다.
  const instance = axios.create({
    // baseURL: 'http://localhost:3000/',
    // 위에 주소를 .env파일에서 공통으로 지정
    baseURL: process.env.VUE_APP_API_URL,
  });
  return setInterceptors(instance);
}

const instance = createInstance();

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

저장 후 로그인 성공하면 log창에 config를 보면 Authorization에 인증토큰이 들어 있는 것을 볼 수 있다.

![Authorization token 개발자 도구에서 확인](/assets/img/vue/vue-token5.png "Authorization token 개발자 도구에서 확인")

<hr>