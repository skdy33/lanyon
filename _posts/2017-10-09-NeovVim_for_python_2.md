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
