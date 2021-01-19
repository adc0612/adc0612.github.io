---
layout: post
comments: true
title: "vue NodeJS setting (NVM)"
description: "vue NodeJS setting (NVM)"
subject: vue
categories: [ vue ]
tags: [ vue, NVM, Nodejs]
---

<hr>

## NVM (Node Version Manager)

이 프로젝트에 쓰일 서버를 실행하려면 Node.js 버전을 10.16번대로 맞춰야 하므로,  
NVM (Node Version Manager)를 설치해서 사용하려고 한다.
NVM은 프로젝트마다 Node.js버전이 다양할 때, Node.js 버전을 쉽게 바꿀 수 있는 툴이다.

[https://github.com/nvm-sh/nvm#installing-and-updating](https://github.com/nvm-sh/nvm#installing-and-updating)

위 링크로 접속하면 NVM 설치에 관한 코드를 확인 할 수 있다.
bash cmd 창에서 아래 설치 command를 실행해서 설치한다.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```

bashrc파일도 수정해야 한다.
```bash
vi ~/.bashrc

export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

설치하면 nvm명령어로 설치 되있는지 확인한다.  
server에 쓰일 10.16.3버전을 설치한다.

```bash
nvm install 10.16.3 #node.js 10.16.3 version 설치
nvm use 10.16.3 #node.js 10.16.3 version 사용
```

<hr>

## NVM 설치 오류

위에 설명대로 설치해도 나는 nvm명령어가 실행이 안됐다.  
Google에서 많이 검색하여 조치도 해봤지만 무슨 오류인지 nvm명령어가 안 됐다.  
그래서 설치되어있던 node.js를 다 제거하고, bashrc파일에 썻던 내용 삭제 후,  `C:\Users\AppData\Roaming\npm`경로의 npm 관련 폴더를 삭제했다.  
npm폴더 삭제시 global로 설치했던 Package들은 모두 삭제되니 필요한 Package는 메모한 뒤 nvm설치 후 다시 재설치 해준다.

아래 경로로 접속해 nvm을 설치했다.  

[https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases)

`nvm-setup.zip`을 선택해 다운로드했다.  
압축푼 뒤 setup으로 설치해줬다.
설치하니 bashrc파일도 다 되있었고 nvm 작동이 잘 됐다.
