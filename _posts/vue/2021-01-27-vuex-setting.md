---
layout: post
comments: true
title: "router-link 셋팅, vuex 설치 및 셋팅"
description: "router-link 셋팅, vuex 설치 및 셋팅"
subject: vue
categories: [ vue ]
tags: [ vue, vue-router, vuex ]
---

<hr>

[1. router-link](#vuex-setting-1)  
[2. vuex 사용 이유](#vuex-setting-2)  
[3. vuex 설명](#vuex-setting-3)  
[4. vuex 설치 및 연결](#vuex-setting-4)  
[5. vuex store에 저장 (mutations)](#vuex-setting-5)  
[6. vuex store값 접근 ($store.state.)](#vuex-setting-6)  
[7. getters 선언](#vuex-setting-7)  
[8. getters 이용](#vuex-setting-8)  

<hr>

## router-link <a name="vuex-setting-1"></a>

이전에 설치했던 `vue-router`를 이용해서 페이지 전환을 적용했다.  
일단 2가지로 이용해 페이지 전환을 할 수 있다. (페이지 이동 주소는 `routes/index.js`에 저장해놨다.)

{% highlight markdown %}

1. <router-link to="">
<router-link to=""> 는 HTML 상에서 이동하는 것이다.
 HTML <a>처럼 눌렀을때 접속을한다. 

```html
<router-link to="/login">Login</router-link>
<router-link to="/signup">Signup</router-link>
```

2. this.$router.push('')
this.$router.push('') 는 javascript 상에서 이동하는 것이다.
버튼눌렀을때 이동할 수 있게 함수로 구성할 수 있고, 로직이 통과되면 이동할 수 있게 구성할 수도 있다.

```javascript
this.$router.push('/main');
```
{% endhighlight %}

<hr>

## vuex 사용 이유 <a name="vuex-setting-2"></a>

App.vue 밑 LoginPage.vue 밑에 있는 LoginForm.vue에서 로그인 한 username을 App.vue 밑에 있는 AppHeader.vue에서 받아 쓰고 싶다.

3가지 옵션이 있다.

{% highlight markdown %}
1. event, props:
하위 컴포넌트인 LoginForm.vue에서 상위 컴포넌트인 LoginPage.vue로 event를 올리고  
다시 상위 컴포넌트인 App.vue에 올려 하위 컴포넌트인 AppHeader.vue로 props를 내려서 받을 수 있다.
2. eventbus:
eventbus를 활용해 컴포넌트 간에 값을 공유 할 수 있다.
3. vuex:
vuex의 store에 담아 컴포넌트 간에 공유될 데이터를 관리

{% endhighlight %}

![project 상태](/assets/img/vue/vue-vuex-setting1.png "project 상태")

프로젝트 규모가 클수록 컴포넌트 통신이 많아지기 때문에 vuex를 선택하는것이 관리가 편하다.

<hr>

## vuex 설명 <a name="vuex-setting-3"></a>

vuex 공식 사이트 설명
>Vuex는 Vue.js 애플리케이션에 대한 상태 관리 패턴 + 라이브러리 입니다. 애플리케이션의 모든 컴포넌트에 대한 중앙 집중식 저장소 역할을 하며 예측 가능한 방식으로 상태를 변경할 수 있습니다.

여러개 컴포넌트간의 공유할 데이터를 store 저장소에 보관하고 관리, 접근을 하는것이다.

<hr>

## vuex 설치 및 연결 <a name="vuex-setting-4"></a>

```bash
npm i vuex
```

위 명령어로 vuex를 설치한다.

`src/main.js`에 vuex관한 것을 바로 정의해서 쓸 수 있지만 `main.js`는 다른 내용도 많이 포함되므로, 따로 분리하는 것이 좋다.

`src/`밑에 `store`폴더를 만들고 `index.js`를 생성한다.

`index.js`

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
  },
  //   state를 바꾸는 역할
  mutations: {
    // state의 username를 넣을 함수
    setUsername(state, username) {
      state.username = username;
    },
  },
});

```

`state`에 username을 담을 `username`을 선언하고 `mutations`에 username을 넣을 function인 `setUsername`을 선언한다.  

`store/index.js`를 `main.js`에 등록해야 한다.

`main.js`

```javascript
import Vue from 'vue';
import App from './App.vue';
import router from '@/routes/index';
import store from '@/store/index';

Vue.config.productionTip = false;

new Vue({
  render: h => h(App),
  // routes/index.js 에서 router로 import해와 등록한다.
  router,
  // store/index.js 에서 vuex를 import해와 등록한다.
  store,
}).$mount('#app');
```

`npm run serve`로 서버를 돌려 사이트로 접속한다음 웹사이트 개발자 도구를 눌러 `Vue Devtools`에서 vuex창에 보면 state밑에 username변수의 상태를 볼 수 있다.

![vue devtools vuex 상태](/assets/img/vue/vue-vuex-setting2.png "vue devtools vuex 상태")

<hr>

## vuex store에 저장 (mutations) <a name="vuex-setting-5"></a>

`this.$store.commit('mutations 함수 이름', 전달할 데이터)`형식으로 선언해놓은 mutations를 통해 store값을 저장할 수 있다.

LoginForm.vue에서 로그인 성공 후 아래와 같이 store의 username에 저장한다.

```javascript
async submitForm() {
      try {
        // 에러 없이 통과했을때 로직
        const userData = {
          username: this.username,
          password: this.password,
        };
        const { data } = await loginUser(userData);
        console.log(data.user.username);
        // username을 store의 username에 저장
        this.$store.commit('setUsername', data.user.username);
        //main창으로 이동
        this.$router.push('/main');
        // this.logmsg = `${data.user.username} login completed`;
        // this.resetForm();
      } catch (error) {
        // 에러 생겼을때 로직
        console.log(error.response.data);
        this.logmsg = error.response.data;
      }
    },
```

사이트에서 로그인을 하면 vue devtools에 vuex창에 state의 username에 로그인 한 아이디가 담기는 것을 볼 수 있다.

![store username에 데이터 값이 담기는 화면](/assets/img/vue/vue-vuex-setting3.png "store username에 데이터 값이 담기는 화면")

## vuex store값 접근 ($store.state.) <a name="vuex-setting-6"></a>

`$store.state.store변수이름`형식으로 store값에 접근 할 수 있다.

AppHeader.vue에 로그인 성공시 username이 보이게 store에 있는 username을 넣었다.

```html
<template>
  <header>
    <div>
      <router-link to="/" class="logo">
        TIL
      </router-link>
    </div>
    <div class="navigations">
      <span>{{ $store.state.username }}</span>
      <router-link to="/login">Login</router-link>
      <router-link to="/signup">Signup</router-link>
    </div>
  </header>
</template>
```

store의 username을 header에서 볼 수 있다.

![store username appheader에 표시](/assets/img/vue/vue-vuex-setting4.png "store username appheader에 표시")

## getters 선언 <a name="vuex-setting-7"></a>

state에 있는 username이 빈 값일때는 login을 하기 전이고 username에 값이 있을때는 login을 한 것으로 가정 할 것이다.

`store/index.js`안에 getters에 isLogin함수를 선언해서 username값이 비어있으면 false를 리턴하고, username값이 있으면 true를 리턴한다.

```javascript
  // state에서 상태가 변경되었을때 계산하는 함수들
  getters: {
    // 만약 state.username이 비어있으면 false, 아니면 true반환
    // state.username으로 login된지 체크
    isLogin(state) {
      return state.username != '';
    },
  },
```

![getters 값 체크 화면](/assets/img/vue/vue-vuex-setting5.png "getters 값 체크 화면")

## getters 이용 <a name="vuex-setting-8"></a>

`$store.getters.getters함수이름`형식으로 getters값을 불러 올 수 있다.

하지만 computed속성에 값을 쉽게 불러 올 수 있게 지정해 놓을 수 있다.

```javascript
  computed: {
    isUserLogin() {
      return this.$store.getters.isLogin;
    },
  },
```

이렇게하면 바로 isUserLogin으로 getters값을 편하게 불러올 수 있다.

