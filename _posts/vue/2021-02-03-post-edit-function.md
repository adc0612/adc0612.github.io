---
layout: post
comments: true
title: "[Vue.js] Post Edit function 추가"
description: "Post Edit function 추가"
subject: vue
categories: [ vue ]
tags: [ vue]
---

<hr>

## 게시글 수정 기능 추가

{% highlight markdown %}
1. 게시물들 밑에 각각 수정버튼 생성
2. 수정버튼에 routeEditPage methods 정의, 연결
3. routeEditPage에서 postItem id를 인자담어 postsEditPage로 넘기기
4. PostEditPage.vue 생성, PostEditForm 생성해서 components로 연결
5. routes/index.js 에서 dynamic route Matching을 이용해 path에 post/:postItem 연결
6. PostEditForm.vue 작성 (PostAddForm.vue에 있는 것 활용)
7. api/posts.js에 fetchPost함수로 아이디에 해당되는 게시글 가져오기
8. PostEditForm.vue 에서 페이지 실행될 때 바로 실행되는 created로 fetchPost import해서 게시글 가져오기
9. id는 router에서 넘겨지는 `this.$route.params._id`이용해서 fetchPost함수에 넘긴다.
10. fetchPost(id) 실행으로 받은 data의 title과 contents를 연결한다.
11. 게시물을 수정하는 editPost(id, postData) api를 정의한다. put(id, postData)로 데이터를 넘겨 덮어쓰게한다.
12. PostEditForm.vue에 methods로 submitForm 안에 editPost함수를 넣는다. 인자는 id와 contents data다.
{% endhighlight %}

<hr>

## 수정버튼 생성

각 게시물 안, 삭제 버튼 옆에 수정 버튼을 생성한다.

`components/posts/PostListItem.vue`
```html
    <div class="post-time">
      {{ postItem.createdAt }}
      <i class="icon ion-md-create" @click="routeEditPage"></i>
      <i class="icon ion-md-trash" @click="deleteItem"></i>
    </div>
```

<hr>

## dynamic route matching

dynamic route matching을 이용하여 route가 matching 될 때 `:`을 이용하여 값을 보낸다.  
methods에 routeEditPage() 함수를 선언한다.

`components/posts/PostListItem.vue`
```javascript
    routeEditPage() {
      this.$router.push(`/post/${this.postItem._id}`);
    },
```

<hr>

## routes에 post route 생성

`routes/index.js`에 `/post/`로 링크가 올 시 `PostEditPage.vue`로 연결시켜준다.

`routes/index.js`
```javascript
    {
      path: '/post/:id',
      component: () => import('@/views/PostEditPage.vue'),
    },
```

<hr>

## PostEditPage.vue와 PostEditForm.vue생성

PostEditPage.vue와 PostEditForm.vue를 생성하여 PostEditPage.vue에 컴포넌트로 PostEditForm.vue를 불러온다.  
PostEditForm.vue에 form에 관련된 로직을 쓴다.  
form은 PostAddForm.vue와 비슷한 form이므로 붙여서 넣는다.

<hr>

## fetchPost api 생성

`api/posts.js`에서 id에 따른 게시글 1개를 불러오는 api를 작성한다.

`api/post.js`
```javascript
// 학습노트 1개의 id에 매칭하는 데이터 조회 API
function fetchPost(postId) {
  return post.get(postId);
}
```

<hr>

## fetchPost api 연결

`components/posts/PostEditForm.vue`페이지가 로딩될 때 실행되게 created()안에 fetchpost함수를 실행한다.  
fetchPost함수 인자로 router에서 받은 id를 이용한다.  
fetchPost(id) 실행으로 받은 data의 title과 contents를 연결한다.

`components/posts/PostEditForm.vue`
```javascript
  async created() {
    try {
      // id는 router에 parameter로 넘긴 id다.
      const id = this.$route.params.id;
      const { data } = await fetchPost(id);
      console.log(data);
      this.title = data.title;
      this.contents = data.contents;
    } catch (error) {
      console.log(error);
    }
  },
```

<hr>

## editPost api 생성

게시물을 수정하고 수정을 서버로 전송하는 editPost api를 작성한다.  
인자로 게시물 id와 contents data를 받아 서버로 전송한다.

`api/posts.js`
```javascript
// 학습 노트 조작과 관련된 CRUD API 함수 파일
import { post } from './index';

// 학습노트 데이터 목록 조회 API
function fetchPostList() {
  return post.get('/');
}

// 학습노트 1개의 id에 매칭하는 데이터 조회 API
function fetchPost(postId) {
  return post.get(postId);
}

// 학습노트 데이터 생성 post API
function createPost(postData) {
  return post.post('/', postData);
}

// 학습노트 데이터 삭제 API
function deletePost(postId) {
  return post.delete(postId);
}

// 학습노트 1개의 id에 매칭하는 데이터 수정 API
function editPost(postId, postData) {
  return post.put(postId, postData);
}

export { fetchPostList, fetchPost, createPost, deletePost, editPost };

```

<hr>

## editPost api 연결

`components/posts/PostEditForm.vue` edit button을 누르면 실행되게 methods안에 submitForm 실행한다.  
submitForm함수 인자로 router에서 받은 id와 contents data를 이용한다.  

`components/posts/PostEditForm.vue`

```vue
<template>
  <div class="contents">
    <h1 class="page-header">Edit Post</h1>
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
        <button type="submit" class="btn">Edit</button>
        <p class="log">
          {{ logmsg }}
        </p>
      </form>
    </div>
  </div>
</template>

<script>
import { fetchPost, editPost } from '@/api/posts';
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
      const id = this.$route.params.id;
      try {
        const response = await editPost(id, {
          title: this.title,
          contents: this.contents,
        });
        // 게시글 성공으로 생성 후 MainPage로 전환
        this.$router.push('/main');
        console.log(response);
      } catch (error) {
        console.log(error.response.data.message);
        this.logmsg = error.response.data.message;
      }
    },
  },
  async created() {
    try {
      // id는 router에 parameter로 넘긴 id다.
      const id = this.$route.params.id;
      const { data } = await fetchPost(id);
      console.log(data);
      this.title = data.title;
      this.contents = data.contents;
    } catch (error) {
      console.log(error);
    }
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

