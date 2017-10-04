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

### 2.1 버퍼와 탭..
atom을 예로 들자. <br>
![아톰 에디터 그림](https://i.imgur.com/BRmWhtR.png)

## 참고자료
* [Use Vim as a python IDE](http://liuchengxu.org/posts/use-vim-as-a-python-ide/) - vim을 파이썬 에디터로 만드는 방법.
* [Vim and python](https://www.fullstackpython.com/vim.html) - 파이썬과 vim의 사용에 대한 설명.
* [python으로 만드는 neovim async plugin](https://astralhpi.github.io/pycon2016_program31/#1)
* [vim의 탭은 그렇게 쓰는 것이 아니다](https://bakyeono.net/post/2015-08-13-vim-tab-madness-translate.html) - CtrlP와 버퍼의 의미
* [vim 설정파일 알아보기](http://jaeheeship.github.io/console/2013/11/15/vimrc-configuration.html) - vimrc의 언어를 이해하기 위해 필요한 기초지식
