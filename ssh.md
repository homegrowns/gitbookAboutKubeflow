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

(# 웹서버를 8000 포트에 실행할필요없다)

## 영구적으로포워딩하기

클릭하우스 HTTP default port = 8123

ssh -f -N -p 30110 -L 8123:localhost:8123 henry.cho@ssh-nipahpc.kubeflow.kakaocloud.com

