---
published: true
layout: "single"
title: "[Linux] alpine 리눅스에서 타임존 설정하기"
category: "linux"
comments: true
---

이번 시간에는 알파인 리눅스 환경에서 타임존은 변경하는 방법을 알아보자.

***

## 알파인 리눅스란

![iamge](https://alpinelinux.org/alpinelinux-logo.svg)

알파인 리눅스는 기존 리눅스 환경에서 불필요하거나 중요도가 낮은 라이브러리나 파일 또는 기능들을 제거한 리눅스 배포판이다.  
알파인 리눅스 [공식 홈페이지](https://alpinelinux.org/about/)에서는 다음과 같이 설명하고 있다

> Alpine Linux is a security-oriented, lightweight Linux distribution based on musl libc and busybox.

간단히 얘기하면 알파인 리눅스는 `musl libc`와 `busybox` 기반의 보안을 중점으로 둔 경량 리눅스 배포판이라고 한다. 나는 위 두개가 정확히 무엇인지는 모르겠지만 알파인 리눅스에서는 용량이 큰 **bash** 대신 **busybox**를 사용한다고 한다. 그래서 인터프리터를 `#!/bin/bash`로 설정한 쉘 스크립트 파일들은 알파인 리눅스 환경에서 작동하지 않는다.

알파인 리눅스의 소개 전문은 다음과 같다.

![image](https://user-images.githubusercontent.com/37629503/80342982-1499f380-88a0-11ea-99e8-fe1a501113c8.png)

## 타임존 설정하기

이제 알파인 리눅스에대해서 간단하게 알아보았으니 본격적으로 타임존은 변경하는 방법을 알아보자.  
먼저 [알파인 리눅스 위키](https://wiki.alpinelinux.org/wiki/Setting_the_timezone)에 들어가면 `uclibc`가 설치되지 않은 환경에서만 사용하는 설정법이라는 경고문구를 볼 수 있다.  또한 `glibc`가 설치되어있는 환경에서는 다른 설정방법을 사용한다고 한다.

먼저 알파인 리눅스에는 타임존에 대한 데이터도 포함되어있지 않기 때문에 라이브러리를 다운로드 받아야 한다.

```shell
> apk --no-cache add tzdata
> ls /usr/share/zoneinfo
```

알파인 리눅스에서는 apk를 사용한다.  
`ls` 명령어를 통해 전 세계의 타임존 정보를 보고 나에게 필요한 지역을 찾도록 하자.

이제 필요한 지역을 정했으면 `/etc/localtime` 경로로 타임존 정보를 복사해야 한다.

```shell
> cp /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```

나는 예시로 서울의 타임존을 선택했다.

타임존 정보를 성공적으로 로컬로 복사해왔으면 타임존 정보를 업데이트하고 `date` 명령어를 통해 확인해보자.

```shell
> echo "Europe/Brussels" >  /etc/timezone
> date
Mon Apr 27 16:29:26 KST 2020
```

결과가 위와 같이 나오면 성공이다.

이제 다른 타임존 정보들은 필요없으니 모두 지워주도록 하자.

```shell
> apk del tzdata
```
