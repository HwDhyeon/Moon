---
published: true
layout: "single"
title: "[Kubernetes] 쿠버네티스 대화형 튜토리얼"
category: "kubernetes"
comments: true
---

쿠버네티스 대화형 튜토리얼이 한글로 번역되어있지 않아 직접 번역했다. 오역이 많을 수 있으나 이해하는데에 지장을 주는 수준을 아닐 것이다.

## Module 1 - Create a Kubernetes cluster

대화형 튜토리얼은 [여기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/create-cluster/cluster-interactive/)에서 진행할 수 있다.

### Cluster up and running

당신을 위해 Minikube가 이미 설치 되어있다. 다음 명령어를 통해 제대로 설치되어있는지 확인하자.

```shell
> minikube version
```

우리는 Minikube가 설치되어 있음을 확인했다. 다음 명령어를 통해 Minikube 클러스터를 실행하자.

```shell
> minikube start
```

당신은 이제 온라인 터미널에 Kubenetes 클러스터가 실행중이다.  
Minikube가 가상 머신을 시작 했고 Kubenetes 클러스터가 해당 VM 에서 실행 중이다.

### Cluster version

Kubenetes와 상호작용 하기 위해 CLI인 Kubectl을 사용한다. 이에 대해선 다음에 자세히 설명하고 지금은 몇 가지 클러스터 정보만 살펴보겠다. 다음 명령어를 통해 Kubectl이 설치되어 있는지 확인하자.

```shell
> kubectl version
```

Kubectl이 제대로 설치 되어있다. 특이한 점은 클라이언트와 서버 버전을 모두 볼 수 있다.  
클라이언트 버전은 Kubectl 버전이고 서버 버전은 마스터에 설치된 Kubenetes 버전이다.

### Cluster details

클러스터 세부 정보를 확인해보자. 다음 명령어를 통해 확인할 수 있다.

```shell
> kubectl cluster-info
```

러닝 마스터와 대시 보드가 있다. Kubenetes 대시 보드를 사용하면 UI에서 애플리케이션을 볼 수 있다. 하지만 지금은 사용하지 않는다.  
클러스터에서 노드를 보려면 다음 명령어를 사용한다.

```shell
> kubectl get nodes
```

이 명령은 애플리케이션을 호스팅하는 데 사용 할 수 있는 모든 노드를 보여준다. 지금 우리는 하나의 노드가 있고 준비 상태임을 알 수 있다.

## Module 2 - Deploy an app

대화형 튜토리얼은 [여기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/deploy-app/deploy-interactive/)에서 진행할 수 있다.

### kubectl basics

Minikube와 마찬가지로 kubectl은 온라인 터미널에 설치됐다. 사용법을 보려면 터미널에 `kubectl`을 입력하면 된다. kubectl 명령의 일반적인 형식은 `kubectl action resource`이다. 명령 뒤에 `--help`를 이용해 명령어에 대한 추가 정보를 얻을 수있다.

다음 명령어를 이용해 kubectl이 클러스터와 통신 가능한 지 확인한다

```shell
> kubectl version
```

kubectl이 설치 되었으면 클라이언트와 서버 정보를 모두 볼 수 있다.

클러스터에서 node를 보려면 다음 명령어를 실행하자.

```shell
> kubectl get nodes
```

여기에 사용 가능한 노드가 표시된다(현재는 하나). Kubernetes는 Node 가용 리소스를 기반으로 애플리케이션을 배포 할 위치를 선택한다.

### Deploy our app

kubectl run 명령을 사용하여 Kubernetes에서 첫 번째 앱을 실행 해 보자.  
`run` 명령어는 새 Deployment를 만든다. Deployment 이름과 앱 이미지 위치를 함께 적어야 한다.   (Docker 허브 외부에서 호스팅되는 이미지의 전체 리포지토리 URL 포함)

```shell
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

Deployment를 만들어 첫 애플리케이션을 배포했다. 다음의 과정을 거쳤다.

- 애플리케이션 인스턴스를 실행할 수 있는 적합한 노드 탐색(현재는 하나)
- 해당 노드에서 애플리케이션이 실행되도록 예약
- 필요할 때 새로운 노드에서 인스턴스를 다시 예약 할 수있는 클러스터를 구성

deployments list 를 보려면 다음 명령어를 실행하자.

```shell
> kubectl get deployments
```

앱의 단일 인스턴스를 실행하는 Deployment가 한 개 있는것을 알 수 있다.  
인스턴스가 노드의 Docker 컨테이너 내에서 실행 중이다.

### View our app

Kubernetes 내에서 실행되는 파드는 각각 격리 된 네트워크에서 실행된다.  
기본적으로 동일한 Kubenetes 클러스터 내의 다른 파드 및 서비스에서는 볼 수 있지만 해당 네트워크 외부에서는 볼 수 없다.  
kubectl을 사용하면 API 엔드 포인트를 통해 상호 작용하여 애플리케이션과 통신한다.

모듈 4에서는 kubernetes 클러스터 외부에서 애플리케이션을 노출하는 방법에 대한 방법을 다룰 것이다.

## Module 3 - Explore your app

대화형 튜토리얼은 [여기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/explore/explore-interactive/)에서 진행할 수 있다.

### Check application configuration

이전 시나리오에서 배포한 애플리케이션이 실행중인지 확인 해야한다.  
다음 명령어를 통해 기존 파드를 찾는다.

```shell
> kubectl get pods
```

만일 파드가 실행되고 있지 않으면 대화형 환경이 여전히 이전 상태로 로드되고 있는것이다.  
잠시 후 명령어를 다시 실행해보자. 한 개의 파드가 실행중이라면 계속 진행하면 된다.

다음으로, 파드에 있는 컨테이너와 컨테이너를 만드는 데 사용되는 이미지를 보려면 다음 명령어를 실행해야 한다.

```shell
> kubectl describe pods
```

여기서 파드 컨테이너에 대한 세부 정보(IP 주소, 사용된 포트 및 파드 수명주기와 관련된 이벤트 목록)를 볼 수 있다.

`describe` 명령어의 결과물은 광범위 하고 아직 설명하진 않았지마 튜토리얼이 끝날때에는 익숙해 질것이다.

Note : `describe` 명령어를 사용하면 대부분의 Kubenetes 기본 요소(노드, 파드, 배포)에 대한 자세한 정보를 얻을 수 있다.  
`describe` 스크립트가 아니라 사람이 읽을 수 있도록 설계되어 있다.

### Show the app in the terminal

파드는 격리된 private 네트워크에서 실행 중이므로 디버그하고 상호작용할 수 있도록 액세스를 프록시 해야 한다.  
이를 위해 새 터미널을 열고 다음 명령어를 수행한다.

```shell
> echo -e "\n\n\n\e[92mStarting Proxy. After starting it will not output a response. Please click the first Terminal Tab\n"; kubectl proxy
```

이제 다시 파드 이름을 얻고 프록시를 통해 해당 파드를 쿼리해야 한다.  
파드 이름을 가져와서 `POD_NAME` 환경변수에 저장하려면 다음 명령어를 실행한다.

```shell
> export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
> echo Name of the Pod: $POD_NAME
```

애플리케이션의 output을 확인하려면 `curl`요청을 수행해야 한다.

```shell
> curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
```

url은 파드의 API 경로이다.

### View the container logs

애플리케이션이 일반적으로 STDOUT에 전달하는 것은 파드 내의 컨테이너에 대한 로그가 된다.  
다음 명령어를 사용하여 이러한 로그를 검색할 수 있다.

```shell
> kubectl logs $POD_NAME
```

Note : 현재 파드 내에 컨테이너가 하나만 있으므로 파드 이름을 지정해 줄 필요가 없다.

### Executing command on the container

파드가 실행되면 컨테이너에서 명령을 직접 실행할 수 있다.  
이를 위해 `exec` 명령어를 사용해 매개 변수로 파드의 이름을 사용한다.  
먼저 환경 변수들을 나열해보자.

```shell
> kubectl exec $POD_NAME env
```

다시 말하지만, 파드 자체에는 단일 컨테이너만 있기 때문에 컨테이너 이름은 생략할 수 있다.

다음으로 파드 컨테이너에서 bash 세션을 시작해보자.

```shell
> kubectl exec -ti $POD_NAME bash
```

이제 NodeJS 애플리케이션을 실행하는 컨테이너에 개방형 콘솔이 있다.  
앱의 소스코드는 `server.js`에서 확인할 수 있다.

```shell
> cat server.js
```

`curl` 명령어를 통해 애플리케이션이 작동 중인지 확인할 수 있다.

```shell
> curl localhost:8080
```

Note : 여기선 NodeJS 파드 내부에서 명령어를 실행했기 때문에 `localhost` 를 사용했다.  
`localhost:8080` 에 연결할 수 없는 경우 `kubectl exec` 명령어를 실행하고 파드 내에서 명령어를 실행 중인지 확인해야 한다.

컨테이너와의 연결을 종료하려면 다음 명령어를 실행해야 한다.

```shell
> exit
```

## Module 4 - Expose your app publicly

대화형 튜토리얼은 [여기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/expose/expose-interactive/)에서 진행할 수 있다.

### Create a new service

우리의 애플리케이션이 실행되고 있는지 확인하자.  
다음 명령어를 통해 기존 파드들을 탐색한다.

```shell
> kubectl get pods
```

만일 파드가 실행되고 있지 않으면 대화형 환경이 여전히 이전 상태로 로드되고 있는것이다.  
잠시 후 명령어를 다시 실행해보자. 한 개의 파드가 실행중이라면 계속 진행하면 된다.

다음으로 클러스터의 현재 서비스들을 찾아보자.

```shell
> kubectl get services
```

Minikube가 클러스터를 시작할 때 기본적으로 생성되는 kubenetes라는 서비스가 있다.  
새 서비스를 생성하고 외부 트래픽에 노출시키기 위해 NodePort와 함께 `expose` 명령어를 매개 변수로 사용한다.  
(Minikube는 아직 LoadBalancer 옵션을 지원하지 않는다)

```shell
> kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```

`get services` 명령어를 다시 실행해보자.

```shell
> kubectl get services
```

이제 `kubenetes-bootcamp` 라는 실행중인 서비스가 있다.  
여기서는 서비스가 고유한 클러스터 IP 내부 포트 및 외부 IP(노드의 IP)를 받았음을 알 수 있다.

외부에서 열린 포트를 확인하려면 아래 명령어를 NodePort 옵션을 이용해 실행한다.

```shell
> export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
> echo NODE_PORT=$NODE_PORT
```

이제 `curl` 명렁어를 이용해 앱이 노드의 IP및 외부 노출 포트를 사용해 클러스터 외부에 노출되는지 테스트 할 수 있다.

```shell
> curl $(minikube ip):$NODE_PORT
```

그리고 우리는 서버로부터 서비스가 노출되었다는 응답을 얻을 수 있다.

### Using labels

배포시 파드에 대한 레이블이 자동으로 생성되었다.  
다음 명령어를 통해 레이블의 이름을 확인할 수 있다.

```shell
> kubectl describe deployment
```

이제 이 레이블을 이용해 파드 목록을 검색할 것이다.  
`kubectl get pods` 명령어의 매개 변수로 `-l` 를 지정하고 레이블 값을 사용한다.

```shell
> kubectl get pods -l run=kubernetes-bootcamp
```

기존 서비스 목록을 검색하기 위해 동일한 방식을 사용할 수 잇다.

```shell
> kubectl get services -l run=kubernetes-bootcamp
```

이제 파드 이름을 가져와서 POD_NAME 환경 변수에 저장해보자.

```shell
> export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{.metadata.name}}{{"\n"}}{{end}}')
> echo Name of the Pod: $POD_NAME
```

새 레이블을 적용하려면 `label` 명령어 다음에 객체 유형, 객체 이름 및 새 레이블을 이용한다.

```shell
> kubectl label pod $POD_NAME app=v1
```

이렇게 하면 파드에 새 레이블이 적용되고(애플리케이션 버전을 파드에 고정), `describe pod` 명령어를 통해 확인할 수 있다.

```shell
> kubectl describe pods $POD_NAME
```

레이블이 파드에 기록된 것을 확인할 수 있다.  
이제 새 레이블을 이용하여 파드 목록을 검색할 수 있다.

```shell
> kubectl get pods -l app=v1
```

이제 파드를 보자.

### Deleting a service

서비스를 삭제하기 위해선 다음 명령어를 사용한다.  
레이블은 여기에서도 사용할 수 있다.

```shell
> kubectl delete service -l run=kubernetes-bootcamp
```

서비스가 정상적으로 삭제되었는지 확인하자.

```shell
> kubectl get services
```

서비스가 정상적으로 삭제되었음을 확인했다.  
경로가 더 이상 노출되지 않는지 확인하기 위해 이전에 노출된 IP와 포트를 `curl` 명령어를 통해 검색할 수 있다.

```shell
> curl $(minikube ip):$NODE_PORT
```

이는 클러스터 외부에서 더 이상 연결할 수 없음을 확인할 수 있다.  
앱이 여전히 파드 내부에 curl이 있는 상태로 실행중인지 확인 할 수 있다.

```shell
> kubectl exec -ti $POD_NAME curl localhost:8080
```

우리는 애플리케이션이 아직 작동중임을 확인할 수 있다.  
이는 Deployment가 애플리케이션을 관리하고 있기 떄문이다. 따라서 애플리케이션을 종료하려면 Deployment도 삭제 해줘야 한다.

## Module 5 - Scale up your app

대화형 튜토리얼은 [여기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/scale/scale-interactive/)에서 진행할 수 있다.

### Scaling a deployment

Deployment 목록을 확인하려면 다음 명령어를 실행해야 한다.

```shell
> kubectl get deployments
```

우리는 한 개의 파드가 필요하다. 그렇지 않다면 명령어를 다시 실행하자.  
이것은 다음과 같은 내용들을 보여준다.

- READY 열에는 CURRENT와 DESIRED 복제본의 비율이 표시됨
- CURRENT는 현재 실행중인 복제본 수이다
- DESIRED는 구성된 복제본 수이다
- UP-TO-DATE는 원하는(구성된) 상태와 일치하도록 업데이트 된 복제본 수이다
- AVALILABLE 상태는 실제로 사용자에게 사용 가능한 복제본 수를 나타낸다

다음으로 Deployment를 4개의 복제본으로 확장할 것이다.  
`kubectl scale` 명령어와 배표 유형, 이름 미 원하는 인스턴스 수를 사용한다.

```shell
> kubectl scale deployments/kubernetes-bootcamp --replicas=4
```

Deployment 목록을 다시 확인 하려면 다음 명령어를 사용한다.

```shell
> kubectl get deployments
```

변경 사랑이 적용되었으면 4개의 애플리케이션 인스턴스를 사용할 수 있음을 확인했다.  
이제 파드 개수가 변경되었는지 확인하자.

```shell
> kubectl get pods -o wide
```

서로 다른 IP를 가진 4개의 파드가 있음을 확인할 수 있다.  
변경 사랑이 Deployment 이벤트 로그에 등록되었따. 이를 확인하려면 다음 명령어를 사용한다.

```shell
> kubectl describe deployments/kubernetes-bootcamp
```

이제 4개의 복제본이 존재함을 학인 할 수 있다.

### Load Balancing

이제 서비스가 트래픽을 로드밸런싱 하고 있는지 확인하자.  
이전 모듈에서 배운 `describe` 명령어를 통해 노출 된 IP 및 포트를 찾을 수 있다.

```shell
> kubectl describe services/kubernetes-bootcamp
```

이제 노드 포트 값을 NODE_PORT 환경 변수에 저장하자.

```shell
> export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
> echo NODE_PORT=$NODE_PORT
```

이제 노출 된 IP 및 포트를 `curl` 명령어를 통해 적용한다.  
명령어를 여러번 실행해보자.

대부분의 경우에 매번 다른 파드가 요청을 받고 있음을 알 수 있다  
이는 로드밸런서가 정상적으로 작동하고 있음을 알게 해준다.

### Scale Down

방금 복제본을 늘렸으니 이번엔 복제본을 줄여보자. 여기에선 2개로 줄여보겠다.  
서비스를 2개의 복제본으로 줄이려면 `scale` 명령어를 다시 실행한다.

```shell
> kubectl scale deployments/kubernetes-bootcamp --replicas=2
```

좋다. 이제 `get deployment` 명령어를 이용해 변경 사항이 적용되었는지 확인해보자.

```shell
> kubectl get deployments
```

복제본 개수가 4개에서 2개로 변경되었음을 알 수있다.  
`get pods` 명령어를 이용해 파드 수를 확인해보자.

```shell
> kubectl get pods -o wide
```

이를 통해 2개의 파드가 종료된것을 알 수 있다.

## Module 6 - Update your app

대화형 튜토리얼은 [여기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/update/update-interactive/)에서 진행할 수 있다.

### Update the version of the app

Deployment 목록을 확인하려면 다음 명령어를 사용한다.

```shell
> kubectl get deployments
```

마찬가지로 실행중인 파드 목록을 확인하려면 다음 명령어를 사용한다.

```shell
> kubectl get pods
```

앱의 현재 이미지 버전을 확인하려면 파드에 대해 `describe` 명령어를 사용한다.(이미지 필드 참조)

```shell
> kubectl describe pods
```

애플리케이션 이미지를 version 2로 업데이트 하려면 다음 명령어를 사용하고 Deployment 이름과 새 이미지 버전을 사용해야한다.

```shell
> kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
```

이 명령어를 통해 Deployment에 앱이 다른 이미지를 사용하도록 알리고 롤링 업데이트를 시작했다.  
새 파드의 상태를 확인하고 `geet pods` 명령어로 종료되는 이전 파드를 확인하자.

```shell
> kubectl get pods
```

### Verify an update

첫 째로 앱시 실행 중인이 확인하라. 노출된 IP 및 포트를 찾으려면 `describe` 서비스를 사용할 수 있다.

```shell
> kubectl describe services/kubernetes-bootcamp
```

NODE_PORT 환경 변수에 Nodeport 값을 저장하자.

```shell
> export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
> echo NODE_PORT=$NODE_PORT
```

이제 다음으로 노출된 IP와 포트를 살펴보자. (여러번 실행해보자)

```shell
> curl $(minikube ip):$NODE_PORT
```

우리는 모든 요청에 대해 매번 다른 파드가 응답했으며, 모든 파드가 최신 버전(V2)를 사용하고 있음을 알 수 있다.

다음 명령어를 사용해 업데이트 상태를 확인할 수도 있다.

```shell
> kubectl rollout status deployments/kubernetes-bootcamp
```

앱의 현재 이미지 버전을 확인하고 싶다면 파드에 대해 `describe` 명령어를 사용하자.

```shell
> kubectl describe pods
```

이제 우리는 앱이 version 2를 사용하고 있음을 알 수 있다. (이미지 필드 참조)

### Rollback an update

이제 다른 업데이트를 수행하고 v10 태그가 지정된 이미지를 배포 해보자.

```shell
> kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10
deployment.extensions/kubernetes-bootcamp image updated
```

`get deployments` 를 사용하여 현재 deployment 상태를 확인하자.

```shell
> kubectl get deployments
```

그리고 뭔가 잘못되었음을 알수 있다.  
우리가 원하는 개수의 파드를 이용할 수 없다.  
파드 목록을 다시 확인해보자

```shell
> kubectl get pods
```

`describe` 명령어를 사용하면 파드에 대한 더 많은 정보를 얻을 수 있다.

```shell
> kubectl describe pods
```

Repository에 `v10` 이라는 이미지가 없는것을 확인할 수 있다.  
이전 작업으로 롤백해야 한다. 이를 위해 다음 명령어를 사용한다.

```shell
> kubectl rollout undo deployments/kubernetes-bootcamp
```

`rollout` 명령어는 dpeloyment를 이전 상태(v2의 이미지)로 되돌렸다.  
업데이트 버전이 지정되어 있으며 이전에 알려진 배포 상태로 되돌릴 수 있다.  
파드 목록을 다시 확인해보자.

```shell
> kubectl get pods
```

4개의 파드가 구동중이다. 그들에게 배포된 이미지를 다시 확인해보자.

```shell
> kubectl describe pods
```

Deployment가 안정적인 버전의 앱(v2)을 사용하고 있음을 알 수 있다.  
롤백에 성공한 것이다.

## End

쿠버네티스의 대화형 튜토리얼은 여기까지이다.

우리는 이 튜토리얼을 통해 다음과 같은 일을 할 수 있다.

- 쿠버네티스 클러스터 생성하기
- 애플리케이션 배포하기
- 앱 조사하기
- 앱 외부로 노출시키기
- 애플리케이션 스케일링하기
- 앱 업데이트하기

아직 이해가 되지 않는다면 [여기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/)의 공식문서를 참고하면 좋다.
