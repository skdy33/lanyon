---
layout: post
title: authentication and permission of django rest framework
---

## 0. 목적.
* 일단 user authentication을 하고, 
* authentication이 된 상태에서 해당하는 파일들만 보여줘야 한다.
* backend
	* id, pwd 입력 시 토큰 발행 view
* frontend
	* 로그인페이지
	* localStorage에 토큰 삽입
	* 토큰 없으면 리디렉션
	* Song에만 토큰 삽입해도 ㄱㅊㄱㅊ
## 1. User model api에 삽입
* ```django.contrib.auth.models``` 에 있는 기본 유저모델을 사용한다.
* ```settings.py```
	* ```INSTALLED_APPS``` 에 ```rest_framework.authtoken``` 추가. 
	* settings 를 바꾸면 언제나처럼 ```python manage.py migrate``` run
	* REST_FRAMEWORK에
	```
	'DEFAULT_AUTHENTICATION_CLASSES': (
	   'rest_framework.authentication.TokenAuthentication',
	   )
	```
	추가
* NGINX : 현재 django를 서빙하고 있는 부분에 uwsgi pass request headers on 을 넣어야 한다.
	* in nginx.conf
		```
		uwsgi_pass_header Authorization;
		uwsgi_pass_request_headers on;
		```
* views :
```
class UserViewSet(viewsets.ModelViewSet):
    """
    A viewset for viewing and editing user instances.
    """
    serializer_class = UserSerializer
    queryset = User.objects.all()
    filter_fields= ('username',)
    lookup_field='username'

    def get_permissions(self):
    	if self.action == 'list':
    	    permission_classes = [IsAuthenticated]
    	else:
    	    permission_classes = [IsAdminUser]
    	return [permission() for permission in permission_classes]

```
* serializers
```
class UserSerializer(serializers.ModelSerializer):
    email = serializers.EmailField(
            required=True,
            validators=[UniqueValidator(queryset=User.objects.all())]
            )
    username = serializers.CharField(
            validators=[UniqueValidator(queryset=User.objects.all())]
            )
    password = serializers.CharField(min_length=8)

    def create(self, validated_data):
        user = User.objects.create_user(validated_data['username'], validated_data['email'],
             validated_data['password'])
        return user

    class Meta:
        model = User
        fields = ('id', 'username', 'email', 'password')
```
* signal
**ps) 기존에 있는 User들은 token을 다 알아서 추가해버림.**
```
from django.db.models.signals import post_save
		from django.dispatch import receiver
			  from django.conf import settings
				 from rest_framework.authtoken.models import Token

					@receiver(post_save, sender=settings.AUTH_USER_MODEL)
	def create_auth_token(a, instance=None, created=False, **kwargs):
		    if created:
			        Token.objects.create(user=instance)
```
* apps.py
```
from django.apps import AppConfig


class FileapiConfig(AppConfig):
    name = 'fileapi'
    def ready(self):
        import fileapi.signals
```
* app 의 init
```
default_app_config = 'fileapi.apps.FileapiConfig'
```

* url
```
router.register(r'users',views.UserViewSet,base_name='user')
url(r'^api-token-auth/', auth_view.obtain_auth_token)
```

### 토큰 세팅 완료

## 2. 모델 약간 변환

### 2.1 Song에 User 모델 넣기
```
fingerings = models.ForeignKey(settings.AUTH_USER_MODEL,blank=False, null=False,default=PK_OF_SUPERUSER)
``` 
해당 문장 추가. <br>
* 근데 문제는 여기서 추가를 할 때 model을 한번 다 지우고 다시 깔아야한다.... 

## Debugging
1. 'user-detail' not defined. <br>
이 부분은 [hyperlinkedmodelserializer](http://www.django-rest-framework.org/api-guide/serializers/#hyperlinkedmodelserializer)를 사용해서 나온 버그이다. <br>
```Songserializer``` 에서 foreignkey는 해당 모델의 ```-detail``` 을 view-name을 통해 serializing 해야한다. <br>
여기서 핵심은 ```HyperlinkedrelatedField```가 lookup_field로 참고할 수 있는 형태의 viewset을 만들어줘야 한다는 것이다. <br>
즉, viewset의 ```filter_fields```를 설정하고, 이를 serializer의 lookup_field로 지정해줘야 하는 것인데, <br>
이 두개를 맞추지를 않았다... <br>
이제 완벽하게 이해했고, 완벽하게 셋업하였다.<br>

## 참고자료
* [django rest framework auth and permission](http://polyglot.ninja/django-rest-framework-authentication-permissions/)
* [Token-based authentication with Django and React](http://geezhawk.github.io/user-authentication-with-react-and-django-rest-framework)
* [user-registration-authentication-with-django-django-rest-framework-react-and-redux](https://iheanyi.com/journal/user-registration-authentication-with-django-django-rest-framework-react-and-redux/)
* [django rest framework 공홈](http://www.django-rest-framework.org/api-guide/viewsets/)
* [공홈에서 signal 참고](http://www.django-rest-framework.org/api-guide/authentication/)
* [약간의 signal 설명](http://dgkim5360.tistory.com/entry/django-signal-example)
* [migration model 지우는 법](https://simpleisbetterthancomplex.com/tutorial/2016/07/26/how-to-reset-migrations.html)
