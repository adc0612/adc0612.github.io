---
layout: post
comments: true
# date: 2020-12-01 18:00:00
title: "Git Workflow"
description: "Git의 전반적인 흐름에 대한 설명"
subject: git
categories: [ git ]
tags: [ git ]
---

<hr>

# Git 구조

![Git Workflow 설명](/assets/img/git/git-workflow.png "Git Workflow 설명")

Git에는 크게 4개 작업환경이 있다.
* working directory  
프로젝트의 파일들을 작업하는 곳

* staging area  
작업한 파일들을 Git에 올릴 준비 된 파일들이 있는곳  
`git add`로 후 commit을 누르기 전 단계

* .git directory `(local)`  
파일들의 version history를 가지고 있는 곳  
`git commit`으로 파일들의 version history를 저장한다.

* .git directory `(remote)`  
파일들의 version history를 가지고 있는 `원격 저장소`  
Local에서 `git push`로 원격저장소 파일들의 version history를 저장한다.  
ex: **Github**

<hr>

# working directory

![working directory 구조 설명](/assets/img/git/local1.png "working directory 구조 설명")

2가지 구조

1. untracted  
아직 git add를 하지 않아 git이 file history를 가지고 있지 않은 파일  
즉 git이 아직 `tracking 하지 않은` 파일

2. tracted  
git add를 하여 git이 file history를 가지고 있는 파일들
여기서도 2가지 나눌 수 있다.  
즉 git이 `tracking 하고 있는` 파일

    * unmodified  
git이 가지고 있는 file history랑 `같은 내용`을 가지고 있는 파일  
(git add 후 변경이 `없는` 파일)

    * modified  
git이 가지고 있는 file history랑 달리 `수정된 내용`을 가지고 있는 파일  
(git add 후 변경이 `있는` 파일)

![working directory 구조2 설명](/assets/img/git/local2.png "working directory 구조2 설명")

git commit을 하면 위 사진과 같이 unmodified 파일은 건들지 않고 `modified`된 파일만 staging area로 가져간다.