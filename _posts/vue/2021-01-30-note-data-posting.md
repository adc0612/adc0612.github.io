---
layout: post
comments: true
title: "Swagger API로 노트 데이터 생성과 조회"
description: "Swagger API로 노트 데이터 생성과 조회"
subject: vue
categories: [ vue ]
tags: [ vue, axios, Axios Interceptor]
---

<hr>

## Swagger Api 문서

Swagger Api (서버 localhost:3000포트에서 실행중)에서 노트 데이터 조회테스트를 위한 임시 노트 데이터를 만든다.

테스트할 ID의 인증 토큰을 `Authorize`버튼을 눌러 인증을 해준다.  
해줘야 해당 테스트할 유저에게 노트 데이터를 넣어 줄 수 있다.

![Swagger API Authorize1](/assets/img/vue/vue-data-posting1.png "Swagger API Authorize1")

값을 붙여 넣고 Authorize누르면 인증이 돼서 데이터를 집어넣을 수 있는 권한이 생긴다.

![Swagger API Authorize1](/assets/img/vue/vue-data-posting2.png "Swagger API Authorize1")

POST /posts (게시물을 한개 생성하는 API)에 `Try it out`을 눌러 임시로 title과 contents를 넣는다.

![Swagger API Post Try it Out 화면](/assets/img/vue/vue-data-posting5.png "Swagger API Post Try it Out 화면")

201Code로 Response가 아래와 같이 나오면 노트 게시물 생성 완료다.

![Swagger API Post Try it Out response 화면](/assets/img/vue/vue-data-posting3.png "Swagger API Post Try it Out response 화면")

<hr>

## axios로 노트데이터 불러오기 Function

axios를 이용하여 노트 데이터를 불러오는 Function을 선언한다.

`api/index.js`
```javascript
// 학습노트 데이터 조회 API
function fetchPost() {
  return instance.get('posts');
}
```

<hr>

## axios로 노트데이터 불러오기 Function 호출

`views/MainPage/vue`에서 fetchPost함수를 호출하여 데이터를 불러온다.  
불러온 후 배열에 담아 화면에 나타낸다.

`views/MainPage/vue`
```vue
<template>
  <div>
    <div class="main list-container contents">
      <h1 class="page-header">Today I Learned</h1>
      <ul>
        <li v-for="postItem in postItems" :key="postItem._id">
          <div class="post-title">
            {{ postItem.title }}
          </div>
          <div class="post-contents">
            {{ postItem.contents }}
          </div>
          <div class="post-time">
            {{ postItem.createdAt }}
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { fetchPost } from '@/api/index';
export default {
  data() {
    return {
      postItems: [],
    };
  },
  methods: {
    async fetchData() {
      const { data } = await fetchPost();
      console.log(data.posts);
      this.postItems = data.posts;
    },
  },
  // 페이지가 생성되자마자 호출할 함수
  created() {
    this.fetchData();
  },
};
</script>
```

화면에 아까 Swagger에서 생성한 노트 데이터를 볼 수 있다.

![note Data 화면](/assets/img/vue/vue-data-posting4.png "note Data 화면")

참고로 `postItem. 인자`들은 Swagger Api창에서 보는 인자들로 json이 구성되어있으므로 참고해서 보여주면 된다.

![Swagger API Post Try it Out response 화면 강조](/assets/img/vue/vue-data-posting6.png "Swagger API Post Try it Out response 화면 강조")


<hr>