---
layout: post
comments: true
title: "Post Item 모듈화"
description: "Post Item 모듈화"
subject: vue
categories: [ vue ]
tags: [ vue, component, props]
---

<hr>

가져온 노트 데이터의 부분을 컴포넌트로 나누려고 한다.  
`components/`안에 `posts/`폴더 생성 후 `PostListItem.vue`를 만든다.

`<li>`부분을 가져온 후 `<template></template>`안에 넣는다.  
`<li v-for="postItem in postItems" :key="postItem._id">`여기서 postItems data와 data가져오는 함수는 없으므로 일단 지워주고 `<li>`만 놔둔다.  
`MainPage.vue`에서 props로 내려줄 예정이다.

`posts/PostListItem.vue`
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
    </div>
  </li>
</template>
```

`MainPage.vue`에서 PostListItem을 import하고 components에 등록한다.

```javascript
import PostListItem from '@/components/posts/PostListItem.vue';
export default {
  components: {
    PostListItem,
  },
};
```

`<ul></ul>`안에는 불러온`<PostListItem>`을 넣어주고 props로 postItem에 postItem을 넣어준다.

`MainPage.vue`
```vue
<template>
  <div>
    <div class="main list-container contents">
      <h1 class="page-header">Today I Learned</h1>
      <ul>
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
```

`PostListItem.vue`에서 props로 내린 postItem을 받아 props로 연결한다.

`PostListItem.vue`
```javascript
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
};
```

이렇게 연결하면 postItem이 잘 나타난다.