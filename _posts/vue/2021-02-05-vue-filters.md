---
layout: post
comments: true
title: "[Vue.js] vue filters 적용"
description: "vue filters 적용"
subject: vue
categories: [ vue ]
tags: [ vue, filters]
---

<hr>

## vue filters

[vue filter 공식문서](https://vuejs.org/v2/guide/filters.html#ad){: target="_blank"}


Vue는 텍스트 형식화를 적용할 수 있는 필터를 지원한다.  
mustache 문법이나 v-bind때 쓸 수 있다.  

{% highlight html %}
{% raw %}
<!-- in mustaches -->
{{ message | capitalize }}

<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>
{% endraw %}
{% endhighlight %}
`filters`를 이용히여 게시글 밑에 생설날짜나 다른 곳에 쓰이는 날짜형식을 지정할 예정이다.

<hr>

## local filter (지역 필터)

지역 필터는 선언 한 컴포넌트 안에서만 적용이 된다.  
`components/posts/PostListItem.vue`에 날짜를 나타내므로 지역필터로 날짜 형식을 지정해본다.  

`components/posts/PostListItem.vue`
```javascript
  filters: {
    formatDate(value) {
      return new Date(value);
    },
  },
```

```html
 <div class="post-time">
      {{ postItem.createdAt | formatDate }}
      <i class="icon ion-md-create" @click="routeEditPage"></i>
      <i class="icon ion-md-trash" @click="deleteItem"></i>
    </div>
```

적용 전
![local filters 적용 전 화면](/assets/img/vue/vue-filters1.png "local filters 적용 전 화면")

적용 후
![local filters 적용 후 화면](/assets/img/vue/vue-filters2.png "local filters 적용 후 화면")

<hr>

## global filter (전역 필터)

`main.js`에 전역 필터를 선언하여 프로젝트 내 모든 소스에서 쓸 수 있도록 필터를 정의한다.  
날짜 형식은 한 형식으로 통일하려고 전역필터로 적용했다. 

`utils/filter.js`파일을 생성한다.

`utils/filter.js`
```javascript
export function formatDate(value) {
  const date = new Date(value);
  const year = date.getFullYear();
  //   month는 0부터 시작해서 +1를 해줘야한다.
  let month = date.getMonth() + 1;
  //   모든 날짜 형식에 숫자가 9보다 작으면 옆에 0을 붙여준다.
  month = month > 9 ? month : `0${month}`;
  let day = date.getDate();
  day = day > 9 ? day : `0${day}`;
  let hours = date.getHours();
  hours = hours > 9 ? hours : `0${hours}`;
  let minutes = date.getMinutes();
  minutes = minutes > 9 ? minutes : `0${minutes}`;
  return `${year}-${month}-${day} ${hours}:${minutes}`;
}

```

main.js에서 연결시켜준다.

`main/js`
```javascript
import Vue from 'vue';
import App from './App.vue';
import router from '@/routes/index';
import store from '@/store/index';
import { formatDate } from '@/utils/filters';

Vue.filter('formatDate', formatDate);
Vue.config.productionTip = false;

new Vue({
  render: h => h(App),
  // routes/index.js 에서 router로 import해와 등록한다.
  router,
  store,
}).$mount('#app');

```

이렇게 `main.js`에서 import해 불러와 사용할 수 있고, 바로 `main.js`에서 filter정의를 할 수 있다.  
하지만 `main.js`에서 로직이 많은것은 알아보기 힘드므로 분리해주는것이 좋다.

`main.js`
```javascript
import Vue from 'vue';
import App from './App.vue';
import router from '@/routes/index';
import store from '@/store/index';

Vue.filter('formatDate', value => {
  const date = new Date(value);
  const year = date.getFullYear();
  //   month는 0부터 시작해서 +1를 해줘야한다.
  let month = date.getMonth() + 1;
  //   모든 날짜 형식에 숫자가 9보다 작으면 옆에 0을 붙여준다.
  month = month > 9 ? month : `0${month}`;
  let day = date.getDate();
  day = day > 9 ? day : `0${day}`;
  let hours = date.getHours();
  hours = hours > 9 ? hours : `0${hours}`;
  let minutes = date.getMinutes();
  minutes = minutes > 9 ? minutes : `0${minutes}`;
  return `${year}-${month}-${day} ${hours}:${minutes}`;
});
Vue.config.productionTip = false;

new Vue({
  render: h => h(App),
  // routes/index.js 에서 router로 import해와 등록한다.
  router,
  store,
}).$mount('#app');
```

<hr>

