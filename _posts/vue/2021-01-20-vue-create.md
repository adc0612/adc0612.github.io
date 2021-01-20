---
layout: post
comments: true
title: "vue Project 생성 (vue CLI)"
description: "vue Project 생성 (vue CLI)"
subject: vue
categories: [ vue ]
tags: [ vue, vue CLI]
---

<hr>

## Vue CLI

Vue CLI는 Vue 프로젝트를 만들어주는 도구다.
아래주소는 Vue CLI 공식 페이지 주소다. 참고하면 좋다.

[https://cli.vuejs.org/guide/installation.html](https://cli.vuejs.org/guide/installation.html){: target="_blank"}

아래 명령어로 설치한다.

```bash
npm install -g @vue/cli
```

설치가 완료되면 `vue`라는 명령어가 인식이 된다.
`vue --version`으로 version확인도 가능하다.
`vue create 프로젝트명`으로 프로젝트를 생성한다.

```bash
vue create vue-til #프로젝트명 vue-til
```

preset 옵션은 아래와 같이 설정한다.

1. Manually select features 선택
2. Babel, Linter / Fromatter, Unit Testing 선택
3. Vue.js version 2.x 선택
4. ESLint + Prettier 선택
5. Lint on save
6. Jest
7. In dedicated config files
8. n

![vue create setting](/assets/img/vue/vue-create1.png "vue create setting")

만들어진 프로젝트 폴더로 이동 후 실행한다.

```bash
cd vue-til #프로젝트 폴더로 이동 명령어
npm run serve #프로젝트 실행 명령어
```

아래 화면에 쓰여있듯이 `npm run serve`는 `package.json`에 명시된 실행 명령어다.

![npm run serve 정의](/assets/img/vue/vue-create2.png "npm run serve 정의")

[Webpack Dev 서버 설명 사이트](https://joshua1988.github.io/webpack-guide/devtools/webpack-dev-server.html#webpack-dev-server){: target="_blank"}
