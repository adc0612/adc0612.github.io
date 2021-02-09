---
layout: post
comments: true
title: "template, v-if 와 v-else"
description: "template, v-if 와 v-else"
subject: vue
categories: [ vue ]
tags: [ vue, template, v-if, v-else ]
---

<hr>

`v-if` 디렉티브는 조건에 따라 블록을 렌더링하기 위해 사용된다.  
블록은 디렉티브의 표현식이 true 값을 반환할 때만 렌더링됩니다.

`v-else-if`는 이름에서 알 수 있듯, `v-if`에 대한 “else if 블록” 역할을 합니다. 또한 여러 개를 사용할 수 있습니다.

`v-else` 디렉티브를 사용하여 `v-if`에 대한 “else 블록”을 나타낼 수 있습니다

<b style='color:#e64545;'>단 `v-else` 엘리먼트는 `v-if` 엘리먼트 또는 `v-else-if` 엘리먼트 바로 뒤에 있어야 합니다. 그렇지 않으면 인식할 수 없습니다.</b>

보이지 않는 래퍼 역할을 하는 `<template>` 엘리먼트에 `v-if`를 사용할 수 있다.

`<template></template>`과 v-if v-else를 이용해 로그인이 되면 state의 username을 보여주고 로그인 전엔 login, signup만 보여줄 예정이다.

`store/index.js`에 getters를 이용해 로그인된지 체크하는 함수를 정의한다.  
로그인의 기준은 state의 username이 비어있으면 false, 아니면 true를 반환할 예정이다.

```javascript
  getters: {
    // 만약 state.username이 비어있으면 false, 아니면 true반환
    // state.username으로 login된지 체크
    isLogin(state) {
      return state.username !== '';
    },
  },
```

`AppHeader.vue`에 computed로 isUserLogin()함수를 선언하고 vuex에 선언했던 getters isLogin함수를 불러온다.

```javascript
  computed: {
    isUserLogin() {
      return this.$store.getters.isLogin;
    },
  },
```

isUserLogin값으로 template들에 있는 v-if와 v-else로 엮어준다.  

{% highlight html %}
{% raw %}
<div class="navigations">
    <!-- isUserLogin이 true면 실행되는 블럭  -->
    <template v-if="isUserLogin">
    <span>{{ $store.state.username }}</span>
    <!-- hyperlink이동기능 방지 -->
    <a href="javascript:;" @click="logoutUser">Logout</a>
    </template>
     <!-- isUserLogin이 false면 실행되는 블럭  -->
    <template v-else>
    <router-link to="/login">Login</router-link>
    <router-link to="/signup">Signup</router-link>
    </template>
</div>
{% endraw %}
{% endhighlight %}

<hr>
