---
published: true
layout: "single"
title: "[Go] golang의 ssh 모듈을 이용해 SSH Client 구성하기"
category: "golang"
comments: true
---

## 오늘의 목표

- Go에서 지원하는 ssh 패키지를 이용해 SSH 서버에 접속한다
- SSH 서버에 명령어를 보내 처리하고 결과를 얻는다

## 잡담

최근 들어서 부쩍 Go에 대한 관심이 높아져 하나둘씩 인터넷에서 정보를 찾아 공부를 하고 있다.  
지금까지 그래왔고 앞으로도 한동안은 Python을 주력으로 사용하겠지만 최근 가장 핫한 언어이기도 하고 마침 Python 과는 다른 맛의 언어를 배우고 싶던 시기와 맞물려서 Go의 매력에 빠지게 된 것 같다.  
혹시 Go에 관심이 있고 지금 쓰고 있는 언어가 재미가 없다면 나처럼 Go를 만나보는 것도 좋을 것 같다. 여러 가지 이유로 실무에서 사용하지 못하더라도 언어 자체가 강력하기 때문에 배워둔다면 언젠가 찾게 될 일이 생길 것이다.  
아무튼 이번 시간에는 Go의 ssh 패키지를 이용해서 SSH 서버에 접속하고 간단한 명령어를 실행해보자.

## 코드 작성하기

먼저 모든 Go 스크립트의 기본인 main 함수를 만들어준다.  
어떤 Go 스크립트를 만들더라도 무조건 들어가야 한다. 마치 C언어의 main 함수처럼.  

```go
package main

func main() {

}
```

메인 함수 선언을 끝마쳤으면 이번 코드에서 필요한 전역변수들을 선언해야 한다. 사실 이번 코드는 복잡하지 않아서 굳이 선언할 필요는 없지만 선언해두면 여러모로 편하다. 전역변수는 항상 패키지 선언 줄 아래에 선언해 주도록 하자

```go
// 패스워드 전달 방식과 타임아웃 전역변수 설정
const (
    CertPassword      = 1
    CertPublicKeyFile = 2
    DefaultTimeout    = 3 // Second
)

// SSH 접속에 필요한 정보를 담는 생성자
type SSH struct {
    IP      string
    User    string
    Cert    string //password or key file path
    Port    int
    session *ssh.Session
    client  *ssh.Client
}
```

여담으로 Go에서는 CamelCase를 사용할 것을 강력하게 권장한다. CamelCase를 사용하지 않으면 계속해서 Warning을 띄운다. 별로 신경 쓰지 않는 사람이라면 상관없겠지만 대부분의 사람들은 심하게 거슬릴 것이 뻔하기 때문에 그냥 하라는 대로 하자. CamelCase가 뭔지 궁금하다면 [여기](https://ko.wikipedia.org/wiki/%EB%82%99%ED%83%80_%EB%8C%80%EB%AC%B8%EC%9E%90)에 자세하게 적혀있다.

가장 먼저 SSH 접속시 패스워드가 아니라 퍼블릭 키를 사용하는 경우를 대비해서 퍼블릭 키를 파싱하는 함수를 작성하도록 하자.

```go
func (S *SSH) readPublicKeyFile(file string) ssh.AuthMethod {
    buffer, err := ioutil.ReadFile(file)
    if err != nil {
        return nil
    }

    key, err := ssh.ParsePrivateKey(buffer)
    if err != nil {
        return nil
    }
    return ssh.PublicKeys(key)
}
```

Go는 키 파일을 읽는 방법이 좀 복잡한데 먼저 일반 파일처럼 읽고 프라이빗 키로 파싱한 다음 퍼블릭 키로 변환해서 리턴해야 한다. 아마 다른 언어에서도 비슷한 과정을 거치겠지만 이 과정을 직접 구현해야 한다는 것은 귀찮긴 하다.

이제 서버에 연결하는 함수를 작성해보자.

```go
// Connect the SSH Server
func (S *SSH) Connect(mode int) {
    var sshConfig *ssh.ClientConfig
    var auth []ssh.AuthMethod
    if mode == CertPassword {
        auth = []ssh.AuthMethod{
            ssh.Password(S.Cert),
        }
    } else if mode == CertPublicKeyFile {
        auth = []ssh.AuthMethod{
            S.readPublicKeyFile(S.Cert),
        }
    } else {
        log.Println("does not support mode: ", mode)
        return
    }

    sshConfig = &ssh.ClientConfig{
        User: S.User,
        Auth: auth,
        HostKeyCallback: func(hostname string, remote net.Addr, key ssh.PublicKey) error {
            return nil
        },
        Timeout: time.Second * DefaultTimeout,
    }

    client, err := ssh.Dial("tcp", fmt.Sprintf("%s:%d", S.IP, S.Port), sshConfig)
    if err != nil {
        fmt.Println(err)
        return
    }

    session, err := client.NewSession()
    if err != nil {
        fmt.Println(err)
        client.Close()
        return
    }

    S.session = session
    S.client = client
}

// RunCmd to SSH Server
func (S *SSH) RunCmd(cmd string) {
    out, err := S.session.CombinedOutput(cmd)
    if err != nil {
        fmt.Println(err)
    }
    fmt.Println(string(out))
}
```

먼저 인자로 `mode`라는 변수를 받아서 패스워드를 어떤 방식으로 입력할 것인지 판단한다. 패스워드를 직접 입력하려면 `CertPassword`를, 퍼블릭 키 파일을 이용하려면 `CertPublicKeyFile`를 인자로 넘겨주면 `readPublicKeyFile`함수를 호출해서 퍼블릭 키를 가져오게 된다. 그리고 ssh 접속을 윟ㄴ 설정을 해주고 `ssh.Dial` 함수로 서버에 연결을 시도한다. 여기서는 url과 port를 하나의 문자열로 연결할 때 `fmt.Sprintf` 함수를 사용했지만 처음부터 port를 string으로 선언해서 연결해 주는 것이 더 간편할 것 같다. 마지막으로 서버에 연결하는 데에까지 성공했다면 새로운 세션을 열고 Connect 함수의 역할은 끝이 나게 된다.

다음으론 커맨드를 서버에 전송하는 함수를 작성헤보자.

```go
func (S *SSH) RunCmd(cmd string) {
    out, err := S.session.CombinedOutput(cmd)
    if err != nil {
        fmt.Println(err)
    }
    fmt.Println(string(out))
}
```

이 부분은 다른 언어와 크게 다른 점이 없으니 그냥 넘어가도록 하자.

이제 세션을 열었으면 마지막으로 세션을 닫아줘야 한다. 세션을 닫아 줄 Close 함수를 작성하자.

```go
func (S *SSH) Close() {
    S.session.Close()
    S.client.Close()
}
```

이 부분도 넘어가자.

마지막으로 처음에 만들었지만 사용하지 않았던 메인 함수에 지금까지의 내용을 추가하면 완성이다.

```go
func main() {
    client := &SSH{
        IP:   "ServerURL",
        User: "Username",
        Port: 22,
        Cert: "Password or Key file path",
    }
    client.Connect(CertPassword) // If you are using a key file, use 'CertPublicKeyFile' instead.
    client.RunCmd("ls -al")
    client.Close()
}
```

## 마치며

이렇게 작성하기는 귀찮지만 사용하기는 간단한 ssh 클라이언트 모듈을 만들어봤다. 이걸 응용해서 더 쉽게 구성할 수 있는 코드를 만들어봐도 재밌을 것 같다. Go는 처음 접하면 나처럼 이해가 안 되기도 하고 어렵게 느껴질 수도 있다. 하지만 쓰면 쓸수록 Go의 매력에 빠지게 될 거다. 다만 Go를 첫 프로그래밍 언어로 선택하는 것은 추천하지 않는다. 처음 접하기에는 어렵기도 하고 개발자가 신경 써야 할 부분이 생각보다 많다. 만일 프로그래밍을 Go로 시작할 생각이라면 다시 한번 고민해보는 것을 추천한다. 이상으로 오늘 글은 마치겠다.

전체 소스코드가 궁금하다면 [여기](https://gist.github.com/HwDhyeon/4250215d5166bc0244df8978af2e7e71)를 참고하면 된다.
