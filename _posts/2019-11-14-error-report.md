---
layout: post
title: 11월 14일 - 오류 보고서
tags: [work, report]
date: 2019-11-14
comments: false
---

# 11월 14일 오류 보고서

- 05-DATABROWSER-MANAGE-REPORT.side 파일에서 에러 발생

```
✓ 01_DB_MNG_RPT_SEARCH (6925ms)
✓ 02_DB_MNG_RPT_COPY (7947ms)
✕ 03_DB_MNG_RPT_DASHBOARD_01 (2326ms)
✓ 04_DB_MNG_RPT_DASHBOARD_02 (10063ms)
✓ 05_DB_MNG_RPT_DASHBOARD_03 (8257ms)
✓ 06_DB_MNG_RPT_REFRESH (6634ms)
✓ 07_DB_MNG_RPT_RENAME (8407ms)
✓ 08_DB_MNG_RPT_AUTHORITY_01 (8277ms)
✓ 09_DB_MNG_RPT_AUTHORITY_02 (7597ms)
✓ 10_DB_MNG_RPT_DEL (8244ms)
✕ 11_DB_MNG_RPT_MODIFY (9666ms)
✓ 12_DB_MNG_RPT_PAGENATION(origin) (6508ms)
```

- 에러 메세지

```
  ● DB_MNG_RPT › 03_DB_MNG_RPT_DASHBOARD_01

    ElementClickInterceptedError: element click intercepted: Element ...<div data-v-d7d5f886="" data-v-644a36e9="" id="menu-path"></div> is not clickable at point (180, 21). Other element would receive the click: <div data-v-644a36e9="" class="button-group">...</div>
      (Session info: headless chrome=77.0.3865.90)

      at Object.throwDecodedError (../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/error.js:550:15)
      at parseHttpResponse (../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:560:13)
      at Executor.execute (../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:486:26)

  ● DB_MNG_RPT › 11_DB_MNG_RPT_MODIFY

    ElementClickInterceptedError: element click intercepted: Element <a>...</a> is not clickable at point (1621, 91). Other element would receive the click: <div class="mu-background ng-scope" mu-dialog="studio.alert" style="z-index: 101; display: block;"></div>
      (Session info: headless chrome=77.0.3865.90)

      at Object.throwDecodedError (../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/error.js:550:15)
      at parseHttpResponse (../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:560:13)
      at Executor.execute (../../../usr/local/lib/node_modules/selenium-side-runner/node_modules/selenium-webdriver/lib/http.js:486:26)
```

- 03_DB_MNG_RPT_DASHBOARD_01
  - 가끔가다 발생하는 오류
  - 현재로서는 발생하는 경우보다 그렇지 않은 경우가 압도적으로 많음
  - 따라서 지금 고칠 필요는 없다고 판단 됨

- 11_DB_MNG_RPT_MODIFY
  - 고질적으로 발생하는 오류

  ![image](https://user-images.githubusercontent.com/37629503/68828746-23259180-06ea-11ea-9809-9dc161ca06e9.png)
  - 위의 오류창을 해결해야 함
  - selenium-side-runner가 "예" 버튼을 제대로 인식하지 못해 오류 발생
  - selenium IDE 에서는 제대로 작동함
  - 우선순위로 해결해야 하지만 그동안의 방법이 제대로 작동하지 않음

  - 지금까지 알아낸 위의 오류를 해결하기 어려운 이유
    - 해당 페이지를 열 때마다 div의 순서가 뒤바뀌어 고정 index로는 해결할 수 없다
    - 같은 내용의 alert이 3 ~ 7번 가량 나타난다

- 지금까지 해결을 위한 노력
  - do ~ repeat if ~ 구문을 사용해 몇번이든 alert 이 나타나도 대응 가능하게 설계함
  - store xpath count ~ 구문을 사용해 alert 의 개수를 is_alert에 저장
    - verify, assert 그리고 wait for element ~ 를 사용하면 오류가 난다
  - `if ${is_alert} != 0` 이라면 __예__ 버튼을 클릭하고 다시 반복