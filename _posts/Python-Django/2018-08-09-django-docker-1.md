---
layout: post
title: Docker를 이용하여 Django 웹 데이터베이스 서버 셋팅
comments: true
categories : [Python/Django, Development Environment/Docker]
tags: [Docker, Django]
---

<br><br>
docker 컨테이너 연결을 하고 난 뒤 django 서버 셋팅하는 방법을 정리하였다. 데이터베이스 컨테이너와 웹서버 컨테이너를 연결하는 방법은 아래의 포스팅을 참고하면 될 것 같다.<br>
* 도커 컨테이너 연결 : https://devyurim.github.io/ubuntu/docker/2018/06/27/ubuntu-docker-6.html
<br><br>

도커 컨테이너 내에서 장고 서버를 구동하기 위한 셋팅 방법을 정리하였다. 장고 걸스 튜토리얼이 많은 도움이 되었다.<br><br>


> <subtitle> 1. django 프로젝트 생성 </subtitle>

장고 컨테이너 내부로 접속하여 장고 웹서버가 작동되는지 확인이 필요하다. 먼저 아래의 명령어를 실행하여 장고 프로젝트를 생성한다.<br>
{% highlight shell %}
$ django-admin startproject mysite .
{% endhighlight %}
<br>

위의 명령어를 실행하여 생성되는 디렉터리 구조는 다음과 같다.<br>
                        ├──manage.py <br>
                        └──mysite <br>
&nbsp;&nbsp;&nbsp;&nbsp;    ├── __init__.py <br>
&nbsp;&nbsp;&nbsp;&nbsp;    ├── settings.py <br>
&nbsp;&nbsp;&nbsp;&nbsp;    ├── urls.py <br>
&nbsp;&nbsp;&nbsp;&nbsp;    └── wsgi.py <br><br>

간단하게 역할들을 살펴보면 아래의 설명과 같다.<br>

* manage.py : 스크립트, 사이트 관리를 도와주는 역할, 스크립트 실행으로 웹서버를 실행할 수 있다.<br>
* mysite/settings.py : 웹사이트 설정 파일<br>
* mysite/urls.py : Django 로 작성된 사이트의 "목차"라고 할 수 있다. <br>
* mysite/wsgi.py : nginx나 apache를 연동하기 위한 interface<br><br>

> <subtitle>2. mysite/settings.py 파일 수정 </subtitle>

그 다음 mysite/settings.py 파일을 수정해야한다. 에디터로 다음의 내용을 수정한다.<br><br>

먼저 시간, 호스트 접근권한을 변경하여 준다. <br>
{% highlight python %}
TIME_ZONE='Asia/Seoul'
ALLOWED_HOSTS = ['xxx.xxx.xxx.xxx(서버ip)','127.0.0.1', '.pythonanywhere.com']  #만약에 서버가 있다면 서버의 ip 주소도 추가해준다.
LANGUAGE_CODE = 'en-us'       # 한국어로 바꾸고 싶다면 'ko-kr'로 변경 해주면 된다.
{% endhighlight %}
<br>

그 다음 STATIC_URL밑에 STATIC_ROOT를 추가한다.<br>
{% highlight python %}
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
{% endhighlight %}
<br>

이렇게 수정한다음 저장하고 아래의 명령어로 migrate를 해준다.<br>
{% highlight shell %}
$ python manage.py migrate
{% endhighlight %}
<br>

그 다음 장고 서버를 실행 한다.<br>
{% highlight shell %}
$ python manage.py runserver 0.0.0.0:8000
{% endhighlight %}
<br>

다음과 같은 화면이 나오면 성공이다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/43830179-b0c7ed74-9b3b-11e8-9e31-d563e2432ea8.png" width="60%"></center><br><br>

> <subtitle>3. 데이터베이스 설정 변경 </subtitle>


장고 서버에서 셋팅하기전 데이터 베이스 컨테이너와 link가 잘 되었는지 확인한다.<br>
{% highlight shell %}
$ mysql -h mariadb -u root -p0000
{% endhighlight %}
<br>

잘 접속이 되면 이제 장고 컨테이너에 링크된 mariadb를 셋팅하면 된다. 장고 서버는 기본 데이터베이스가 sqlite3으로 설정되어 있다.<br><br>

나는 mariadb를 사용하길 원하므로 mysite/settings.py 에서 다음과 같이 장고 서버의 셋팅을 변경한다.<br>

{% highlight python %}
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'impactfactor',               # default로 사용하려는 데이터베이스 명
        'USER':'root',                        # 유저 명
        'PASSWORD':'0000',                    # 비밀번호
        'HOST':'mariadb',                     # 호스트 명
        'PORT':'',                            # 포트 번호 (default 3306)
    }
}
{% endhighlight %}
<br><br>

migrate가 성공하고 난 뒤 다시 서버를 실행하면 정상적으로 실행되는 것을 확인 할 수 있다.<br>
{% highlight shell %}
$ python manage.py migrate
$ python manage.py runserver 0.0.0.0:8000
{% endhighlight %}
<br>
<center><img src="https://user-images.githubusercontent.com/20412850/43830179-b0c7ed74-9b3b-11e8-9e31-d563e2432ea8.png" width="60%"></center><br><br>

> <subtitle>4. django 어플리케이션 생성 </subtitle>

데이터 베이스 화면을 보고싶다면 어플리케이션을 작성(데이터베이스 관련)하고 관리자 계정을 생성하여야 한다. 먼저 관리자 계정을 생성하려면 프로젝트 내부에 어플리케이션이 있어야 한다. manage.py가 있는 경로에서 다음의 명령어로 어플리케이션 폴더를 생성한다.<br>

{% highlight shell %}
$ python manage.py startapp show      # startapp 다음에 어플리케이션 이름을 입력한다.
{% endhighlight %}
<br><br>

그럼 현재 디렉터리가 다음과 같을 것이다.<br>

├──manage.py <br>
├──mysite <br>
&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;    ├── __init__.py <br>
&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;    ├── settings.py <br>
&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;    ├── urls.py <br>
&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;    ├── wsgi.py <br>
└── show <br>
&nbsp;&nbsp;&nbsp;&nbsp;     ├── migrations <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     | &nbsp;&nbsp;&nbsp;&nbsp;      └── __init__.py <br>
&nbsp;&nbsp;&nbsp;&nbsp;     ├── __init__.py <br>
&nbsp;&nbsp;&nbsp;&nbsp;     ├── admin.py <br>
&nbsp;&nbsp;&nbsp;&nbsp;     ├── models.py <br>
&nbsp;&nbsp;&nbsp;&nbsp;     ├── tests.py <br>
&nbsp;&nbsp;&nbsp;&nbsp;     └── views.py <br><br>

mysite/settings.py를 열고 다음의 항목에 어플리케이션(show)을 추가해준다.<br>

{% highlight python %}
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'show',
]
{% endhighlight %}
<br><br>

데이터베이스 테이블을 만들기 위해서 show/models.py 파일을 아래와 같이 수정한다.<br>
테이블명이 클래스명이 되며 클래스의 변수는 테이블의 각 컬럼이 된다.<br>

{% highlight python %}
from django.db import models

class Journal(models.Model):
    isbn = models.BigIntegerField(primary_key=True)
    title = models.CharField(max_length=128)

{% endhighlight %}
<br><br>

그 다음 show/admin.py 파일을 수정하여 관리자 페이지에 테이블이 나타나도록 수정한다.<br>
{% highlight python %}
from django.contrib import admin
from show.models import Journal

admin.site.register(Journal)
{% endhighlight %}
<br><br>

그 다음 변경사항을 반영하기 위해 아래의 명령어를 실행한다.<br>
{% highlight shell %}
$ python manage.py makemigrations
$ python manage.py migrate
{% endhighlight %}
<br><br>

migrate를 하면 데이터베이스에 테이블이 만들어지면서 동기화가 된다. mysql 데이터베이스를 확인해보면 다음과 같다. 아래의 데이터베이스는 추가적으로 여러개의 모델을 생성하여 테이블을 여러개 생성한 결과이다. <br>
 <center><img src="https://user-images.githubusercontent.com/20412850/43893594-6fde0030-9c0a-11e8-8349-d6e4f3e9dfc1.png" width="40%"></center><br><br>


> <subtitle>5. django 관리자 계정 생성 </subtitle>

이제 데이터 베이스에 관련된 셋팅은 다 되었고 최종적으로 확인하기 위하여 관리자 계정을 생성해야한다. 서버를 켜둔 상태로 다른 프롬프트 창을 열어 컨테이너에 재접속하여 아래의 명령어로 관리자 계정을 생성한다.<br>

{% highlight shell %}
$ python manage.py createsuperuser
Username (leave blank to use 'root'): # 관리자 계정명
Email address: # 이메일
Password: # 패스워드 입력
Password (again): # 패스워드 확인
{% endhighlight %}
<br><br>

 호스트주소:포트/admin(ex. 127.0.0.1:8888/admin) 으로 접속하면 관리자 계정 로그인창이 뜬다. 방금 생성한 관리자 계정으로 로그인하면 접속이 되고 models.py에서 만들었던 테이블이 생성되 있는 것을 볼 수 있다.<br>
 <center><img src="https://user-images.githubusercontent.com/20412850/43833454-b5d5b3be-9b45-11e8-8a4a-b1afd1e02201.png" width="60%"></center><br><br><br>


> <subtitle>reference</subtitle>

* https://tutorial.djangogirls.org/ko
* http://blog.daum.net/_blog/BlogTypeView.do?blogid=0QtKg&articleno=28&categoryId=0&totalcnt=30


<br><br><br><br><br>
