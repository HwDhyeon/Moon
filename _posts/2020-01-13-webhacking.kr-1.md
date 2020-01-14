---
published: true
layout: "single"
title: "[Webhacking.kr] Challenge 문제풀이 - 1번"
category: "Webhacking.kr"
comments: true
---

여느때와 같이 개발 공부를 하기 위해 다른 여러 개발 블로그들을 방문하던 중 재미있는 문제 풀이 사이트를 발견해서 한 번 문제를 풀어보고 문제를 푸는 과정을 소개하려고 한다.

먼저 문제풀이에 앞서 사이트의 주소와 기본적인 회원가입, 로그인, 문제풀이 방법을 소개하려한다. 이 내용은 이번 포스트에서 한 번만 설명한다.

사이트의 주소는 [webhacking.kr](https://webhacking.kr)<br>
먼저 사이트에 들어가면 다음과 같은 화면이 보이게 된다.

![main](/assets/images/webhacking.kr/main.png)

먼저 위에 보이는 __Login/Join__ 페이지로 이동한 뒤 아래에 정보를 입력하고 Join 버튼을 누르면 회원가입이 끝난다

![main](/assets/images/webhacking.kr/join.png)

회원가입이 완료되면 같은 페이지의 Login 폼에 정보를 입력하면 된다.

이제 다시 메인으로 돌아와서 Challeng(old)페이지에 들어가자

![main](/assets/images/webhacking.kr/main-challenge.png)

그러면 다음과 같은 화면을 보게 된다.

![main](/assets/images/webhacking.kr/challenge-old.png)

이제 풀고 싶은 문제를 골라 풀기 마하 면 된다. 꼭 순서대로 풀 필요는 없으나 번호순으로 난이도가 어려워지는 것 같아 낮은 번호부터 차근차근 풀어 나가는 게 좋을 것 같다.

이제 본격적으로 1번 문제를 풀어보자. 먼저 1번 문제를 선택하면 다음의 화면이 보인다.

![main](/assets/images/webhacking.kr/q-001.png)

무언가 잘못된 것 같기도 하지만 저게 전부다. 문제에 대한 힌트는 view-source 페이지에서 얻을 수 있다. 다음은 링크를 클릭하면 보이는 모습이다.

```php
<?php
  include "../../config.php";
  if($_GET['view-source'] == 1){ view_source(); }
  if(!$_COOKIE['user_lv']){
    SetCookie("user_lv","1",time()+86400*30,"/challenge/web-01/");
    echo("<meta http-equiv=refresh content=0>");
  }
?>
<html>
<head>
<title>Challenge 1</title>
</head>
<body bgcolor=black>
<center>
<br><br><br><br><br>
<font color=white>
---------------------<br>
<?php
  if(!is_numeric($_COOKIE['user_lv'])) $_COOKIE['user_lv']=1;
  if($_COOKIE['user_lv']>=6) $_COOKIE['user_lv']=1;
  if($_COOKIE['user_lv']>5) solve(1);
  echo "<br>level : {$_COOKIE['user_lv']}";
?>
<br>
<a href=./?view-source=1>view-source</a>
</body>
</html>
```

우리는 위의 내용에서 몇 가지 정보를 알 수 있다.

- 이 페이지는 `php`로 통신한다.
- `user_lv` 라는 이름으로 쿠키값을 `1`로 설정한다.
- 쿠키값이 숫자가 아닐경우 `1`로 설정된다.
- 쿠키값이 `6`보다 크거나 같을경우 `1`로 설정된다.
- 쿠키값이 `5`보다 클 경우 문제 해결.

위 내용으로 미루어 볼 때 우리는 쿠키값이 5보다 크지만 6보다 작다면 문제를 해결할 수 있다는 사실을 알 수 있다. 그렇다면 쿠키값을 변조해야 하는데 크롬이나 IE의 확장프로그램들을 이용하거나 Python 등을 사용해 코드로 해결하는 방법도 있다. 하지만 나는 이 문제에서 크롬 개발자 도구를 사용할 생각이다.<br>
먼저 **F12**를 눌러 크롬 개발자도구를 열자.

![main](/assets/images/webhacking.kr/dev_tool.png)

상단에서 Application 탭을 찾아 이동한다. 보이지 않는경우 **>>** 를 클릭하면 찾을 수 있을 것이다.<br>
그럼 user_lv의 값이 1로 설정되어 있는걸 볼 수 있는데 이를 5.1 ~ 5.9 사이 값으로 바꿔주자.

![cookie](/assets/images/webhacking.kr/cookie.png)

그런다음 새로고침을 하면 문제를 해결했다는 메세지가 나올 것이다.

![image](https://user-images.githubusercontent.com/37629503/72317125-9a23da00-36db-11ea-81d9-b1dc2b202705.png)

나는 이 포스트를 작성하기 전에 문제를 해결해서 이미 해결했다는 메세지가 나오지만 여러분은 다음과 같은 메세지를 볼 수 있을것이다.

![image](https://user-images.githubusercontent.com/37629503/72317237-ec64fb00-36db-11ea-9a22-bf3f0f04e866.png)

우리는 이렇게 간단하게 벌써 한 문제를 해결했다. 앞으로도 이런식으로 문제를 풀어나가다 보면 웹에 대한 지식이 더 늘어날 것이다.