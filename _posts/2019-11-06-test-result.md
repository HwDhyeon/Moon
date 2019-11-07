---
layout: post
title: 11월 06일 - CLUSTER
tags: [work, report]
date: 2019-11-06
comments: false
---

# CLUSTER 테스트 안정화

### 08-CLUSTER-SYSTEM-RESRC2.side

- #### 총 5번의 테스트를 진행했다
- #### linux_server와 window_laptop 에서 진행했다
- #### 이 문서에는 linux_server의 결과만 기록한다

- 첫 번째 테스트 결과

```powershell
info:    Running 08-CLUSTER-SYSTEM-RESRC2.side
  console.log commons.js:147
    SKIP

 PASS  ./CLUSTER_SYSTEM_RESRC2.test.js (29.124s)
  CLUSTER_SYSTEM_RESRC2
    ✓ 01_CL_SYS_RESRC2_SEARCHBOX_01 (7117ms)
    ✓ 02_CL_SYS_RESRC2_SEARCHBOX_02 (11310ms)
    ✓ 03_CL_SYS_RESRC2_SEARCHBOX_03 (7968ms)
    ✓ 04_CL_SYS_RESRC2_DOWNLOAD (430ms)

Test Suites: 1 passed, 1 total
Tests:       4 passed, 4 total
Snapshots:   0 total
Time:        29.399s
Ran all test suites.
```

- 두 번째 테스트 결과

```powershell
info:    Running 08-CLUSTER-SYSTEM-RESRC2.side
  console.log commons.js:11
    Unable to resize window. Skipping.

  console.log commons.js:147
    SKIP

 FAIL  ./CLUSTER_SYSTEM_RESRC2.test.js (12.773s)
  CLUSTER_SYSTEM_RESRC2
    ✕ 01_CL_SYS_RESRC2_SEARCHBOX_01 (5153ms)
    ✕ 02_CL_SYS_RESRC2_SEARCHBOX_02 (2487ms)
     (2502ms)
    ✓ 04_CL_SYS_RESRC2_DOWNLOAD (418ms)

  ● CLUSTER_SYSTEM_RESRC2 › 01_CL_SYS_RESRC2_SEARCHBOX_01

    WebDriverError: unknown error: session deleted because of page crash
    from unknown error: cannot determine loading status
    from tab crashed
      (Session info: headless chrome=77.0.3865.90)

      at Object.throwDecodedError (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/error.js:550:15)
      at parseHttpResponse (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:560:13)
      at Executor.execute (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:486:26)

  ● CLUSTER_SYSTEM_RESRC2 › 02_CL_SYS_RESRC2_SEARCHBOX_02

    WebDriverError: unknown error: session deleted because of page crash
    from unknown error: cannot determine loading status
    from tab crashed
      (Session info: headless chrome=77.0.3865.90)

      at Object.throwDecodedError (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/error.js:550:15)
      at parseHttpResponse (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:560:13)
      at Executor.execute (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:486:26)

  ● CLUSTER_SYSTEM_RESRC2 › 03_CL_SYS_RESRC2_SEARCHBOX_03

    NoSuchSessionError: invalid session id

      at Object.throwDecodedError (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/error.js:550:15)
      at parseHttpResponse (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:560:13)
      at Executor.execute (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:486:26)

Test Suites: 1 failed, 1 total
Tests:       3 failed, 1 passed, 4 total
Snapshots:   0 total
Time:        13.063s, estimated 30s
Ran all test suites.
```

- 세 번째 테스트 결과

```powershell
info:    Running 08-CLUSTER-SYSTEM-RESRC2.side
  console.log commons.js:147
    SKIP

 FAIL  ./CLUSTER_SYSTEM_RESRC2.test.js (46.204s)
  CLUSTER_SYSTEM_RESRC2
     01_CL_SYS_RESRC2_SEARCHBOX_01 (3587ms)
    ✓ 02_CL_SYS_RESRC2_SEARCHBOX_02 (30451ms)
    ✓ 03_CL_SYS_RESRC2_SEARCHBOX_03 (9567ms)
    ✓ 04_CL_SYS_RESRC2_DOWNLOAD (418ms)

  ● CLUSTER_SYSTEM_RESRC2 › 01_CL_SYS_RESRC2_SEARCHBOX_01

    WebDriverError: unknown error: session deleted because of page crash
    from unknown error: cannot determine loading status
    from tab crashed
      (Session info: headless chrome=77.0.3865.90)

      at Object.throwDecodedError (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/error.js:550:15)
      at parseHttpResponse (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:560:13)
      at Executor.execute (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:486:26)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 3 passed, 4 total
Snapshots:   0 total
Time:        46.488s
Ran all test suites.
```

- 네 번째 테스트 결과

```powershell
info:    Running 08-CLUSTER-SYSTEM-RESRC2.side
  console.log commons.js:147
    SKIP

 FAIL  ./CLUSTER_SYSTEM_RESRC2.test.js (26.998s)
  CLUSTER_SYSTEM_RESRC2
    ✓ 01_CL_SYS_RESRC2_SEARCHBOX_01 (7389ms)
    ✓ 02_CL_SYS_RESRC2_SEARCHBOX_02 (10800ms)
    ✕ 03_CL_SYS_RESRC2_SEARCHBOX_03 (6311ms)
    ✓ 04_CL_SYS_RESRC2_DOWNLOAD (421ms)

  ● CLUSTER_SYSTEM_RESRC2 › 03_CL_SYS_RESRC2_SEARCHBOX_03

    WebDriverError: unknown error: session deleted because of page crash
    from unknown error: cannot determine loading status
    from tab crashed
      (Session info: headless chrome=77.0.3865.90)

      at Object.throwDecodedError (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/error.js:550:15)
      at parseHttpResponse (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:560:13)
      at Executor.execute (../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:486:26)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 3 passed, 4 total
Snapshots:   0 total
Time:        27.256s, estimated 47s
Ran all test suites.
```

- 다섯 번째 테스트 결과

```powershell
info:    Running 08-CLUSTER-SYSTEM-RESRC2.side
  console.log commons.js:147
    SKIP

 FAIL  ./CLUSTER_SYSTEM_RESRC2.test.js (41.014s)
  CLUSTER_SYSTEM_RESRC2
    ✕ 01_CL_SYS_RESRC2_SEARCHBOX_01 (19021ms)
    ✓ 02_CL_SYS_RESRC2_SEARCHBOX_02 (11187ms)
    ✓ 03_CL_SYS_RESRC2_SEARCHBOX_03 (8162ms)
    ✓ 04_CL_SYS_RESRC2_DOWNLOAD (428ms)

  ● CLUSTER_SYSTEM_RESRC2 › 01_CL_SYS_RESRC2_SEARCHBOX_01

    TimeoutError: Waiting for element to be located By(xpath, //div/input)
    Wait timed out after 15118ms

      at ../../../../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/webdriver.js:841:17

Test Suites: 1 failed, 1 total
Tests:       1 failed, 3 passed, 4 total
Snapshots:   0 total
Time:        41.274s
Ran all test suites.
```

- 세 번째 테스트 이후로는 같은 결과가 나왔다
- 하지만 다섯번의 테스트결과가 모두 일치하지 않는다
- 총 3번 실패한 01_CL_SYS_RESRC2_SEARCHBOX_01를 살펴볼 필요가 있다