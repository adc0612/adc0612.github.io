---
layout: post
comments: true
title: "API 분할"
description: "API 분할"
subject: vue
categories: [ vue ]
tags: [ vue, api]
---

<hr>

api 함수들이 대부분 api/index.js에 들어있는 관계로 기능에 따라 분리하려 한다.  

## instance 분리

지금의 로그인, 회원가입, 게시글 조회와 게시글 추가 함수들이 create instance 할 때 모두 인증값을 interceptor에 저장해 보낸다.  
그런데 로그인과 회원가입은 로그인 하기 전 기능으로 보낼 인증값이 없으므로 interceptor부분을 안 넣으려 한다.

그래서 인증값 없는 `createInstance()`함수와 인증값 있는 `createInstanceWithAuth`로 분리한다.  

`index.js`
```javascript
import axios from 'axios';
import { setInterceptors } from './common/interceptors';

// axios 초기화 함수
function createInstance() {
  return axios.create({
    baseURL: process.env.VUE_APP_API_URL,
  });
}

// interceptor 인증값 포함 함수
function createInstanceWithAuth(url) {
  const instance = axios.create({
    baseURL: `${process.env.VUE_APP_API_URL}${url}`,
  });
  return setInterceptors(instance);
}

export const instance = createInstance();
// 게시물 조회, 추가, 삭제는 posts url이 필요하므로 넣어준다.
export const post = createInstanceWithAuth('posts');
```

<hr>

## 로그인, 회원가입 API

로그인, 회원가입에 쓰일 API들은 `api/auth.js`로 분리해준다.  
import한 instance는 인증값이 없는 instance다.

`auth.js`
```javascript
// 로그인, 회원가입, 회원탈퇴 API
import { instance } from './index';

// 회원가입 data post API
function registerUser(userData) {
  return instance.post('signup', userData);
}

// 로그인 기능 data post API
function loginUser(userData) {
  return instance.post('login', userData);
}

export { registerUser, loginUser };
```

<hr>

## 게시물 추가, 조회, 삭제 API

게시물 추가, 조회, 삭제에 쓰일 API들은 `api/posts.js`에 분리해준다.
import한 posts는 인증값을 가지고 있는 instance다.

`posts.js`
```javascript
// 학습 노트 조작과 관련된 CRUD API 함수 파일
import { post } from './index';

// 학습노트 데이터 조회 API
function fetchPost() {
  return post.get('/');
}

// 학습노트 데이터 생성 post API
function createPost(postData) {
  return post.post('/', postData);
}

export { fetchPost, createPost };
```

이제 api 경로가 달라졌으므로 api쓰이는 파일마다 수정을 해준다.

<hr>
