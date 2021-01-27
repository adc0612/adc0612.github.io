---
layout: post
comments: true
title: "api통해 axios 데이터처리, email validation"
description: "api통해 axios 데이터처리, email validation"
subject: vue
categories: [ vue ]
tags: [ vue, axios ]
---

<hr>

## SignupForm.vue 수정

`api/index.js`로 지정해놓은 api를 써서 signupform에서 입력한 userData를 보낼 예정이다.

```vue
<template>
  <div class="contents">
    <div class="form-wrapper form-wrapper-sm">
    <!-- v-on directive를 이용해 폼의 submit 이벤트를 바인딩
      submit시 페이지 갱신을 막는 prevent
      v-on:submit == @submit -->
      <form @submit.prevent="submitForm" class="form">
        <div>
          <label for="username">id: </label>
          <input id="username" type="text" v-model="username" />
        </div>
        <div>
          <label for="password">pw: </label>
          <input id="password" type="text" v-model="password" />
        </div>
        <div>
          <label for="nickname">nickname: </label>
          <input id="nickname" type="text" v-model="nickname" />
        </div>
        <!-- email validation 통과, password와 nickname이 있어야 submit버튼 활성화 -->
        <button
          :disabled="!isUsernameValid || !password || nickname"
          type="submit"
          class="btn"
        >
          Submit
        </button>
        <p class="log">{{ logmsg }}</p>
      </form>
    </div>
  </div>
</template>
```

`form`을 등록해서 submit할 때 `submitForm` function이 작동되게 한다.

```vue
<script>
import { registerUser } from '@/api/index';
export default {
  data() {
    return {
      // form values
      username: '',
      password: '',
      nickname: '',
      // log msg
      logmsg: '',
    };
  },
  methods: {
    submitForm() {
      const userData = {
        username: this.username,
        password: this.password,
        nickname: this.nickname,
      };
      // promise 기법
      const { data } = registerUser(userData)
        .then()
        .catch(error => console.log(error));
      this.logmsg = `${data.username} signup completed`;
      this.resetForm();
      console.log('Form submit');
      console.log(data.username);
    },
    resetForm() {
      this.username = '';
      this.password = '';
      this.nickname = '';
    },
  },
};
</script>
```

<hr>

## promise기법 대신 async & await

axios로 post하는 `registerUser` api function은 promise를 return한다.  
그래서 registerUser(userData)로 data보내고 나온 promise를 `then, catch`로 받아야 한다.  

```javascript
const { data } = registerUser(userData)
        .then()
        .catch(error => console.log(error));
```

`ES8`도입된 `async & await`를 쓰면 비동기 코드를 동기적으로 깔끔하게 나타낼 수 있다.  
function이름 옆에 async붙이고 그 안에 호출할 비동기 function앞에 await를 붙여 주면 된다.  

```javascript
async submitForm() {
      try {
        // 에러 없이 통과했을때 로직
        const userData = {
          username: this.username,
          password: this.password,
          nickname: this.nickname,
        };
        // promise 기법
        // const { data } = registerUser(userData)
        //   .then()
        //   .catch(error => console.log(error));
        const { data } = await registerUser(userData);
        this.logmsg = `${data.username} signup completed`;
        this.resetForm();
        console.log('Form submit');
        console.log(data.username);
      } catch (error) {
          //에러 났을때 로직
        console.log(error.response.data);
      }
    },
```

에러가 났을때를 대비해 try, catch로 묶는다.

<hr>

## email validation

id를 email형식으로 받을 것이므로 간단하게 email형식으로 입력했는지 validation처리를 한다.  
javascript email validation 치면 해당하는 regular expression이 나온다.

email validation도 공통적으로 쓰일 것이므로 `src/`밑에 `utils`폴더 생성후 `validation.js`를 생성한다.

```javascript
// email validation
function validateEmail(email) {
  const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
  return re.test(String(email).toLowerCase());
}

export { validateEmail };
```

`signupForm.vue`에서 import 해주고 computed에 넣는다. 

```javascript
import { validateEmail } from '@/utils/validation';
  computed: {
    // username이 eamil형식인지 validation
    // email textfield에 글자 하나 칠 때마다 validation검사가 됨
    isUsernameValid() {
      return validateEmail(this.username);
    },
  },

```

<hr>

## watch vs computed

* watch: watch 속성은 감시할 데이터를 지정하고 그 데이터가 바뀌면 이런 함수를 실행하라는 방식으로 소프트웨어 공학에서 이야기하는 ‘명령형 프로그래밍’ 방식.

* computed: computed는 데이터의 변경에 반응하여 특정 값을 반환해주는 일종의 getter 함수이다.

```html
<div id="demo">{{ fullName }}</div>
```

```javascript
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```

```javascript
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```

<hr>