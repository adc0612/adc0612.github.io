---
layout: post
comments: true
title: "vue MongoDB Cloud setting"
description: "vue MongoDB Cloud setting"
subject: vue
categories: [ vue ]
tags: [ vue, MongoDB, MongoDB Cloud]
---

<hr>

## MongoDB Cloud

회원의 정보를 저장할 데이터베이스로 MongoDB를 사용한다.  
MongoDB를 직접설치 하는 대신 MongoDB Cloud를 이용한다.  

[https://www.mongodb.com/cloud](https://www.mongodb.com/cloud)

회원가입 후 무료로 제공해주는 500MB만 사용한다.  

## Network Access

MongoDB Cloud랑 통신을 할 수 있게 우회경로를 설정해야 한다.  
SECURITY -> Network Access에 접속해서 허용 할 IP를 등록해야 한다.

![Add IP address](/assets/img/vue/mongodb1.png "Add IP address")

어차피 무료이고 테스트용으로 하는 것이라 어디서든 접근 가능하게 `Alllow Access From Anywhere`를 선택하고 Confirm했다.

![Add Whitelist Entry](/assets/img/vue/mongodb2.png "Add Whitelist Entry")

## Database Access

데이터베이스에 접속할 admin계정을 생성해야한다.  
SECURITY -> Database Access에 접속해서 ID를 생성해야 한다.

![Add New User](/assets/img/vue/mongodb3.png "Add New User")

`Read and write to any database`권한을 주고 test 계정을 생성한다.

![Add New User test](/assets/img/vue/mongodb4.png "Add New User test")

## Clusters

생성한 Test계정을 서버에 연결해줘야 한다.
ATLAS -> Clusters에 접속해서 CONNECT를 누른다.

![Clusters](/assets/img/vue/mongodb5.png "Clusters")

`Connect Your Application`을 누른다.

![Connect Your Application](/assets/img/vue/mongodb6.png "Connect Your Application")

Node.js 3.0 or later 선택 후 나오는 String값을 복사한다.

![Connection string](/assets/img/vue/mongodb7.png "Connection string")

## 서버에 연결

Server프로젝트에 src/app.js파일에 들어가 아래 내용을 위에 나온 String값으로 변경한다.
이때 `<password>`는 아까 셋팅했던 비밀번호로 한다.

![Server connecting](/assets/img/vue/mongodb8.png "Server connecting")

Server를 `npm run dev`로 실행 한 다음 `localhost:3000/api/docs`에서 api문서가 보이는지 확인한다.
