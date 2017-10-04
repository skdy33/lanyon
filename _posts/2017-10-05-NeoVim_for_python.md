---
layout: post
title: Neovim 파이썬 에디터로 사용하기.
---

## 1. 코딩엔 어떤 에디터를...
우연찮게 RedHat 개발자의 강연을 듣게된 적이 있는데, <br>
**Java eclipse** 를 정말 기똥차게 사용하더라. <br>
그 때 알게되었다. <br>
에디터를 잘 쓰는 것이 얼마나 중요한가를. <br>

그렇다면 neovim을 파이썬 개발 에디터로 사용하는 것을 왜 선택하게 되었는가. <br>
* **coding ninja** 타이틀 *(vim 을 잘 쓰는 개발자를 칭하는 말)*
* vim의 철학에 공감 : 개발자가 키보드에서 손을 떼면 효율성이 떨어진다.
* 타 에디터들은 내 입맛에 맞게 수정하는 것이 번거로움.
* ssh로 접근하여 서버컴퓨터에서 코딩을 하기엔 vim을 반드시 알아야 함.
* 해피해킹 키보드 짱..

이러한 이유 때문에 **NeoVim** 을 python IDE로 선택한다. <br>

cf) 현재 용도에 따라 다양한 IDE를 사용하고 있다. (모델링 - *jupyter notebook*, 다양한 언어로 작업 - *atom*) <br>
종국에는 이 모든 작업들을 neovim으로 대체하길 희망한다. <br>

## 2. vim에 대한 조금 더 깊은 이해.
일단 여타 에디터처럼 vim(*이 아래로 neovim을 그냥 vim이라 쓰겠다.*)은
* 다른 파일들의 navigating이 쉬워야 하고,
* 코드 내에서 실행을 시켜볼 수도 있어야 할 것이며
* docstring 표현, auto-completion, syntax error detection도 쉬워야 한다. <br>

처음엔 이러한 기능들을 다만 plugin을 설치하면 가능할 줄 알았는데, <br>
그것보다 조금 더 근본적인 이해가 필요했다. <br>

아래를 읽고 나면 위의 기능들을 구현하기 위해 다음과 같은 내용들이 왜 필요했는가를 자연스레 알게된다. <br>

### 2.1 버퍼와 탭.. 그리고 윈도우
atom을 예로 들자. <br>
![아톰 에디터 그림](https://i.imgur.com/BRmWhtR.png) <br>
가장 왼쪽에서 파일을 누르면, 해당 파일에 대한 **탭** 이 열려 그 탭을 수정한다. <br>
그렇다면 vim에서 이런 상황과 마찬가지인 형태를 만들기 위해선 가장 먼저 vim 내에서의 **탭** 과 **버퍼** 를 이해해야만 한다. <br>

#### 버퍼란

```vim tmpfile``` 을 터미널에서 입력했다. 이를 통해 들어가는 파일은 <br>
*vim이 열어준 **버퍼**이다. * <br>
두 가지 관점의 해석을 제시한다. <br>
* 버퍼 = 편집중인 텍스트.
* vim으로 파일을 열면 vim의 버퍼로 내용이 로드된다.

이렇게 우리가 보게 되는 것이 vim은 atom 에디터와는 다르게, 탭이 아니라 버퍼이다. <br>

혹시 실수로 ```vim somefile otherfile``` 이런 식으로 vim을 열어본 적이 있으면, <br>
이제는 조금 이해할 수 있다. <br>
두 개의 버퍼가 열렸고, 이 사이는 ```:bnext```라는 커맨드로 돌아다닐 수 있다. <br>
![example](https://joshldavis.com/img/vim/buffers.gif) <br>
*두 개의 버퍼를 돌아다니는 예시* <br>

#### 윈도우란

조금만, 조금만 더 들어가보자. <br>
버퍼라는 vim이 가진 공간에 데이터가 잔뜩 들어있다. <br>
그럼 우리는 그 버퍼가 출력한 **전체 값**을 보고있는건데 <br>
이 보고있는 화면을 **윈도우**라고 부른다. <br>

위의 두 개의 버퍼를 돌아다니는 예시를 보자. <br>
버퍼는 두 갠데, 윈도우는 하나니까! 우리는 하나밖에 못보는거다. <br>
그럼 윈도우를 나누면, 버퍼에 있는 값들이 동시에 보이겠다! <br>
```:split```을 입력해보자. <br>
![split example](https://joshldavis.com/img/vim/windows.gif) <br>
*윈도우를 쪼개는 예시* <br>  

#### 탭이란

자 이제 **탭**만 알면 된다. <br>
[공식 문서에](http://vim.wikia.com/wiki/Using_tab_pages) 따르면
```
탭 : 탭은 윈도우의 집합이다.
```
라고 나와있다. <br>

아직 애매한게 당연히 이상한게 아니다. <br>
자, 저 위 그림에서 하나의 터미널에서 2개의 윈도우가 나오면, 그 두 개의 윈도우가 하나의 **탭** 이다. <br>

그렇다면 새로운 탭을 ```:tabnew```로 만들어보자. <br>
![multiple tabs example](https://i.imgur.com/VTDjCaX.png) <br>
터미널 왼쪽 위를 보면, 두 개의 파일이 마치 크롬에서 여러 창이 열리듯 나온걸 볼 수 있다. <br>
이는 ```:tabNext``` 를 통하여 tab을 바꿀 수 있다. <br>
이렇게 각 탭마다 다른 레이아웃과, 다른 용도를 띄워 사용할 수 있다. <br>

그렇다면 탭은 여러 개의 버퍼를 띄운 윈도우랑 똑같은 용도로도 사용할 수 있는데, 굳이 왜 만들었는가? <br>

[stack overflow 발췌](https://stackoverflow.com/questions/26708822/why-do-vim-experts-prefer-buffers-over-tabs) <br>
* **버퍼**를 각 파일을 추적하고 관리하는 용도로 사용한다.
* **탭**은 working space, 즉 여러 개의 project 단위를 동시에 수정해야 할 때 사용한다.

코딩 닌자를 꿈꾸는 우리들은 일단 하나의 working space에서 놀 일 밖에 없으니까, <br>
버퍼만 잘 만지자. <br>

### 2.2 CtrlP: 버퍼를 관리하기 위한 플러그인.
이제 하나의 탭 안에서 여러 버퍼를 잘 navigating해야 할 이유를 알게 되었다. <br>
그럼 이를 도와주는 플러그인이 CtrlP이다.

#### 설치부터.

설치는 [공식 홈페이지](http://ctrlpvim.github.io/ctrlp.vim/#installation)를 참고한다. <br>
```zsh
# vim 파일이 관리되는 곳에 일단 들어간다.
cd ~/.local/share/nvim
# 설치
git clone https://github.com/ctrlpvim/ctrlp.vim.git bundle/ctrlp.vim
```
이제 ```init.vim```에 추가한다. <br>
```zsh
vi ~/.config/nvim/init.vim
```
vim 안에서 플러그인을 추가한다. <br>
```vim
set runtimepath^=~/.config/nvim/bundle/ctrlp.vim
```
그 vim을 끄지말고, Vim의 커맨드라인에서 다음을 돌린다. <br>
```vim
:helptags ~/.config/nvim/bundle/ctrlp.vim/doc
```
이제 껏다키면 설치 끝!

#### 세팅과 사용 예

## 참고자료
* [Use Vim as a python IDE](http://liuchengxu.org/posts/use-vim-as-a-python-ide/) - vim을 파이썬 에디터로 만드는 방법.
* [Vim and python](https://www.fullstackpython.com/vim.html) - 파이썬과 vim의 사용에 대한 설명.
*  [python으로 만드는 neovim async plugin](https://astralhpi.github.io/pycon2016_program31/#1)
* [vim의 탭은 그렇게 쓰는 것이 아니다](https://bakyeono.net/post/2015-08-13-vim-tab-madness-translate.html) - CtrlP와 버퍼의 의미
* [vim 설정파일 알아보기](http://jaeheeship.github.io/console/2013/11/15/vimrc-configuration.html) - vimrc의 언어를 이해하기 위해 필요한 기초지식
