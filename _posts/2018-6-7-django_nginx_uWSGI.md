---
layout: post
title: django, nginx, uWSGI로 배포
---

## 1. django 배포 in aws.....
저번 포스팅에서 ec2, rds, s3 셋업했으니, 이제 django rest framework를 배포해보자. <br>

## 2. 설치
* anaconda
```
wget https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh
bash Anaconda-latest-Linux-x86_64.sh
export PATH=~/anaconda3/bin:$PATH
```
* virtual environment
```
conda create --name django
source activate django
```
* django & else
```
pip install django==1.11.5
pip install djangorestframework
```
* uWSGI
```
pip install uwsgi
```
* nginx
`sudo apt-get install nginx`
* firewall 은 알아서!
* python modules
```
pip install django-cors-headers
pip install django-filter
sudo apt-get install build-dep python-psycopg2
pip install psycopg2
pip install webvtt-py
```

## 결국 uwsgi 안깔려서 gunicorn으로 함
* (이거)[https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-16-04] 그냥 따라함.
## debugging
* unsupported locale setting
`export LC_ALL=C`
* lto-wrapper failed
`conda install gxx_linux-64`
## 참고자료
* [Nginx uWSGI Django 연결하기](https://twpower.github.io/41-connect-nginx-uwsgi-django)
* [unsupported locale setting](https://stackoverflow.com/questions/36394101/pip-install-locale-error-unsupported-locale-setting)
* [conda env](https://www.anaconda.com/download/#linux)
