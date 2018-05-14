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
		'rest_framework.authentication.SessionAuthentication',
	   )
	```
	추가
* NGINX : 현재 django를 서빙하고 있는 부분에 uwsgi pass request headers on 을 넣어야 한다.
	* in nginx.conf
		```
		uwsgi_pass_header Authorization;
		uwsgi_pass_request_headers on;
		```
* views : <br>

```python
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

* serializers <br>
	* Modelserializer의 의미는 model을 위한 validator를 자동으로 만들어주는데 있다.
```python
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

* signals.py <br>
**ps) 기존에 있는 User들은 token을 다 알아서 추가해버림.** <br>
```python
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.conf import settings
from rest_framework.authtoken.models import Token

@receiver(post_save, sender=settings.AUTH_USER_MODEL)
def create_auth_token(a, instance=None, created=False, **kwargs):
    if created:
        Token.objects.create(user=instance)

```
* apps.py <br>

```python
from django.apps import AppConfig


class FileapiConfig(AppConfig):
    name = 'fileapi'
    def ready(self):
        import fileapi.signals
```

* app 의 init <br>

```python
default_app_config = 'fileapi.apps.FileapiConfig'
```

* url <br>

```python
router.register(r'users',views.UserViewSet,base_name='user')
url(r'^api-token-auth/', auth_view.obtain_auth_token)
```

### 토큰 세팅 완료

## 2. 모델 약간 변환

### 2.1 Song에 User 모델 넣기
```python
fingerings = models.ForeignKey(settings.AUTH_USER_MODEL,blank=False, null=False,default=PK_OF_SUPERUSER)
``` 
해당 문장 추가. <br>
* 근데 문제는 여기서 추가를 할 때 model을 한번 다 지우고 다시 깔아야한다.... 

## 3. Token...
* 반드시 https로만 serving을 해야한다는데... 그 이유는?

## Debugging
1. 'user-detail' not defined. <br>
이 부분은 [hyperlinkedmodelserializer](http://www.django-rest-framework.org/api-guide/serializers/#hyperlinkedmodelserializer)를 사용해서 나온 버그이다. <br>
```Songserializer``` 에서 foreignkey는 해당 모델의 ```-detail``` 을 view-name을 통해 serializing 해야한다. <br>
여기서 핵심은 ```HyperlinkedrelatedField```가 lookup_field로 참고할 수 있는 형태의 viewset을 만들어줘야 한다는 것이다. <br>
즉, viewset의 ```filter_fields```를 설정하고, 이를 serializer의 lookup_field로 지정해줘야 하는 것인데, <br>
이 두개를 맞추지를 않았다... <br>
이제 완벽하게 이해했고, 완벽하게 셋업하였다.<br>
2. 현재 csrf token이 안된다고 나오더니, service에서 로그인자체가 먹히지를 않는다. 도대체 왜그런거야??

## APITestCase
* 이걸 반드시 익혀야 한다.

## QnA
* view 쪽에서 post_save 하는 것과, serializer에서 create하는 것의 차이는?
	* 사실 post_save는 signal이다.
	* 그렇다면 models 의 publish는? 
		* 이건 그냥 내가 낭비하는 코드같아.
	* 자 그러면 [save vs create](https://stackoverflow.com/questions/23926385/difference-between-objects-create-and-object-save-in-django-orm)
		* 이건 basically same. 근데 serializer에서는 어떻게 create를 하는거지?
	* view 의 perform_update와 serializer의 update, 그리고 model의 save가 어떤 관계인지 잘 모르겠다.
		1. 일단 serializer가 들어온 데이터에 대해서 validate한 지 평가한다.
		2. validate한 지 평가하고 viewset의 perform_create()를 돌린다.
		3. 이게 다시 serializer의 save() 메쏘드를 돌린다.
		4. 그러면 여기서 연결된 serializer의 create() 혹은 update()를 돌린다.
		5. 그러면 serializer.instance의 것이  save, update를 돌린다.
		6. viewset이 response로 serializer.data를 내놓는다.
			* 여기서 serializer.data는 serializer의 to_representation()으로 dict 형태로 바뀐 것.
		* 결국 핵심은 모델의 save를 안간다는 것.
## 참고자료
* [django rest framework auth and permission](http://polyglot.ninja/django-rest-framework-authentication-permissions/)
* [Token-based authentication with Django and React](http://geezhawk.github.io/user-authentication-with-react-and-django-rest-framework)
* [user-registration-authentication-with-django-django-rest-framework-react-and-redux](https://iheanyi.com/journal/user-registration-authentication-with-django-django-rest-framework-react-and-redux/)
* [django rest framework 공홈](http://www.django-rest-framework.org/api-guide/viewsets/)
* [공홈에서 signal 참고](http://www.django-rest-framework.org/api-guide/authentication/)
* [약간의 signal 설명](http://dgkim5360.tistory.com/entry/django-signal-example)
* [migration model 지우는 법](https://simpleisbetterthancomplex.com/tutorial/2016/07/26/how-to-reset-migrations.html)
