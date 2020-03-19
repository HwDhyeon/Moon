---
published: true
layout: "single"
title: "[Git] Git에서 특정 브랜치만 clone하기"
category: "git"
comments: true
---

git을 사용하다 보면 clone 시 수많은 브랜치 정보를 전부 받아오는 게 부담스러울 때가 있다. 그럴 때 내가 원하는 브랜치만 clone 해 올수 있는데 아래와 같이 clone 받으면 된다.

```git
git clone -b {branch name} --single-branch {repo url}
e.g,) git clone -b master --single-branch https://github.com/golang/go.git
```
