---
published: true
layout: "single"
title: "[Python] 파이썬에서 타임아웃 기능 구현하기"
category: "python"
comments: true
---

파이썬으로 프로그래밍을 하다보면 함수가 특정 시간까지 작업이 완료되어야 하는 경우가 있다. 나는 주로 테스트를 할 때 이런 경우를 주로 겪었는데 예를 들면 서버에 리퀘스트 요청을 보냈는데 너무 오랜시간동안 아무런 응답도 오지 않는 경우가 있다. 이런 경우를 대비해서 그 함수에 타임아웃을 걸어주면 어느정도 해결할 수 있다. 나는 `multiprocessing` 모듈을 이용했는데 아마 찾아보면 다른 방법으로도 타임아웃을 구현할 수 있을것 같다. 소스코드는 아래와 같다.

```python
from multiprocessing import Process
import time
import sys


TIMEOUT = 5


def tenSecond():
    for i in range(10):
        sys.stdout.write('\r{} sec'.format(i + 1))
        time.sleep(1)
    print('\nDone!!')


def do(a):
    tenSecond()


if __name__ == "__main__":
    print('This program has a 5 second timeout.')
    a = 'arg1'
    actionProcess = Process(target=do, args=(a, ))
    actionProcess.start()
    actionProcess.join(timeout=TIMEOUT)
    actionProcess.terminate()
    print('\nProcess is End')
```

위 소스코드를 입력하고 실행해보면 `tenSecond` 함수가 5번만 실행되고 프로그램이 종료되는걸 볼 수 있다. 궁금하다면 `TIMEOUT` 변수의 값을 조절해가면서 확인해보도록 하자.
