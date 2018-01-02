---
layout: post
title: Jupyterhub 설치
---

## 1. 이유
학습용 워크스테이션은 나날이 좋아지는데, 다만 ssh로 접근해서 돌리기에는 뭔가 아깝다는 생각이 들었다. <br>
아싸리 전처리, 모델링 과정 자체가 본 워크스테이션에서 일어난다면 보다 빠른 작업을 할 수 있을텐데, 하면서 <br>
방법을 찾아보다 생각한게 **Jupyterhub**. <br>

목적은 다음과 같다. <br>
1. jupyter notebook을 어디서든 워크스테이션 서버로 접근
2. 일단은 하나의 프로젝트 *현재는 음성인식 kaggle* 만을 위한 jupyterhub.
3. 굳이 다중 작업이 필요하지 않고, 확장성또한 여기서 핸들링 할 필요 없다.

사실 현재 docker공부도 하고 있어서 docker로 하면 좀 더 편할텐데... <br>
일단은 그냥 진행해보고 docker로도 시간이 나면 해보자.


## 2. 설치 과정 설명

설치 과정은 다음과 같다 :

1. jupyterhub 설치
2. LetsEncrypt를 통해 SSL 발급 받고, jupyterhub에 적용
3. nginx 웹서버를 사용하여, 80(http) 포트를 열어놓고 443(https)으로 리디렉트.
  * 아, 그니까 https로 하기 위해서 ssl을 발급받고 하는건데, 외부에서 http로 접근해도 https로 리디렉트해서 열 수 있도록 하기 위해서 하는거구나.

### 2.1 도메인 네임

아, 그리고 **jupyterhub** 의 경우에는 반드시 SSL 프로토콜로만 접근이 가능하기 때문에, <br>
SSL를 발급받기 위해서 도메인 네임이 있어야 한다. <br>

가비아에서 사서 설정한다.

방법은 다음과 같다 :
1. 도메인 네임 구매! 필자의 도메인 네임은 mappiness.co.kr 이다.
2. 도메인 네임 a 레코드 설정 : a 레코드를 본 컴퓨터의 WAN에 연결한다.
  1. WAN의 경우에는 192.168.0.1에서 확인할 수 있다.
  2. a 레코드를 설정하는 것은 귀찮으니까, 그냥 가비아 홈페이지 열심히 뒤져보면 나온다!
  3. 그리고 192.168.0.1에서 포트 포워딩을 해야한다.
    * 보통 443, 80 둘다 ifconfig에 나오는 inet iddr에 하면 되는데 본 컴퓨터는 이미 443 port를 쓰고 있으니까, 8002로 해보자.


## 3. 사전 정보

### 3.1 주피터 헙

주피터헙은 3가지로 이루어져 있다. <br>
1. hub : 사용자 정보, notebook을 관리하는 app
2. proxy : 사용자 정보를 통하여 사용자와 jupyter notebook을 연결해주는 어플리케이션
3. Single User Notebook: 하나의 계정별로 운영되는 하나의 Jupyter Notebook

### 3.2 HTTPS 와 SSL
* HTTPS : 보안이 뛰어난 HTML을 전송하기 위한 통신규약(프로토콜). 암호화가 되어있다. <br>
* SSL : 이 또한 인터넷 상에서 정보를 암호화하여 송/수신하는 프로토콜. <br>
여기서 둘의 관계는 **HTTPS 는 SSL 위에서 돌아가는 프로토콜** 이란 점. <br>

### 3.3 SSL 인증서
이는 클라이언트와 서버 간의 통신을 제 3자가 보증해주는 전자화된 문서이다. <br>
클라이언트가 서버에 접속하면, 서버는 클라이언트에 이 인증서 정보를 전달한다. <br>
이를 클라이언트가 확인함으로, 서버가 신뢰할 수 있는 서버인지를 알 수 있다. <br>

추가적인 SSL에 대한 설명은 아래 [생활코딩](https://opentutorials.org/course/228/4894)을 참고하도록 하자.

## 4. 설치 및 구축 시작

### 4.0 배포 환경 확인
일단 가상환경에서 설치한다. <br>
필자는 conda 가상환경을 사용하며, 이름은 torch이다. <br>
이 부분은 알아서 하도록. <br>

jupyterhub은 node.js, python에 대한 의존성이 있기 때문에 먼저 환경을 확인한다. <br>
참고로, 필자의 배포 환경은 다음과 같다 :

```bash
(torch) ❯ conda -V
conda 4.3.25
(torch) ❯ python -V
Python 3.6.2 :: Continuum Analytics, Inc.
(torch) ❯ node -v
v4.2.6
```


### 4.1 jupyterhub 설치

```bash
source activate torch # torch 가상환경 실행
pip install jupyterhub
```

### 4.2 configuration 시작
일단, jupyterhub의 기존 설정파일을 만들어야 한다. <br>

```bash
mkdir kaggle # 캐글 폴더에서 진행
cd kaggle
jupyterhub --generate-config
```

그러면 다음과 같은 파일들이 생성된다. <br>
```bash
(torch) ❯ tree .
.
├── jupyterhub_config.py
├── jupyterhub_cookie_secret
└── jupyterhub.sqlite
```

이제 ```jupyterhub_config.py``` 파일 내의 몇 가지를 수정하자.
```python
c.JupyterHub.ip = '0.0.0.0'  
c.JupyterHub.port = 8002 # 나는 8002로 https port를 하기로 하였으니까!!!  
```

### 4.3 port 방화벽 설정
일단, 필자는 ubuntu 16.04 환경에서 이를 진행하고 있으므로 <br>
컴퓨터의 포트 방화벽을 ```ufw```로 관리한다. <br>
8002 포트를 열도록 한다. <br>

```bash
sudo ufw allow 8002
```
ufw 로 하고 있지 않는 사람들은, 컴퓨터에 포트 여는 법을 찾아 알아서 열 것. <br>

### 4.4 SSL 적용
Let's encrypt 는 공짜 AC 이다. 그래서 여기서 SSL 인증서를 다운받아 사용하도록 한다!! <br>
* 어느 폴더에서 하던 상관 없음
```bash
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt
./letsencrypt-auto --help
./letsencrypt-auto certonly --standalone -d mappiness.co.kr
```
이제 개인 키 파일과, 인증 키 파일은
```bash
/etc/letsencrypt/live/mappiness.co.kr/privkey.pem # 개인키
/etc/letsencrypt/live/mappiness.co.kr/fullchain.pem # 인증키
```
에 저장된다. <br>
이 키를 이제 작업 폴더에 카피하자. 이거는 알아서. <br>

이제 ```jupyterhub_config.py``` 에 이를 적용한다!
```python
c.JupyterHub.ssl_cert = '/복사한/위치/fullchain.pem'  
c.JupyterHub.ssl_key = '/복사한/위치/privkey.pem'  
```

## 5. 에러 디버깅
* 포트가 열려있으면 닫아야 한다....
* jupyter 자체를 업글해주면 500 error가 해결된다.

## 참고자료
* [jupyterhub 환경 구축](https://ansuchan.com/gettings-started-with-jupyterhub/)
* [HTTPS와 SSL 인증서](https://opentutorials.org/course/228/4894)
