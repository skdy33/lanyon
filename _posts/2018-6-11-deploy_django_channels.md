---
layout: post
title: django channels 배포
---

## 개요
현재 django 기반의, django channels를 이용하는, channel layer로 redis server를 이용하는 어플을 배포해야 할 때가 왔다. 어떻게 할까!

## 버전 스펙
```
django == 1.11.10
django channels == 2.1.1 
redis-stable == 
```

## redis 설치
* In memory db redis server로 channel layer 사용하니까, 일단 redis daemon을 띄워놔야 한다.
```
cd /tmp
curl -O http://download.redis.io/redis-stable.tar.gz
tar xzvf redis-stable.tar.gz
cd redis-stable
make
make test
sudo make install
```
`pip install asgi_redis`

### configure install
copy configure file, and change it
```
sudo mkdir /etc/redis
sudo cp /tmp/redis-stable/redis.conf /etc/redis
```
change configuration
```
supervised systemd
dir /var/lib/redis
```

### create directory and chown
```
sudo mkdir /var/lib/redis
sudo chown ubuntu /var/lib/redis
sudo chmod 770 /var/lib/redis
```

### Create a Redis systemd Unit File
in `/etc/systemd/system/redis.server`
```
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
User=ubuntu
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```

## openssl 적용
```
cd ~
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt
./letsencrypt-auto --help
./letsencrypt-auto certonly --standalone -d 유알엘.co.kr
```
* 키 빼오기 in `/etc/letsencrypt/live/유알엘/`

## 이거 안돼서 certbot으로 진행
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python-certbot-nginx
sudo certbot --nginx -d example.com -d www.example.com
```

## daphne ASGI protocol server
* Daphne is a HTTP, HTTP2 and WebSocket protocol server for ASGI and ASGI-HTTP, developed to power Django Channels
* 반드시 이걸 써야 하는지는 잘 모르겠지만, 일단 쓴다.
* in `myproject/asgi.py` (wsgi.py 있는 곳에)
```python
"""
ASGI entrypoint. Configures Django and then runs the application
defined in the ASGI_APPLICATION setting.
"""

import os
import django
from channels.routing import get_default_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "myproject.settings")
django.setup()
application = get_default_application()
```

## setting 바꾸기
* psycopg2 로 rds psql 사용하도록
* media url 추가

## nginx
```
sudo apt-get install nginx
```
```
server {
  listen 80;
  server_name linkey.co.kr;
  charset utf-8;
  client_max_body_size 20M;

  location /static {
    alias /home/ubuntu/ex_linky/static;
  }

  location /media {
    alias /home/ubuntu/ex_linky/media;
  }

  location / {
    proxy_pass http://0.0.0.0:8000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";

    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Host $server_name;
  }
}
```


## 참고자료
* [deploying django channels](https://channels.readthedocs.io/en/latest/deploying.html?highlight=redis)
* [config redis server](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)
* [certbot nginx](https://twpower.github.io/44-set-free-https-by-using-letsencrypt)
