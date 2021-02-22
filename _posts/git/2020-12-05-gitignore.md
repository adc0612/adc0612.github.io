---
layout: post
comments: true
title: "[git] Git ignore"
description: "Git ignore 설명"
subject: git
categories: [ git ]
tags: [ git, gitignore ]
---

<!-- <hr>

[1. .gitignore 설명](#.gitignore-설명)  
[2. .gitignore 실습](#.gitignore-실습)   -->

<hr>

## .gitignore 설명

git이 tracking하면 안되는 파일이나 폴더를 `.gitignore` 파일을 생성해 기록한다.

<hr>

## .gitignore 실습

현재 아래와 같이 폴더들과 파일들이 있다

![gitignore 실습 폴더 구조](/assets/img/git/git-ignore1.png "gitignore 실습 폴더 구조")

log1.log 파일은 git이 tracking 안 했으면 한다.
그럼 .gitigore 파일 생성 후 log1.log를 입력한다.

```bash
echo log1.log >> .gitignore
```

![gitignore 생성 및 편집 후 git status 확인](/assets/img/git/git-ignore2.png "gitignore 생성 및 편집 후 git status 확인")

`git add .`를 했는데 log1.log는 add가 안 된것을 볼 수 있다.

![gitignore 편집](/assets/img/git/git-ignore3.png "gitignore 편집")

파일명을 정확히 써서 하나의 파일만 쓸 수도 있고  
*.log 같이 *를 써서 모든 log파일들을 제외 할 수 있고  
build/ 같이 폴더명을 써서 build내 모든 파일을 제외 시킬 수 있다.  
`#은 주석이다`

![gitignore 편집 후 git status 확인](/assets/img/git/git-ignore4.png "gitignore 편집 후 git status 확인")

