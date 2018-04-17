---
layout: post
title: Neovim2, auto complete 그리고 소소한 단축키 설정.
---

## 1. 잠깐 neovim을 써보면서...
자꾸만 손이 atom 으로 가게된다... <br>

아무래도 여러 이유가 있지만, <br>
1. md를 쓸 때는 syntax understanding이 neovim이 오류가 많다.
2. python을 쓸 때는 autocomplete이 없으니까 전혀 쓸 수가 없다.
3. 그리고 buffer, window가 손에 익지가 않는다.

그래서 가장 먼저 buffer, window를 손에 익도록 해보자. <br>

## 2. 단축키 외우기.

일단 단순하게 작업과정은 다음과 같다 : <br>

1. vsplit을 통해 윈도우 두 개를 킨다.
	* 사실 터미널 두 개를 켜서 작업을 해도 전혀 상관은 없다.
	* 근데 사실 이렇게 하는 이유는, 사이즈 변환이나 추가적인 split이 편하기 때문이다. 
2. 윈도우 사이즈를 원하는대로 조정한다.
3. 오른쪽에서 확인하면서 작성해야할 파일을 띄워놓고, 
4. 맘껏 다른 파일을 키고, 버퍼를 돌아다니며 작업한다. 

### 2.1 윈도우 키는 방법
* ```vsplit``` vsplit은 윈도우를 세로로 쪼갠다.
* ```split``` 의 경우에는 가로로 버퍼를 쪼갠다.

### 2.2 윈도우 사이즈 조정
* 기본적으로 상, 하는 ```:res +-5``` 이런식으로 사이즈를 조정한다.
	* 단축키는 <Ctrl> + w 하고 + 하면 늘어나고, - 하면 줄어든다. 
	* maximum height로 하기 위해선 <Ctrl> + w, 그리고 _ 하면 된다. 
* 기본적으로 좌, 우는 ```:vertical res +-5``` 이런식으로 사이즈를 조정한다.  
	* 단축키는 <Ctrl> + w 하고 > 하면 늘어나고, - 하면 줄어든다.
	* Maximum width를 가지려면 <Ctrl> + w 하고 | 하면 된다. 

### 2.3 작성해야 할 파일 띄워놓기
* 일단 윈도우를 돌아다니려면 <Ctrl> + w, 그리고 w를 통해서 윈도우를 돌아다닐 수 있다. 
* 그러면 바꾸고, 우리가 설치했던 CtrlP를 통해서 원하는 파일을 키자!!!!

### 2.4 알아야 할 것들
* w, b, 컴퓨터 버퍼을 이용한 복붙은 Ctrl + Shift + c(or v),
* 드래그는 v, y 가 copy, p 가 paste (이건 컴퓨터가 아닌, vim buffer)

## 3. 이제 auto complete
YCM(You complete me, [jedi](https://github.com/davidhalter/jedi) based)이라는 code completion engine을 통해서 python tab code completion을 구현해야 한다. <br>
하나하나 차근차근 해보자. <br>

### 3.1 python support neovim
```:echo has('python') || has('python3')``` 쳐보자. <br>
쓰잘때기없이 더 멋있는거 쓰자고 neovim을 썼더니 neovim이 python support가 안되는 것 같다. <br>

일단 이걸 위해서 [다음 자료]()를 확인하자... <br>

그리고 일단 ```:help provider-python``` 를 쳐서 정독하자. <br>
* ```:CheckHealth``` 를 쳐보면 python이 지원 되는가 안되는가를 알 수 있다.
* conda 가상환경 하나 만들어 놓고(neovim에서 쓸) 거기다가 neovim 연결해야 한다.
	* ```sudo pip3 install --upgrade neovim``` 으로 해당 가상환경에 neovim 설치해놓고
	* ```~/.config/nvim/init.vim```
	* init.vim 에 가상환경 위치 추가. ```let g:python3_host_prog = '/home/mappiness/anaconda3/envs/torch/bin/python'```
	* 이제 neovim은 파이썬을 인식한다.

### 3.2 Install YCM with vundle
아... plugin을 **vundle** 로 설치했던 적	

## 참고자료
* [Resize splits more quickly](http://vim.wikia.com/wiki/Resize_splits_more_quickly)
* [You Complete Me](https://github.com/Valloric/YouCompleteMe)

autocomplete - YCM. 민상이한테 깔아둠

ycm을 설치하는데 생각보다 오래걸리네
* local share 에 git clone 하고
* 그 다음 init.vim 에서 '''PlugInstall'''하고
* 근데도 안되네
그래서 ycm_build를 ~에 제작

알고보니 plug 시작에 안넣었었네 거기에 넣고
이제 nvim for python 설치해야하네
failed to load python command
python path를 안에 anaconda로 넣어줬고
그 다음에 core compile ycm_build
또 안돼서 install.sh 
그래도 안돼서 껐다 키고
위치가 다른가 해서 이번엔 bundle 말고 plugged에서 해봄
근데 뭔가가 없어서 설치함
GLIBCXX_3.4.20
The following solution works for me:

Follow these steps to solve the issue:

go to the right location and backup your current anaconda2 shortcut (change its name so it isn't overwritten):

	cd ~/anaconda2/lib
	mv -vf libstdc++.so.6 libstdc++.so.6.old

	create a new shortcut using the ln command (I am assuming that I am in the previous location ~/anaconda2/lib):

		ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6 ./libstdc++.so.6

		restart spyder / other interface you use
https://github.com/CauldronDevelopmentLLC/CAMotics/issues/178


