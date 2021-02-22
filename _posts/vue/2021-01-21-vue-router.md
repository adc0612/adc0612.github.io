---
layout: post
comments: true
title: "[Vue.js] vue-router 설치 및 연결"
description: "vue-router 설치 및 연결"
subject: vue
categories: [ vue ]
tags: [ vue, vue router]
---

<hr>

[1. vue-router 설치](#vue-router-1)  
[2. vue-router 등록](#vue-router-2)  
[3. routes 생성](#vue-router-3)  
[4. router-link, router-view 추가](#vue-router-4)  
[5. 코드 분할 (code splitting)](#vue-router-5)  
[6. 초기 진입 페이지 설정 (routing redirect 설정)](#vue-router-6)  
[7. 없는 페이지로 접근할 때 라우터 처리 (routing callback 설정)](#vue-router-7)  
[8. history mode 설정](#vue-router-8)  

<hr>

## vue-router 설치 <a name="vue-router-1"></a>

화면 구성을 하기 위해 `npm i vue-router`로 vue router 설치를 한다.

```bash
npm i vue-router #vue-router 설치
```

<hr>

## vue-router 등록 <a name="vue-router-2"></a>

프로젝트 루트에 있는 `src/` 밑에 `routes`라는 폴더를 만들고 `index.js`파일을 만들어 router설정을 한다. 

`index.js`

```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';

// vue router 사용
Vue.use(VueRouter);

// router instance를 쓸 수 있게 export한다
export default new VueRouter();
```

`src/routes/index.js`파일에서 export 한 router는 `src/main.js`파일에 연결해줘야 한다.

`main.js`

```javascript
import Vue from 'vue';
import App from './App.vue';
import router from '@/routes/index';

Vue.config.productionTip = false;

new Vue({
  render: h => h(App),
  // routes/index.js 에서 router로 import해와 등록한다.
  router,
}).$mount('#app');
```

<hr>

## routes 생성 <a name="vue-router-3"></a>

`src/`폴더 안에 보이는 화면을 구성할 vue파일들을 넣을 `views`폴더를 생성한다.   
`LoginPage`와 `SignupPage`를 생성한다.

생성한뒤 `routes`에 포함 시켜준다.

`routes/index.js`
```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';
import LoginPage from '@/views/LoginPage.vue';
import SignupPage from '@/views/SignupPage.vue';

// vue router 사용
Vue.use(VueRouter);

// router instance를 쓸 수 있게 export한다
export default new VueRouter({
  routes: [
    {
      path: '/login',
      component: LoginPage,
    },
    {
      path: '/signup',
      component: SignupPage,
    },
  ],
});

```

<hr>

## router-link, router-view 추가 <a name="vue-router-4"></a>

`src/App.vue`에 해당 routes로 갈수 있는 `<router-link>`와 routes로 갔을 때 보여주는 `<router-view>`를 추가해야 한다.

```vue
<template>
  <div class="app">
    <router-link to="/login">Login</router-link>
    <router-link to="/signup">Signup</router-link>
    <div class="app-contents">
      <router-view></router-view>
    </div>
  </div>
</template>
```

<hr>

## 코드 분할 (code splitting) <a name="vue-router-5"></a>

SPA(Single Page Application)은 페이지가 바뀌어도 네트워크 패널을 보면 변화가 없다.  
일반적인 웹 페이지는 페이지를 이동할 때 마다 서버로 가서 해당 html을 받아와서 페이지 전환이 일어나는데,  
SPA는 윈도우 히스토리 API를 이용해 URL을 컨트롤 한다.

현재 이 프로젝트 페이지에서 네트워크 패널을 열고 `refresh`를 해보면 app.js의 response에 Login, Signup 내용들이 들어있는걸 확인할 수 있다.

![app.js response](/assets/img/vue/vue-router1.png "app.js response")

SPA(Single Page Application)는 초기 실행시에 필요한 웹 자원을 모두 다운 받는 특징이 있다.   
즉 사이트 접속시 페이지 20개면 20개 30개면 30개 다 처음 로딩 될 때 불러온다.  
코드 분할을 활용하게 되면 초기 로딩시에 모든 웹 자원을 다운받지 않고 필요한 시점에 다운 받아 성능 상의 이점이 생긴다.  
페이지가 많을 수록 오랜시간이 걸림으로 code Spliting을 이용해 Login Page에서는 Login만 Signup페이지에서는 Signup페이지만 불러오게 할 예정이다.  

routes/index.js에서 component부분을 최상단에서 import가 아닌 클릭 했을때 들고올 수 있게 아래처럼 만들어준다.

```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';

// vue router 사용
Vue.use(VueRouter);

// router instance를 쓸 수 있게 export한다
export default new VueRouter({
  routes: [
    {
      path: '/login',
      // component에 function으로 import로 들고오면 해당 페이지에 들어갈 때 해당파일을 자원을 들고올 수 있다.
      component: () => import('@/views/LoginPage.vue'),
    },
    {
      path: '/signup',
      component: () => import('@/views/SignupPage.vue'),
    },
  ],
});
```

위와 같은 방법을 지연된 로딩(Lazy Loading)이라고 합니다.   
이는 애플리케이션 규모가 커져 싱글 페이지 애플리케이션의 초기 화면 로딩 시간을 줄일 때 사용하는 방법이다다.   
왜냐면 화면이 10개인 웹 앱이 있는데 애플리케이션을 처음 시작했을 때 쓰지도 않을 나머지 화면 9개를 다 불러오는 것 보다는 특정 화면으로 이동할 때마다 해당 화면의 내용을 추가적으로 불러오는 것이 애플리케이션 로딩 속도 면에서 더 효율적이기 때문이다.  

<hr>

## 초기 진입 페이지 설정 (routing redirect 설정) <a name="vue-router-6"></a>

제작하는 애플리케이션 페이지를 `localhost:8080/` 아무런 인자 없이 초기 진입시 로그인 페이지가 안 뜬다.  
아래 코드를 입력하여 초기 진입 페이지를 LoginPage로 설정한다.

routes대괄호 안에 입력한다.

```javascript
    // 주소 default로 접속 할 때 바로 login 페이지로 연경
    {
      path: '/',
      redirect: '/login',
    },
```

<hr>

## 없는 페이지로 접근할 때 라우터 처리 (routing callback 설정) <a name="vue-router-7"></a>

routes에 지정해놓은 Login과 Signup이 아닌 다른 인자를 치고 접속하는 연결을 막아줘야 한다.  
`src/views`에 `NotFoundPage.vue`를 생성해서 없는 페이지를 접근할 때 보여줄 것이다.  

routes대괄호 안에 입력한다.

```javascript
    {
      // 위에 지정해놓은 페이지가 아닌 URL로 연결 되었을 때 보여줄 페이지
      path: '*',
      component: () => import('@/views/NotFoundPage.vue'),
    },
```

<hr>

## history mode 설정 <a name="vue-router-8"></a>

보통 router를 이용하면 URL값에 `# (Hash)`가 붙어있다. vue-router의 기본 설정은 hash 모드다.  
URL 해시를 사용하기 때문에 URL이 변경될 때 페이지가 다시 로드되지 않는다.

해시를 제거하기 위해 vue-router는 history 모드도 지원한다.  
history mode를 적용했을 때 바로 해당 주소를 직접 사용자가 입력하여 접속하려한다면 뜨게 되는 404 오류가 뜬다.

Vue에서는 기본적으로 url 직접 접근이 불가능하다. 그 이유는 vue는 싱글 페이지 클라이언트 앱이기 때문이다.   
Vue Router는 프론트의 요청에 따라 새로운 돔을 변경하는 것이 아닌, 브라우저에 변화가 있는 부분만 돔을 변경한다. 
그러니깐 실제적인 페이지에 서버가 할당되지 않았는데 해당링크를 직접 작성하여 접속하니 생기는 문제인것이다.  
그래서 vue는 기본적으로 포괄적으로 뜰 수 있는 404 오류페이지를 설정할 것을 권장한다. 

[Vue router History Mode 주의사항 문서](https://router.vuejs.org/guide/essentials/history-mode.html){: target="_blank"}

```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';

// vue router 사용
Vue.use(VueRouter);

// router instance를 쓸 수 있게 export한다
export default new VueRouter({
  // URL의 #을 없애기 위해
  mode: 'history',
  routes: [
    // 주소 default로 접속 할 때 바로 login 페이지로 연경
    {
      path: '/',
      redirect: '/login',
    },
    {
      path: '/login',
      // component에 function으로 import로 들고오면 해당 페이지에 들어갈 때 해당파일을 자원을 들고올 수 있다.
      component: () => import('@/views/LoginPage.vue'),
    },
    {
      path: '/signup',
      component: () => import('@/views/SignupPage.vue'),
    },
    {
      // 위에 지정해놓은 페이지가 아닌 URL로 연결 되었을 때 보여줄 페이지
      path: '*',
      component: () => import('@/views/NotFoundPage.vue'),
    },
  ],
});


```