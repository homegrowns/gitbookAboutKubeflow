---
description: 쿠베플로우 포드의 어플리케이션(웹이나 DB)에 외부 접근하기위한 방법
---

# ssh 포드 포워딩

포드내 어플리케이션 접속을 위한 방법에 문의주신 내용에 답변 드립니다.

\
ssh를 이용한 포트포워딩을 통해 노트북에서 동작시킨 서버에 접근하실 수 있습니다.&#x20;

아래는 원격 서버에 간단한 웹 서버를 띄운 후 이를 로컬 PC에서 접속하는 예시입니다.

(사용자 예시: poby.hyeon)

### &#x20;case1. Web

1\. 원격 서버에서 서버 실행

$ ssh -p 32374 poby.hyeon@ssh-nipahpc.kubeflow.kakaocloud.com

poby.hyeon@ssh-nipahpc.kubeflow.kakaocloud.com's password:

\


\# 웹서버를 8000 포트에 실행

poby.hyeon@test-poby19-0:\~$ python3 -m http.server 8000

Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) …

\


2\. 원격 서버의 웹서버를 localhost:8080으로 포워딩

$ ssh -p 32374 -L 8080:localhost:8000 poby.hyeon@ssh-nipahpc.kubeflow.kakaocloud.com

poby.hyeon@ssh-nipahpc.kubeflow.kakaocloud.com's password:

\


3\. localshot:8080에 접속하여 웹서버 확인





### case2. DB

포드안에서 외부설치 ( apt-get 등등)를 한다. **도커컨테이너를 사용할수 없더라**..(포워딩이안되는듯듯)&#x20;

디폴트로 포트 설정이 될것이다. 그포트를 사용한다.

(# 웹서버를  꼭8000 포트에 실행할필요없다)

### ssh로 포워딩하기

클릭하우스 HTTP default port = 8123

ssh -f -N -p 30110 -L 8123:localhost:8123 henry.cho@ssh-nipahpc.kubeflow.kakaocloud.com



### 옵션 요약

#### 1. **`-f` (background)**

* **설명**: SSH 명령어가 백그라운드에서 실행되도록 설정하는 옵션입니다.
* **사용 목적**: 사용자가 직접적으로 명령어를 입력하는 쉘 세션을 사용하지 않고도 SSH 연결을 유지할 수 있게 합니다. 주로 **포트 포워딩**을 설정할 때 사용됩니다.
* **예시**: 명령어 실행 후, SSH 세션이 백그라운드로 이동하여 다른 작업을 바로 수행할 수 있습니다.

#### 2. **`-N` (no remote commands)**

* **설명**: 원격 서버에서 명령을 실행하지 않고, **포트 포워딩**만 수행하는 옵션입니다.
* **사용 목적**: SSH를 사용하여 명령어 실행 없이 단순히 터널링이나 포트 포워딩만 필요할 때 사용합니다.
* **예시**: 특정 포트를 로컬과 원격 서버 간에 연결할 때 사용되며, 원격에서 쉘 세션을 실행하지 않습니다.

#### 3. **`-p` (port)**

* **설명**: 원격 서버의 **SSH 포트 번호**를 지정하는 옵션입니다. 기본 SSH 포트는 `22`이지만, 다른 포트 번호를 사용할 경우 이 옵션을 통해 지정할 수 있습니다.
* **사용 목적**: 만약 SSH 서버가 기본 포트(`22`)가 아닌 다른 포트에서 실행 중이라면, 이 옵션으로 해당 포트 번호를 지정하여 연결할 수 있습니다.
* **예시**: `-p 2222`를 사용하면, SSH 서버가 2222번 포트에서 대기하고 있는 서버에 접속합니다
