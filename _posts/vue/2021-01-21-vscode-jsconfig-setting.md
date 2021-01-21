---
layout: post
comments: true
title: "파일을 절대 경로로 찾기 설정(jsconfig.json 설정)"
description: "vscode내 jsconfig.json설정"
subject: vue
categories: [ vue ]
tags: [ vue, vscode, jsconfig.json]
---

<hr>

## jsconfig.json 설정

vscode에서만 지정 할 수 있는 절대경로 설정이다.  
절대경로를 설정하지 않으면 매번 `../`, `../../`이렇게 상단에 올라가서 파일을 상대경로를 통해 찾아야 한다.  

프로젝트 최상단 폴더에 jsconfig.json파일을 생성한다.  
아래와 같은 코드를 입력하여 저장하면 `@/`를 이용해 프로젝트 최상단에 있는 `src/`폴더를 지정해서 사용할 수 있다.

```javascript
// vscode에서 제공하는 파일로 파일 절대경로 설정 
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "~/*": [
        "./*"
      ],
      "@/*": [ //@로 최상단에 src폴더로 인식
        "./src/*"
      ],
    }
  },
  "exclude": [ //제외 할 폴더 목록
    "node_modules",
    "dist"
  ]
}
```
