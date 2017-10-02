---
layout: post
title: zsh 쉘 튜토리얼(1)
---

## Introduction

**효율성** 보다 개발자에게 중요한 단어가 있을까. <br>
그 효율성을 위하여 별의별 기상천외한 짓을 다 한다. <br>
단적인 예로는 :
* 손에 맞는 디바이스
* 개발 환경 커스터마이징
* IDE 커스터마이징

나는 왜 그러한 시간을 쓰지 않은걸까? <br>
 마냥 알고리즘의 해석과 어플리케이션의 제작에만 시간을 쓰다가, 문득 이러한 생각이 들었다. <br>

최적화를 위한 첫 번째 스텝. <br>
이제 막 개발을 마음먹은 학생의 효율성 확장 과정. <br>

첫 번째 시작은, **bash shell**의 진화판, **zsh shell** 로 하고자 한다.

## what is zsh

처음 과감히 윈도우를 밀어버리고, 우분투를 깔았을 때가 생각이 난다. <br>
여러 모듈 설치 및 dependency 관리가 용이하고, 가볍고 빠르고 공짜니까, 이런 이유가 있었지만 <br>
*bash에서 작업하는 그 **간지** <br>*
*vim에서 코딩하는 그 **간지** <br>*
사실은 이게 전부가 아녔을까. <br>

두개를 만지다 보니, shell에 대한 욕망이 점점 더 생겨났다. <br>
* navigating을 더 편하게 할 순 없을까? <br>
* alias를 쉽게 나에게 더 최적화시킬 순 없을까? <br>
* mac만큼이나 간지나는 shell을 사용할 순 없을까? <br>

그러다가 찾아낸 것이 바로 bash shell의 진화판, **zsh shell** 이다. <br>

## Why zsh

공식 [FAQ](http://zsh.sourceforge.net/FAQ/zshfaq01.html#l4)에서는 zsh의 장점을 많이 다루지만, 내가 zsh를 선택한 기준은 3가지로 나눌 수 있다. <br>
* shell 중 가장 많은 기능을 담고있으며, 지금도 계속해서 개발중인 shell
* regex가 매우 강력하여, 대충 tab 눌러도 완성되는 auto-completion.
* 오픈소스 프로젝트 [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) 프레임워크를 통한 환경설정 관리의 간편함. <br>

이 세가지 이유로, 나는 **zsh shell** 을 파겠다. <br>

zsh 쉘 튜토리얼(1) 에서는, **zsh** 와 **oh-my-zsh** 설치 방법만을 다룬다. <br>

## 설치 시작

### 작업 환경
```zsh  
❯ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.2 LTS
Release:	16.04
Codename:	xenial
```
```zsh
❯ zsh --version
zsh 5.1.1 (x86_64-ubuntu-linux-gnu)
```

### zsh 설치
일단 zsh 를 설치한다.
``` zsh
sudo apt-get install zsh
```
그 후, 기본 쉘을 zsh로 바꾼다.
``` zsh
# zsh shell 실행파일 위치 확인
❯ which zsh
/usr/bin/zsh

# default shell을 zsh로 바꾼 후, 컴퓨터를 리부팅한다.
❯ chsh -s /usr/bin/zsh
```

### oh-my-zsh 설치
```zsh
❯ curl -L  https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

### z 설치
z 는 나의 방문기록을 저장해주는 툴이다. <br>
바탕화면 안에, 2017년 폴더 안에, 강의명 안에, 해당 숙제 폴더를 들어가기 위해 얼마나 비효율적인 프로세스를 거치는가. <br>
이를 최적화하는 수단이 **z** 이다. <br>


1. Download ```wget https://raw.githubusercontent.com/rupa/z/master/z.sh```
2. Run printf ```"\n\n#initialize Z (https://github.com/rupa/z) \n. ~/z.sh \n\n" >> .zshrc.``` <br>
 This command appends . ~/z.sh to .zshrc file, which tells it to run Z on start-up.
3. ```source ~/.zshrc```

이제 자주가는 페이지에 방문을 하고, 다시 ```~/``` 에 돌아와서 ```z somthing + tab``` 을 눌러보자. <br>
쩐다.



*벌써 설치가 끝났다!!*

## 환경설정

강력한 기능에 대한 공부나, optimization의 경우는 (2)탄부터 천천히 다루도록 하겠다. <br>
오늘은 순전히 **간지** 를 위한, 테마 변경과 설정 약간을 바꿔주고 끝마친다. <br>

### 테마 변경
테마는 ```~/.zshrc``` 에서 변경한다. <br>
사람들마다 좋아하는 테마 기호가 참 많이 다를 것이다. <br>
오픈소스 프로젝트답게 별 쓸데없는 기능이 다 들어있는데, <br>
그 중 내게 도움을 줬던건 바로 "random theme" 기능이다. <br>
즉, 터미널을 킬 때마다 새로운 테마로 열려, 확고한 기호가 나오기 전 까지 여러가지를 경험해 볼 수 있다. <br>
```zsh
vi ~/.zshrc
```
이제 ZSH_THEME 변수의 값을 바꿔준다.
```vim
ZSH_THEME="random"
```
필자는 오랜 끝에 ```ZSH_THEME="refined"``` 를 가장 선호하게 되었다. <br>
참조 링크에 여러 추천 테마도 있으니 여러 개를 시도해보길 바란다.

## 참조 링크

* [터미널 초보의 필수품인 oh my zsh! 를 사용하자](https://nolboo.kim/blog/2015/08/21/oh-my-zsh/) - zsh와 oh my zsh에 대한 간략한 소개와 설치법. <br>
* [What is your favorite zsh theme](https://www.quora.com/What-is-your-favorite-oh-my-zsh-theme) - 사람들이 즐겨 사용하는 테마들을 얘기한 자리.
