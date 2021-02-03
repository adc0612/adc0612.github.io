---
layout: post
comments: true
title: "Post Add Page and function 추가"
description: "Post Add Page and function 추가"
subject: vue
categories: [ vue ]
tags: [ vue]
---

<hr>

## Post Add Page 이동 버튼 생성

MainPage.vue에 router-link를 추가 후 to는 "/add"로 바꿔준다.  

`MainPage.vue`
```html
<router-link to="/add" class="create-button">
    <i class="ion-md-add"></i>
</router-link>
```

아이콘은 아이오닉 아이콘으로 설정한다.  
pubilc/index.html에서 헤드에 링크를 추가해놨다.

`public/index.html`

```html
<link href="https://unpkg.com/ionicons@4.5.5/dist/css/ionicons.min.css" rel="stylesheet">
```

<hr>

## routes에서 POst Add Page 추가

routing목록을 작성하는 `routes/index.js`에서 PostAddPage.vue를 `/add`로 추가한다.

```javascript
{
    path: '/add',
    component: () => import('@/views/PostAddPage.vue'),
},
```
<hr>

## Post Add Page 생성

할 일을 추가 할 페이지는 PostAddPage.vue를 views폴더 안에 생성한다.  
Form 처리 관한것은 PostAddForm 컴포넌트를 만들어 진행 할 예정이다.

`views/PostAddPage.vue`
```vue
<template>
  <div class="form-container">
    <PostAddForm></PostAddForm>
  </div>
</template>

<script>
import PostAddForm from '@/components/posts/PostAddForm.vue';
export default {
  components: {
    PostAddForm,
  },
};
</script>

<style></style>
```

<hr>

## createPost 생성
api/posts.js에서 게시글 post 할 api 함수를 생성한다.  
createPost인자를 postData를 받아 post('posts',postData)로 만든다. export도 한다.

`api/posts.js`
```javascript
// 학습노트 데이터 생성 post API
function createPost(postData) {
  return post.post('/', postData);
}
```

<hr>

## Post Add Form 생성

PostAddPage의 form을 담당할 컴포넌트인 components/posts/PostAddForm을 생성한다.  
form을 넣고 div안에 title label과 input, contents label과 textarea를 생성한다.  
submit에 쓰일 button도 추가한다.  
그러고 PostAddPage안에 PostAddForm을 컴포넌트로 연결한다.  
PostAddForm에 data로 title과 contents를 만든 후 v-model로 각각 연결한다.  
submit button에는 v-on submit으로 submit을 동작 할 submitForm methods를 생성하여 연결한다.  

`components/posts/PostAddForm.vue`
```vue
<template>
  <div class="contents">
    <h1 class="page-header">Create Post</h1>
    <div class="form-wrapper">
      <form class="form" @submit.prevent="submitForm">
        <div>
          <label for="title">Title:</label>
          <input id="title" type="text" v-model="title" />
        </div>
        <div>
          <label for="contents">Contents:</label>
          <textarea id="contents" type="text" rows="5" v-model="contents" />
          <p v-if="!isContentsValid" class="validation-text warning">
            Contents must be less than 200
          </p>
        </div>
        <button type="submit" class="btn">Create</button>
        <p class="log">
          {{ logmsg }}
        </p>
      </form>
    </div>
  </div>
</template>

<script>
import { createPost } from '@/api/posts';
export default {
  data() {
    return {
      title: '',
      contents: '',
      logmsg: '',
    };
  },
  computed: {
    isContentsValid() {
      return this.contents.length <= 200;
    },
  },
  methods: {
    async submitForm() {
      try {
        const response = await createPost({
          title: this.title,
          contents: this.contents,
        });
        console.log(response);
      } catch (error) {
        console.log(error.response.data.message);
        this.logmsg = error.response.data.message;
      }
    },
  },
};
</script>

<style scoped>
.form-wrapper .form {
  width: 100%;
}
</style>
```

<hr>

## PostAddForm.vue에 createPost연결

PostAddForm에서 api/index.js의 createPost를 import한다.  
submitForm함수를 async를 붙이고 await붙여 createPost함수를 선언해서 response변수 안에 넣어준다.  
createPost인자는 객체로 title과 contents를 넣어 전달한다.  

<hr>

## await함수인 createPost의 에러 핸들링

try catch로 감싼다.
catch에서 에러 발생시 error.response.data.message안에 에러 설명이 있으므로,
그것을 logmsg로 넣어 화면에 표시해준다.

<hr>

## 글자 수 validation 삽입

PostAddForm.vue에 computed로 contents의 글자수가 200자 이내면 true고 아니면 false값을 return 하는 isContentsValid함수를 선언한다.
textarea밑에 p태그를 삽입 후 v-if속성을 걸어 isContentsValid함수가 false면 나타나게 설정한다.

<hr>
