---
layout: post
title: DB modeler **pgmodeler** RDS에 연동
---

## 1. DB modeler란?
특히 DB를 설계할 떄 table이 많아질수록 GUI에 대한 목마름은 절박하다. <br>

어떤 테이블이 어떤 테이블과 연관이 있는가는 사실 GUI로만 직관적으로 이해할 수 있는 부분이라 생각한다. <br>

필자가 캐나다에서 작업을 했을 때는 MySQL을 local workbench에서 작업을 하고 <br>

이를 서버에 dump하는 형태로 하였는데, <br>

이젠 RDS를 사용하지 않는가? <br>
그리고 PostgreSQL을 사용하지 않는가? ㅠㅠ <br>

그렇다면 PostgreSQL 테이블을 잘 관리할 수 있는 DB modeler 중, <br>
가장 구글의 검색엔진 위에뜨는(구글 짱) **pgmodeler** 를 사용해보기로 한다. <br>

대충 검색해보니 RDS에서도 잘 연동이 된다고 한다. <br>

이제 시작하자. <br>

## 2. 설치

설치는 [해당 documentation](https://www.simonholywell.com/post/2016/10/install-pgmodeler-ubuntu/)을 참고한다. <br> 

일단 작업환경을 명시한다.
```zsh
╰─$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.2 LTS
Release:	16.04
Codename:	xenial
```

### 2.1 Prilims
pgmodeler app 은 **QT** based 이다. <br>
따라서 qt를 먼저 설치를 완료해야 한다. <br>

가장 먼저, build tools 를 설치한다. <br> 
```zsh
install libxml2-dev qttools5-dev-tools clang qt5-image-formats-plugins libqt5svg5
```

그 다음, **QT UI toolkit** 을 설치한다. <br>
```zsh
sudo add-apt-repository ppa:ubuntu-sdk-team/ppa && sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get install ubuntu-sdk
```

이제 **QT framework** 설치 <br>
```zsh
sudo apt-get install qt5-default
```

그 다음은, PostgreSQL 의 C 언어 인터페이스인 **libpq** 를 설치한다. <br>
아마 pgmodeler 는 postgresql과 C언어로 통신하는가 보다. <br>
```zsh
sudo apt-get install libpq-dev
```

### 2.2 설치!
설치는 매우 간단하다. <br>
```zsh
sudo apt-get install pgmodeler
```

끝. 

## 사용
터미널에서
```
 pgmodeler
```
타이핑 하면 바로 나온다. <br>

이제 settings 에서 connections 에서 rds로 connect하면 끝! <br>

근데 supported postgreSQL 버전이 9.0에서 9.4 사이란다... <br>

다시 RDS를 새로 구성해서 시작해야겠
다
