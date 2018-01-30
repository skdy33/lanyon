---
layout: post
title: 로컬 django project를 dockerizing 하는 법.
---

# Dockerizing django project

## 1. 왜?

사실 docker project에 붙어서 django를 개발하면 배포할 때 완벽한 상황이지만, <br>
속도가 생명인 작금의 상황에 배포자랑 앱 개발자는 당연히 나눠져 있지 않는가. <br>

나는 장고 개발자가 만든 app을 dockerizing 하여 배포해보도록 한다. <br>

## 2. 환경

### 2.1 로컬 개발 환경
* OS : ubuntu 16.04
* django : 1.11.9
* python : 3.6
* virtualenv : 파이참에서 자체적으로 설정하는데 뭔지 잘 모르겠다.
* DB : docker PostgreSQL

### 2.2 배포 환경
* OS : ubuntu 16.04
* django : 1.11.9
* python : 3.6
* virtualenv : docker dude
* DB : HDD

## 3. docker postgres 셋업
* postgres 버전 10이 나왔지만, 너무 최근이므로 일단 9.6 버전으로 한다.
* 별도로 한글을 처리할 필요 없다.

### 3.1 먼저 Dockerfile 제작
```bash
FROM postgres:9.6
ENV POSTGRES_USER 'psql 유저 이름'
ENV POSTGRES_PASSWORD 'psql 유저 비밀번호'
ENV POSTGRES_DB 'psql DB 이름'
```

### 3.2 run container
```bash
# Image build
# Dockerfile 위치 가서
docker image build -t '이미지 이름' .

docker container run -p 5432:5432 --name 'container 이름' -d '이미지 이름'

sudo ufw allow 5432 # 포트 열기.
```

## 4. 간단한 django app 만들기

### 4.1 project, app 만들기.

* ```pip install Django==1.11.9```
```bash
django-admin startproject pdf #그냥 pdf라는 프로젝트 이름으로 만든다.
python manage.py startapp parse # parse라는 앱 이름으로 만든다!
```

### 4.2 setting 설정
* INSTALLED_APPS에  parse 추가
* DATABASE configuration
  - ```pip install psycopg2```
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'db_name',                      
        'USER': 'db_user',
        'PASSWORD': 'db_user_password',
        'HOST': '127.0.0.1', # 이건 docker deploy 하면 docker 이름이 되어야 한다.
        'PORT': '5432',
    }
}
```

### 4.3 테스트용 model, view, template 만들기.
* **models.py**
```python
from django.db import models

# Create your models here.
class Post(models.Model):
    author = models.ForeignKey('auth.User')
    title = models.CharField(max_length=200)
    text = models.TextField()
    published_date = models.DateTimeField( blank = True, null = True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
```
* **admin.py**
```python
from .models import Post
# Register your models here.
admin.site.register(Post)
```
* **pdf/urls.py**
```python
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'', include('parse.urls')),
]
```
* **parse/urls.py**
```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.post_list, name='post_list'),
]
```
* **parse/views.py**
```python
# Create your views here.
from .models import Post

def post_list(request):
  posts = Post.objects.order_by('published_date')
  return render(request, 'parse/post_list.html', {'posts':posts})
```

* **parse/templates/parse/post_list.html**
```html
<html>
    <head>
        <title>상허니</title>
    </head>
    <body>
      {% for post in posts %}
          <div>
              <p>published: {{ post.published_date }}</p>
              <h1><a href="">{{ post.title }}</a></h1>
              <p>{{ post.text|linebreaksbr }}</p>
          </div>
      {% endfor %}
    </body>
</html>
```

완성.
### 이제 2부에서, 해당 django 의 dockerizing을 다룬다.

### 고려사항
* HDD 하나에 웹사이트 데이터 다 올려놓으면, 백업은?
  - volume으로 외부에 붙여야 한다.
* 대쉬보드 어케만들어
* QQQ nginx 와 django는 어떠한 관계에 있는거지?
* wsgi란? (gunicorn) - 미들웨어.
* window 8으로 개발한걸 바로 가져올 수 있을까.

### 참고자료
* [django  + docker](http://ruddra.com/2016/08/14/docker-django-nginx-postgres/index.html)
* [python django engine psycopg](https://stackoverflow.com/questions/5394331/how-to-setup-postgresql-database-in-django)
