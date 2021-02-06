---
layout: post
comments: true
title: "로그인 상태에 따른 이동 링크처리 & 로그아웃"
description: "로그인 상태에 따른 이동 링크처리 & 로그아웃"
subject: vue
categories: [ vue ]
tags: [ vue, router, logout]
---

<hr>

## 로그인 상태에 따른 이동 링크 처리

AppHeader에 로고를 누르면 '/'로 가게 설정이 되어있는데, `/`는 로그인페이지를 가르키고있다.  
로그인 전에는 로그인페이지로 가고, 로그인 후에는 메인페이지로 넘어가게 바꿀 예정이다.

`components/common/AppHeader.vue`에서 로고에 있는 `to`를 v-bind로 computed된 값을 넣어줄 것이다.

`components/common/AppHeader.vue`
```html
      <router-link :to="logoLink" class="logo">
        TIL
        <span v-if="isUserLogin">by {{ $store.state.username }}</span>
      </router-link>
```

computed에 logoLink함수 선언 후 store.getters.inLogin이 true면 main으로 아니면 login 주소값을 넘겨준다.

`components/common/AppHeader.vue`
```javascript
    logoLink() {
      return this.$store.getters.isLogin ? '/main' : '/login';
    },
```
<hr>

## 로그아웃 처리

현재 AppHeader.vue에 로그아웃을 시켜주는 logoutUser methods가 있지만, store에 있는 username으로 지우고 로그인페이지로 전환해주는 것 밖에 구현이 안 되어 있다.  
store의 token값과 쿠키에 있는 username과 token까지 삭제하는 기능도 추가한다.

store의 token값을 없애는 clearToken함수를 `store/index.js`의 mutations에 선언한다.

`store/index.js`
```javascript
    clearToken(state) {
      state.token = '';
    },
```

`utils/cookies.js`에서 'deleteCookie'함수를 이용해 지워줄 것이다.

`utils/cookies.js`
```javascript
function deleteCookie(value) {
  document.cookie = `${value}=; expires=Thu, 01 Jan 1970 00:00:01 GMT;`;
}
```

deleteCookie함수는 인자로 저장했던 쿠키 키값을 받으면 그 값을 지워준다.  
그래서 `AppHeader.vue`의 logoutUser함수에서 저장했던 토큰과 유저네임 쿠키의 key를 전달해 지워준다.

`components/common/AppHeader.vue`
```javascript
    logoutUser() {
      // vuex mutations에 있는 clearUsername 함수 호출 store username값 초기화
      this.$store.commit('clearUsername');
      // vuex mutations에 있는 clearToken 함수 호출해서 store token값 초기화
      this.$store.commit('clearToken');
      //쿠키에서 username, token삭제
      deleteCookie('til_auth');
      deleteCookie('til_name');
      // loginPage로 전환
      this.$router.push('/login');
    },
```

<hr>

