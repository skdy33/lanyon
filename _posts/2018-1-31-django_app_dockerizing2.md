---
layout: post
title: 로컬 django project를 dockerizing 하는 법.
---

## 개요
목적과 사전 준비들은 전부 전편에서 다룬다. <br>

이미 django project가 있으신 분은 전편에서 PostgresSQL docker를 만들고 오세용! <br>

## 0. django docker 스펙.
* django version: 1.10.4-python3 (1.11.9 가 없어..)
* ubuntu : 16.04
* Docker version : 17.12.0-ce, build c97c6d6

## 1. make Dockerfile

## 2. make requirements.txt

## 3. docker-compose.yml

### 참고자료
* [Dockerize your django app](https://runnable.com/docker/python/dockerize-your-django-application)
* [Packaging Django applications into Docker container images](http://michal.karzynski.pl/blog/2015/04/19/packaging-django-applications-as-docker-container-images/)
