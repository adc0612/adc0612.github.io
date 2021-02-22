---
layout: post
comments: true
title: "[git] Git status"
description: "Git status 설명"
subject: git
categories: [ git ]
tags: [ git, git status ]
---

<hr>

## git status 설명

git status를 통해 git의 상태를 체크한다.

<hr>

## git status (git status --long)

a2문구를 a.txt에 추가하고 f.txt 파일을 새로 생성한다.

```bash
echo a2 >> a.txt
echo f1 >> f.txt
```

git status를 통해 상태를 체크한다.
그냥 git status를 치면 기본 `git status --long`으로 적용되서 나타난다.

![git status 와 git status --long 비교](/assets/img/git/git-status1.png "git status 와 git status --long 비교")

a.txt는 modified 됐다고 나온다.  
수정돼서 commit하려면 git add를 다시 해야된다고 나온다.

f.txt는 새로 추가해서 untracked 됐다고 나온다.
새로 추가해서 commit하려면 git add를 해야 된다고 나온다.

<hr>

## git status -s

위와 같은 상황에서 `git status -s`를 쳤다.

![git status -s 화면](/assets/img/git/git-status2.png "git status -s 화면")

<span style="color:#13fc03">A</span>는 현재 git에 add 되있다는 표시

<span style="color:#13fc03">A</span><span style="color:red">M</span>는 현재 git에 add 되있지만 수정돼서 commit하려면 git add를 다시 해야 된다.

<span style="color:red">??</span>는 새로 추가해서 untracked 됐다고 나온다.
새로 추가해서 commit하려면 git add를 해야 된다고 나온다.

git status -s는 화면에 자리를 별로 차지 안 하므로 빠르게 보고 싶을 때 유용하다.


