---
layout: post
comments: true
title: "router navigation guard 적용"
description: "router navigation guard 적용"
subject: vue
categories: [ vue ]
tags: [ vue, router, navigation guard]
---

<hr>

## router navigation guard

[router naviagion guard 공식 문서](https://router.vuejs.org/guide/advanced/navigation-guards.html){: target="_blank"}

`vue-router`가 제공하는 `navigation guards`는 주로 리디렉션하거나 취소하여 네비게이션을 보호하는 데 사용한다.  
흔히 login 후 접속할 수 있는 페이지를 주소만으로 접속할 때 막아줄때 사용된다.

## router navigation guard 적용

먼저 `routes/index.js`에 VueRouter를 export default로 바로 내보내지말고 router 객체에 담아준다.  
beforeEach문에 navigation guard를 적용하기 위해서다.  

routes에 path마다 부가적인 정보를 넣을 수 있는 meta를 작성한다.  
meta안에 auth를 넣어 로그인 인증이 필요한 페이지들은 true 값을 넣는다. (메인페이지, 추가페이지, 수정페이지)

로그인이 상태를 `store.getters.isLogin`함수로 가져온다음 인증이 필요한 페이지에 로그인이 안된 상태이면 로그인 페이지로 넘겨지게 한다.

`routes/index.js`
```javascript
import Vue from 'vue';
import VueRouter from 'vue-router';
import store from '@/store/index';

// vue router 사용
Vue.use(VueRouter);

// router instance를 쓸 수 있게 export한다
const router = new VueRouter({
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
      path: '/main',
      component: () => import('@/views/MainPage.vue'),
      meta: { auth: true },
    },
    {
      path: '/add',
      component: () => import('@/views/PostAddPage.vue'),
      meta: { auth: true },
    },
    {
      path: '/post/:id',
      component: () => import('@/views/PostEditPage.vue'),
      meta: { auth: true },
    },
    {
      // 위에 지정해놓은 페이지가 아닌 URL로 연결 되었을 때 보여줄 페이지
      path: '*',
      component: () => import('@/views/NotFoundPage.vue'),
    },
  ],
});

// to: 이동하려는 페이지 (link클릭한 주소)
// from: 현재 페이지
// next: 페이지 이동할 때 호출하는 API
router.beforeEach((to, from, next) => {
  if (to.meta.auth && !store.getters.isLogin) {
    console.log(`인증이 필요합니다`);
    next('/login');
    // return을 써주는 이유는 if문타고 왔을때 if문 밖에 있는 next가 실행이 안 되게 하기위해서
    return;
  }
  next();
});

export default router;

```

<hr>

