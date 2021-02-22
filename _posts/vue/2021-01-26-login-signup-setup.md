---
layout: post
comments: true
title: "[Vue.js] login, signup page 만들기"
description: "login, signup page 만들기"
subject: vue
categories: [ vue ]
tags: [ vue ]
---

<hr>

## common component header 생성

page마다 동일한 component들은 공통의 component로 만든 후 page마다 `components`로 등록해서 쓰는 것이 코드가 간결하다.

`src/components`폴더 안에 `common`폴더를 만든 뒤 `AppHeader.vue`를 넣는다.

`AppHeader.vue`안 에는 page로 이동해주는 link를 넣어놨다.

```vue
<template>
  <header>
    <div>
      <router-link to="/" class="logo">
        TIL
      </router-link>
    </div>
    <div class="navigations">
      <router-link to="/login">Login</router-link>
      <router-link to="/signup">Signup</router-link>
    </div>
  </header>
</template>
```

`App.vue`안에 등록해줬다.

```vue
<template>
  <div class="app">
    <AppHeader></AppHeader>
    <div class="app-contents">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import AppHeader from '@/components/common/AppHeader.vue';

export default {
  components: {
    AppHeader,
  },
};
</script>
```

<hr>

## LoginPage SignupPage 생성
 
`src/views`는 page들만 보여주는 vue파일로 로직이 많이 연계 안 되도록 로직이 많은 component들을 만들어서 연결하는 방식으로 했다.

![vue folder structure](/assets/img/vue/vue-file-structure1.png "vue folder structure")

`views/LoginPage.vue`안에는 Login Page 기능을 등록할 component를 `common/LoginForm.vue`만들어 등록했다.

`views/SignupPage.vue`안에는 Signup Page 기능을 등록할 component를 `common/SignupForm.vue`만들어 등록했다.

