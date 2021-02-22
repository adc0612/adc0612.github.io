---
layout: post
comments: true
title: "[Vue.js] 로그인 인증 토큰, HTTP헤더에 토큰삽입"
description: "로그인 인증 토큰, HTTP헤더에 토큰삽입"
subject: vue
categories: [ vue ]
tags: [ vue, token]
---

<hr>

## 인증 토큰

메인페이지에서 할 일 리스트를 조회하려면 유저의 권한이 필요하기 떄문에 로그인 토큰이 필요하다.  
로그인 인증 토큰은 로그인 성공시 반환하는 promise의 data안에 들어서 반환 된다.

![login api token](/assets/img/vue/vue-token3.png "login api token")

그 token을 받아서 할 일 리스트 조회할 때 같이 넘겨줘야한다.  
그래서 token을 받아 state의 token에 저장하고 http헤더에 실어서 보내야 한다.

<hr>

## axios headers authorization

`api/index.js`에서 instance 안에 headers를 정의 후 Authorization안에 token 값을 실어보내면 서버에 Token이 전달 가능하다.

Token이 전달되는지 확인을 위해 임시로 `test1234`를 넣었다.

`api/index.js`
```javascript
// axios post나 get 호출을 할 때 instance에 정의한것을 가지고 수행을한다.
const instance = axios.create({
  // baseURL: 'http://localhost:3000/',
  // 위에 주소를 .env파일에서 공통으로 지정
  baseURL: process.env.VUE_APP_API_URL,
  headers: {
    // authorization에 token값을 실어보내면 로그인권한 token값으로 이용 가능하다.
    Authorization: 'test1234',
  },
});
```

저장 후 다시 로그인하여 개발자 도구 네트워크 패널을 보면 Token값이 안에 들어있는 것을 볼 수 있다.

![login api token 개발자 도구에서 확인](/assets/img/vue/vue-token1.png "login api token 개발자 도구에서 확인")

<hr>

## store에 Token값 저장

일단 store에 Token값을 저장하기위해 state에 token변수 추가 후 mutations에서 token값을 넣을 수 있는 setToken 함수를 선언한다.

`store/index.js`
```javascript
import Vue from 'vue';
import Vuex from 'vuex';

// vuex 호출
Vue.use(Vuex);

// vuex의 instance들을 export
export default new Vuex.Store({
  // 여러 component들 사이에 공유되는 데이터
  state: {
    username: '',
    token: '',
  },
  // state에서 상태가 변경되었을때 계산하는 함수들
  getters: {
    // 만약 state.username이 비어있으면 false, 아니면 true반환
    // state.username으로 login된지 체크
    isLogin(state) {
      return state.username !== '';
    },
  },
  //   state를 바꾸는 역할
  mutations: {
    // state의 username를 넣을 함수
    setUsername(state, username) {
      state.username = username;
    },
    clearUsername(state) {
      state.username = '';
    },
    setToken(state, token) {
      state.token = token;
    },
  },
});
```

로그인이 처리 될 때 token값이 나오므로 `LoginForm.vue`에 로그인 성공시 값을 넣어주는 setToken을 등록한다.  
Token은 data.token에 저장되어있다.

`LoginForm.vue`
```javascript
methods: {
    async submitForm() {
      try {
        // 에러 없이 통과했을때 로직
        const userData = {
          username: this.username,
          password: this.password,
        };
        const { data } = await loginUser(userData);
        console.log(data.token);
        // store의 token의 데이터 token값을 저장
        this.$store.commit('setToken', data.token);
        // username을 store의 username에 저장
        this.$store.commit('setUsername', data.user.username);
        //main창으로 이동
        this.$router.push('/main');
        this.resetForm();
      } catch (error) {
        // 에러 생겼을때 로직
        console.log(error.response.data);
        this.logmsg = error.response.data;
      }
    },
```

저장 후 다시 로그인 후 개발자 도구에서 vuex를 보면 store의 token에 token값이 담긴것을 볼 수 있다.

![login api token vuex에서 확인](/assets/img/vue/vue-token2.png "login api token vuex에서 확인")

<hr>

## axios headers authorization에 token값 저장

이제 store의 token을 axios headers Authorization에 넣어줘야 한다.  
axios 호출 함수들이 등록되어 있는 `api/index.js`에서 `store/index.js`를 import해와 state의 token값을 넣는다.

```javascript
import axios from 'axios';
import store from '@/store/index';

// axios post나 get 호출을 할 때 instance에 정의한것을 가지고 수행을한다.
const instance = axios.create({
  baseURL: process.env.VUE_APP_API_URL,
  headers: {
    // authorization에 token값을 실어보내면 로그인권한 token값으로 이용 가능하다.
    Authorization: store.state.token,
  },
});
// 회원가입 data post
function registerUser(userData) {
  return instance.post('signup', userData);
}

// 로그인 기능 data post
function loginUser(userData) {
  return instance.post('login', userData);
}

```

## authorization에 토큰 값 missing

store의 token을 Authorization에 넣어서 보냈지만 보이지 않는다.

![login api token 개발자 도구에서 확인](/assets/img/vue/vue-token4.png "login api token 개발자 도구에서 확인")

<b style='color:#e64545;'>살펴보니 axios create는 login할 때 쓰는 loginUser함수를 호출하기 전보다 먼저 실행되어 Authorization에는 빈 값이 들어가서 보내졌다.<br/>
javascript는 값이 바뀌었다고 자동으로 조정해주지는 않는다.</b>

<hr>
