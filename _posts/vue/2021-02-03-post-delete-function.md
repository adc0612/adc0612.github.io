---
layout: post
comments: true
title: "[Vue.js] Post delete 기능 추가"
description: "Post delete 기능 추가"
subject: vue
categories: [ vue ]
tags: [ vue]
---

<hr>

## Post delete 버튼 생성

`components/posts/PostListItem.vue`에 시간표시해주는 div안 밑에 삭제 icon을 추가한다.  
삭제 icon을 클릭했을 때 실행되는 deleteItem함수를 생성해서 v-on click으로 연결한다

`components/posts/PostListItem.vue`
```vue
<template>
  <li>
    <div class="post-title">
      {{ postItem.title }}
    </div>
    <div class="post-contents">
      {{ postItem.contents }}
    </div>
    <div class="post-time">
      {{ postItem.createdAt }}
      <i class="icon ion-md-create"></i>
      <i class="icon ion-md-trash" @click="deleteItem"></i>
    </div>
  </li>
</template>

<script>
export default {
  // MainPage.vue에서 props값을 내린 것을 받아준다.
  props: {
    postItem: {
      // 받는 형식은 Object이고
      type: Object,
      // 이 값은 필수값이라 지정
      required: true,
    },
  },
  methods: {
    deleteItem() {},
  },
};
</script>

<style></style>

```

<hr>

## Post delete API 확인

Swagger에 있는 delete API 문서를 보면 게시글을 지울 때 필요한 것은 게시글이 가지고 있는 고유의 아이디다.

![Swagger delete api](/assets/img/vue/vue-post-delete1.png "Swagger delete api")

게시글의 id는 각 PostListItem의 `_id`에 값이 들어있는 것을 확인할 수 있다.  
그 값을 전달해서 지워야 한다.

![PostListItem id 확인](/assets/img/vue/vue-post-delete2.png "PostListItem id 확인")

<hr>

## delete api 작성

`api/posts.js`에서 delete api인 `deletePost`를 작성한다. 작성하고 export 한다.

`api/posts.js`
```javascript
// 학습노트 데이터 삭제 API
function deletePost(postId) {
  return post.delete(postId);
}
```

<hr>

## import delete api

`components/posts/PostListItem.vue`에 deleteItem methods안에 deletePost api 함수를 넣는다.  
인자로 postItem의 id가 들어가야 하므로 `this.postItem._id`를 넣어준다.

`components/posts/PostListItem.vue`
```javascript
async deleteItem() {
    try {
    await deletePost(this.postItem._id);
    console.log('deleted');
    } catch (error) {
    console.log(error);
    }
},
```

methods가 잘 작동되는 것을 확인 할 수 있다.  
하지만 post delete이후에 화면이 새로고침이 되지 않아 post가 잘 지워졌는지는 화면을 새로고침해야 볼 수 있다.

<hr>

## 삭제 후 새로고침

post가 삭제 된 후 화면에서는 새로고침을 해야 확인이 되므로 삭제 후 post들만 다시 갱신 될 수 있게 기능을 추가한다.  

`components/posts/PostListItem.vue`에서 삭제함수가 성공되면 상위 컴포넌트인 `views/MainPage.vue`에 `refresh` 이벤트를 보낸다.

`components/posts/PostListItem.vue`
```javascript
    async deleteItem() {
      try {
        await deletePost(this.postItem._id);
        // MainPage.vue에 refresh이벤트 전달
        this.$emit('refresh');
        console.log('deleted');
      } catch (error) {
        console.log(error);
      }
    },
```

`views/MainPage.vue`에 가서 refresh 이벤트가 오면 데이터를 불러오는 `fetchData`함수를 넣어 실행이 되게 한다.

`views/MainPage.vue`
```html
<PostListItem
    v-for="postItem in postItems"
    :key="postItem._id"
    :postItem="postItem"
    @refresh="fetchData"
>
</PostListItem>
```

<hr>

## 삭제 확인하는 창 띄우기

삭제 버튼을 잘 못 누를수도 있으므로 다시 한번 확인하는 확인 창을 띄운다.  
javascript confirm 함수를 이용한다.  
confirm 함수는 매개변수로 받은 question(질문)과 확인 및 취소 버튼이 있는 모달 창을 보여준다.  
사용자가 확인버튼를 누르면 true, 그 외의 경우는 false를 반환합니다.  
true일 때만 삭제 기능이 될 수 있게 수정한다.  

`components/posts/PostListItem.vue`
```javascript
    async deleteItem() {
      try {
        if (confirm('You want to delete it?')) {
          await deletePost(this.postItem._id);
          // MainPage.vue에 refresh이벤트 전달
          this.$emit('refresh');
        }
        // console.log('deleted');
      } catch (error) {
        console.log(error);
      }
    },
```
<hr>


