---
layout: post
title: [Python] 파일 다루기 (읽기, 쓰기)
tags: [python, learn]
date: 2019-11-01
comments: true
---

# [Python] 파일 다루기

예시 txt
```
filename : example.txt


1
2
3
4
5
6
7
8
9
10
```

예시 py
```python
f = open(example.txt, 'r')
# Your code here!!
f.close()
```
예시 또다른 입출력 py
```python
with open(example.txt, 'r') as f:
    # Your code here!!
```
위의 두개는 완전히 동일함
