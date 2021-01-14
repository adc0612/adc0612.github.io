---
layout: post
comments: true
title: "Git commit"
description: "Git commit 설명"
subject: git
categories: [ git ]
tags: [ git, git commit ]
---

<hr>

## git commit

`git commit`은 파일들의 버전을 저장할 때 쓰인다.  
commit할 파일들을 먼저 `git add`를 이용하여 staging area로 옮긴다.

![git status and add 화면](/assets/img/git/git-commit1.png "git status and add 화면")

`git commit`을 입력하면 아래와 같은 화면이 뜨며 commit에 넣을 메세지를 작성 할 수 있다.

```bash
git commit
```

![git commit 화면](/assets/img/git/git-commit2.png "git commit 화면")

`git log`를 입력하면 commit이 된 것을 볼 수 있다.

![git log 화면](/assets/img/git/git-commit3.png "git log 화면")

## git commit -m

보통은 위와 같이 안 하고 아래와 같이 `-m` 옵션을 붙여서 사용한다.  
일단 commit을 하기위해 파일을 추가한 다음 `git add`로 staging area로 옮긷ㄴ다.

![git status and add 화면2](/assets/img/git/git-commit4.png "git status and add 화면2")

`git commit -m`을 입력하여 옆에 아래와 같이 메세지를 쓴다.

```bash
git commit -m "second commit"
```

![git commit 화면2](/assets/img/git/git-commit5.png "git commit 화면2")

`git log`를 입력하면 위에 쓴 메세지로 commit이 된 것을 볼 수 있다.

![git log 화면2](/assets/img/git/git-commit6.png "git log 화면2")

## git commit -am

한번 `git add`로 staging area에 올린 파일들은 수정하면 commit하기 전 따로 `git add`로 올릴 필요 없다.  

![git edit and status](/assets/img/git/git-commit7.png "git edit and status 화면")

```bash
git commit -am "third commit"
```

`git commit -am`을 쓰면 add와 동시에 commit이 완료 된다.

![git commit 화면3](/assets/img/git/git-commit8.png "git commit 화면3")


