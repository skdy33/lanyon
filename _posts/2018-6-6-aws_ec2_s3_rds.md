---
layout: post
title: aws ec2, s3, rds로 배포
---

## 1. ec2..
사실 rds랑 s3만으로 배포가 되면 얼마나 좋을까. <br>
API server 또한 ec2로 올리면서, 3배로 돈이 나가는 상황을 어쩔 수 없이 부딪쳐야만 한다. <br>
어짜피 deploy는 `django-rest-framework`, `nginx`, `uWSGI` 로 진행을 하기 때문에 <br>

rds, ec2, s3로 진행해본다. <br>

## 2. s3를 ec2에 붙이기!
* `s3fs` 를 
* ec2 주소 : 13.125.207.33
1. `sudo apt-get update`
2. install dependency
`sudo apt-get install automake autotools-dev fuse g++ git libcurl4-gnutls-dev libfuse-dev libssl-dev libxml2-dev make pkg-config`
3. 그 다음에는 filesystem s3fs를 사용해야 한다. 소스코드 다운!
`git clone https://github.com/s3fs-fuse/s3fs-fuse.git`
4. 컴파일!!
```
cd s3fs-fuse
./autogen.sh
./configure --prefix=/usr --with-openssl
make
sudo make install
```
5. 체크!
`which s3fs`

## 3. S3 만지기.
* s3는 일단 그냥 만들기만

## 4. IAM 그룹에 사용자 만들기
* S3에 접근 권한이 있는 사용자를 만든다.
* I need my access key and secret key of the user 일단 어따가 적어놓자.

## 5. 비밀번호 파일 제작
* ec2 안 `/etc/passwd-s3fs`에다가 
`accesskey:secretkey` 형태로 파일 제작
`sudo chmod 640 /etc/passwd-s3fs`

## 6. mount!
```
sudo mkdir /s3bucket
sudo s3fs your_bucketname -o use_cache=/tmp -o allow_other -o uid=유아이디 -o mp_umask=002 -o multireq_max=5 /mys3bucket
```
* 여기서 your_bucketname 은 bucket이름 가져오고
* 위 코드 돌리기 전에 `/etc/fuse.conf` 에서 `user_allow_other` 설정 해주시고
* `id -u <username>` 으로 s3 bucket 쓸 username 가져오고 그거 uid 뒤에 붙이고
* 사용!

## 7. auto mount after reboot
1. 외운다.
`which s3fs`
2. 연다
`sudo vi /etc/rc.local`
3. 넣는다.
`외운거 your_bucketname -o use_cache=/tmp -o allow_other -o uid=유아이디 -o mp_umask=002 -o multireq_max=5 /mys3bucket`

## 문제
* 아이씨..... rds는 일단 다른 아이디에 붙여서 실험하고....
 
## 참고자료
* [Mount s3 on ec2](https://cloudkul.com/blog/mounting-s3-bucket-linux-ec2-instance/)
