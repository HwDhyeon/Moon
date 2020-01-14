---
published: true
layout: "single"
title: "[Webhacking.kr] Challenge 문제풀이 - 1번"
category: "Webhacking.kr"
comments: true
---

여느 때와 같이 개발 공부를 하기 위해 다른 여러 개발 블로그들을 방문하던 중 재미있는 문제 풀이 사이트를 발견해서 한 번 문제를 풀어보고 문제를 푸는 과정을 소개하려고 한다.

먼저 문제 풀이에 앞서 사이트의 주소와 기본적인 회원가입, 로그인, 문제풀이 방법을 소개하려 한다. 이 내용은 이번 포스트에서 한 번만 설명한다.

사이트의 주소는 [webhacking.kr](https://webhacking.kr)<br>
먼저 사이트에 들어가면 다음과 같은 화면이 보이게 된다.

![main](https://user-images.githubusercontent.com/37629503/72317823-d2c4b300-36dd-11ea-8f14-c85f58555c48.png)

먼저 위에 보이는 __Login/Join__ 페이지로 이동한 뒤 아래에 정보를 입력하고 Join 버튼을 누르면 회원가입이 끝난다

![join](https://user-images.githubusercontent.com/37629503/72317837-de17de80-36dd-11ea-9f8e-c5f79b1b71d8.png)

회원가입이 완료되면 같은 페이지의 Login 폼에 정보를 입력하면 된다.

이제 다시 메인으로 돌아와서 Challeng(old) 페이지에 들어가자

![main-challenge](https://user-images.githubusercontent.com/37629503/72317866-ee2fbe00-36dd-11ea-8433-976a33b1023b.png)

그러면 다음과 같은 화면을 보게 된다.

![challenge-old](https://user-images.githubusercontent.com/37629503/72317876-f556cc00-36dd-11ea-8b14-eb0ae6504cc0.png)

이제 풀고 싶은 문제를 골라 풀기만 하면 된다. 꼭 순서대로 풀 필요는 없으나 번호순으로 난이도가 어려워지는 것 같아 낮은 번호부터 차근차근 풀어 나가는 게 좋을 것 같다.

이제 본격적으로 1번 문제를 풀어보자. 먼저 1번 문제를 선택하면 다음의 화면이 보인다.

![q-001](https://user-images.githubusercontent.com/37629503/72317883-fdaf0700-36dd-11ea-92a3-ddb49272ce41.png)

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
- `user_lv`라는 이름으로 쿠키 값을 `1`로 설정한다.
- 쿠키 값이 숫자가 아닐 경우 `1`로 설정된다.
- 쿠키 값이 `6`보다 크거나 같을 경우 `1`로 설정된다.
- 쿠키 값이 `5`보다 클 경우 문제 해결.

위 내용으로 미루어 볼 때 우리는 쿠키 값이 5보다 크지만 6보다 작다면 문제를 해결할 수 있다는 사실을 알 수 있다. 이는 쿠키 값을 조작해야 한다는 말이 된다. 쿠키 값을 조작하기 위해서 크롬이나 IE의 확장 프로그램들을 이용하거나 프로그래밍을 통해서 직접 조작할 수도 있다.. 하지만 나는 이 문제에서 크롬 개발자 도구를 사용할 생각이다.<br>
먼저 **F12**를 눌러 크롬 개발자 도구를 열자.

![dev_tool](https://user-images.githubusercontent.com/37629503/72317893-056eab80-36de-11ea-9a40-03bea9e6ff73.png)

상단에서 Application 탭을 찾아 이동한다. 보이지 않는 경우 **>>**를 클릭하면 찾을 수 있을 것이다.<br>
그럼 user_lv의 값이 1로 설정되어 있는 걸 볼 수 있는데 이를 5.1 ~ 5.9 사이 값으로 바꿔주자.

![cookie](https://user-images.githubusercontent.com/37629503/72317900-0d2e5000-36de-11ea-9bd5-2c777f72fd6b.png)

그런 다음 새로 고침을 하면 문제를 해결했다는 메시지가 나올 것이다.

![image](https://user-images.githubusercontent.com/37629503/72317125-9a23da00-36db-11ea-81d9-b1dc2b202705.png)

나는 이 포스트를 작성하기 전에 문제를 해결해서 이미 해결했다는 메시지가 나오지만 여러분은 다음과 같은 메시지를 볼 수 있을 것이다.

![image](https://user-images.githubusercontent.com/37629503/72317237-ec64fb00-36db-11ea-9a22-bf3f0f04e866.png)

우리는 이렇게 간단하게 벌써 한 문제를 해결했다. 앞으로도 이런 식으로 문제를 풀어나가다 보면 웹에 대한 지식이 더 늘어날 것이다.