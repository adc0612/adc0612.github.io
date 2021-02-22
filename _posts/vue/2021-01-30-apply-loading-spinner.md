---
layout: post
comments: true
title: "[Vue.js] Loading Spinner 등록"
description: "Loading Spinner 등록"
subject: vue
categories: [ vue ]
tags: [ vue, component, spinner]
---

<hr>

## LoadingSpinner.vue 생성

현재는 로컬서버에서 데이터를 불러와 시간이 짧게 걸린다.  
하지만 사이트가 구현이 되면 서버에서 데이터를 불러오는 시간이 걸릴 수도 있다.  
그래서 데이터 로딩 될 때 로딩상태를 나타내는 로딩스피너를 추가한다.  
`componets/common/`에 `LoadingSpinner.vue`를 생성한다.

`LoadingSpinner.vue`
```vue
<template>
  <div class="spinner-container">
    <div class="spinner" />
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.spinner-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 240px;
}
.spinner {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 5px solid #e0e0e0;
  border-bottom: 5px solid #fe9616;
  animation: spin 1s linear infinite;
  position: relative;
}
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
```

<hr>

## LoadingSpinner.vue 등록

`MainPage.vue`에서 `common/LoadingSpinner.vue`를 import 한 후 component에 저장한다.
로딩여부를 알려주는 isLoading을 data로 생성한다.  
노트 데이터를 불러오는 `await fetchPost();`바로 위에 isLoading에 true값을 준다.  
그리고 노트 데이터를 불러오면 false값을 주도록 바로 밑에 isLoading false값을 준다.  
`<ul></ul>`옆에 `<LoadingSpinner>`를 붙이고 v-if="isLoading"으로 해준다.  
`<ul></ul>`에는 v-else를 넣는다.  
isLoading이 true면 LoadingSpinner 컴포넌트 보여주고 false면 ListItem을 보여주는 것이다.  

`MainPage.vue`
```vue
<template>
  <div>
    <div class="main list-container contents">
      <h1 class="page-header">Today I Learned</h1>
      <LoadingSpinner v-if="isLoading"></LoadingSpinner>
      <ul v-else>
        <!-- PostListItem.vue에 props의 postItem을 postItem으로 내렸다. -->
        <PostListItem
          v-for="postItem in postItems"
          :key="postItem._id"
          :postItem="postItem"
        >
        </PostListItem>
      </ul>
    </div>
  </div>
</template>

<script>
import LoadingSpinner from '@/components/common/LoadingSpinner.vue';
import PostListItem from '@/components/posts/PostListItem.vue';
import { fetchPost } from '@/api/index';
export default {
  components: {
    PostListItem,
    LoadingSpinner,
  },
  data() {
    return {
      postItems: [],
      isLoading: false,
    };
  },
  methods: {
    async fetchData() {
      this.isLoading = true;
      const { data } = await fetchPost();
      this.isLoading = false;
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
<style></style>
```

<hr>
