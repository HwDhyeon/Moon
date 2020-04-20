---
published: true
layout: "single"
title: "[Git] Git에서 특정 브랜치를 삭제하고 GitHub에 반영하기"
category: "git"
comments: true
---

Git을 사용할 때 특정 브랜치가 더 이상 필요없어져서 그 브랜치를 삭제하고 싶은 경우가 있다. 그래서 오늘은 브랜치를 삭제하고 이를 깃허브에 반영하는 방법을 알아보자.

```shell
> git branch --delete [branch name]
> git push origin :[branch name]
```

위 명령어에서 `--delete`는 `-D`로 바꾸어 쓸 수 있다.

몇 가지 주의할 점은

- 현재 작업중인 브랜치가 삭제하려는 브랜치이면 삭제되지 않고 오류가 발생한다.
- 푸시를 할때 오류가 발생한다면 `-f` 옵션을 주어 강제로 푸시하면 된다.
- 한 번 삭제한 브랜치는 되돌릴 수 없다.

이 정도만 기억하고 있다면 브랜치를 실수로 삭제하거나 하는 일은 없을 것이다.
