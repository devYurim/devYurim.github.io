---
layout: post
title: 도커(Docker)를 이용하여 텐서플로우(Tensorflow) GPU 버전 설치하기 - Ubuntu 16.04(Titan XP)
comments: true
categories : [Python/Tensorflow, Development Enviroment/Docker]
tags: [Tensorflow, Docker]
---

<br><br>
나의 험난했던... 연구실 서버 셋팅기를 글로 남겨보려고 한다..! 연구실 서버의 사양은 <point>Ubuntu 16.04 / Titan XP</point> 이다. 여러명이서 같이 써야하기 때문에 <point>docker로 텐서플로우 GPU 버전</point>을 사용할 수 있는 환경을 구성하였다.<br><br>


> <subtitle> 1. Docker 설치 및 셋팅

* <point>도커(Docker)</point> 설치 : 서버 운영체제는 <point>우분투 16.04 LTS</point> 를 사용하고 있고 아래의 명령어를 이용하여 자동 스크립트로 도커를 설치 하였다.<br>

{% highlight shell %}
$ curl -fsSL https://get.docker.com/ | sudo sh
{% endhighlight %}
<br><br>

* sudo 없이 사용하기 : docker는 기본적으로 root권한이 필요한데, root가 아닌 사용자가 sudo없이 사용하려면 <point>사용자를 docker 그룹에 추가</point>해 주면 된다.<br>

{% highlight shell %}
$ sudo usermod -aG docker $USER           // 현재 접속중인 사용자에게 권한주기
$ sudo usermod -aG docker ailab           // 'ailab' 사용자에게 권한주기
{% endhighlight %}
<br><br>

* 도커(Docker) 버전 확인 : 버전을 확인하여 설치가 되었는지 확인한다.<br>
{% highlight shell %}
$ docker version
{% endhighlight %}
<br><br>

> <subtitle> 2. NVIDIA-docker 설치

도커 컨테이너가 NVIDIA GPU를 support하려면 이미지를 실행할 때 <point>nvidia-docker로 반드시 실행</point>을 해야한다. 설치방법은 아래의 github에 자세히 명시되어 있다. 현재는 nvidia-docker가 2.0 버전이지만, 2.0 버전을 설치하고 실행했을때 명령어를 실행할 수 없어서 <point>1.0으로 설치</point> 하였다.. 2.0으로 설치했을때 docker 실행이 왜 안되는지는 나도 모르겠다 ㅠ.ㅠ..<br>

* nvidia-docker : https://github.com/NVIDIA/nvidia-docker
* nvidia-docker 1.0 설치 공식문서 : https://github.com/NVIDIA/nvidia-docker/wiki/Installation-(version-1.0)
<br><br>

다음의 명령어로 <point>nvidia-docker 1.0</point> 을 설치해준다.<br>
{% highlight shell %}
$ sudo apt-get install nvidia-docker
{% endhighlight %}
<br><br>

다음의 명령어로 nvidia-docker 명령어가 제대로 실행되는지 확인해준다.(자동으로 CUDA 이미지가 다운로드 받아지는데 나중에 지워도 무방하다.)<br>
{% highlight shell %}
$ nvidia-docker run --rm nvidia/cuda nvidia-smi
{% endhighlight %}
<br><br>

아래의 이미지와 같이 <point>GPU 정보</point>가 뜨면 설치 완료이다!<br>
<center><img src="https://user-images.githubusercontent.com/20412850/40543756-c12653a6-6060-11e8-9e6b-a76d55ce6407.png" width="60%"></center>
<br><br>

> <subtitle> 2. NVIDIA 드라이브, CUDA, cuDNN 설치 확인

Tensorflow GPU 버전을 설치하려면 반드시 <point>CUDA와 cuDNN</point>이 설치되어 있어야 한다. 또한, CUDA를 설치하기 위해서는 <point>NVIDIA 드라이버</point>가 설치되어 있어야한다. 자세한 설치 내용은 아래의 블로그 포스팅을 참조하길 바란다.<br>

* NVIDIA 드라이버 설치 : https://devyurim.github.io/2018/05/24/15.html
* CUDA 9.0, cuDNN 7.0 설치 : https://devyurim.github.io/2018/05/25/16.html

<br>
{% highlight shell %}
$ cat /usr/local/cuda/version.txt                                                   // cuda 설치 확인
$ cat /usr/include/cudnn.h | grep -E "CUDNN_MAJOR|CUDNN_MINOR|CUDNN_PATCHLEVEL"     // cuDNN 설치 확인
{% endhighlight %}
<br><br>

> <subtitle> 3. Tensorflow Docker 설치

도커(Docker)는 <point>이미지(image)파일</point>을 이용하여 개발 환경을 운영할 수 있다. 텐서플로우에서 공식적으로 제공하는 <point>텐서플로우 도커</point>를 다운로드 받을 수 있다.<br>

우선 공식 무료 배포 이미지들을 다운로드 받으려면 [<point>docker hub</point>](https://hub.docker.com/) 홈페이지에 가입이 필요하다.<br>
* DockerHub : https://hub.docker.com/
<br><br>

그리고 터미널에서 아래의 명령어를 이용하여 docker hub에 <point>로그인</point>해준다.<br>

{% highlight shell %}
$ docker login
{% endhighlight %}
<br><br>

아이디와 비밀번호를 입력하고 엔터를 누르면 Login Succeeded가 뜨면 docker hub에 접속할 수 있는 권한을 얻은 것이다.<br><br>

* docker hub에서 <point>tensorflow 이미지 검색</point>
docker hub에서 <point>tensorflow</point> 로 검색하여 <point>tag버튼</point> 을 누르면 아래와 같이 다운로드 받을 수 있는 <point>태그 목록</point>을 확인 할 수 있다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/40583308-0302621c-61c7-11e8-91a3-8ebecae5cc42.png" width="60%"> <br>tensorflow docker image 파일 태그 예시</center>
<br><br>

나는 <point>Tensorflow 1.8.0 gpu버전 python3</point>을 사용하길 원하기 때문에 아래의 명령어를 이용하여 <point>텐서플로우 이미지를 다운로드</point> 받았다. 자세한 사항은 아래의 사이트 참고<br>
* tensorflow docker hub : https://hub.docker.com/r/tensorflow/tensorflow/
<br>

{% highlight shell %}
$ docker pull tensorflow/tensorflow:1.8.0-gpu-py3       // 태그명이 1.8.0-gpu-py3인 텐서플로우 이미지 다운로드
$ docker images                                         // 도커 내 이미지 확인 명령어
{% endhighlight %}
<br><br>

이미지를 다운 로드 받았으면 <point>nvidia-docker</point>를 이용하여 컨테이너를 생성하면된다. docker에서 컨테이너를 생성하는 명령어는 <point>run</point>이다. 내가 사용한 옵션은 다음과 같다.<br>

{% highlight shell %}
$ nvidia-docker run -it \
-p 8891:8888 -p 6006:6006
--name ailab-yurim
-v /home/ailab/docker/ailab-yurim:/notebooks \
-e PASSWORD="0000" \
--restart always \
tensorflow/tensorflow:1.8.0-gpu-py3
{% endhighlight %}
<br><br>

* **옵션 추가 설명** <br><br>
-p : 포트 번호 입력 (호스트 포트번호 : 컨테이너 포트번호) <br>
     옵션에서 포트번호를 2개를 해준 이유는 8888은 <point>Jupyter notebook</point> 연결 포트이고 6006은 <point>Tensorboard</point> 연결 포트이다. <br>
     **6006을 연결 안해주면** Tensorboard가 실행이 안되므로 포트를 꼭 맵핑해주어야 한다!!<br>
--name : 생성할 컨테이너 이름 <br>
-v : 컨테이너와 맵핑할 폴더의 경로 + Jupyter notebook 설정 <br>
-e : 컨테이너 접속 비밀번호 <br>
--restart always : 도커 데몬이 실행됬을때 컨테이너 자동으로 실행(run) <br>
tensorflow:1.8.0-gpu-py3 : 실행할 이미지를 <point>'이미지명:태그명'</point> 으로 입력 <br><br><br>

Jupyter notebook 접속시 localhost라면 <point>http://localhost:8888</point> 로 접속하면 되고, 따로 ip주소가 설정되있다면 <point>http://ip주소:호스트포트번호</point> 로 접속하면 된다. Tensorboard도 마찬가지 이다.<br><br><br>


> <point>[!!!] 우분투 도커 데몬 실행 안될때</point>

컴퓨터를 재부팅하고 docker를 재부팅 하였을때 아래와 같은 이미지의 오류와 <point>docker deamon을 실행할 수 없다는 오류</point>가 발생하였다. <br>
<center><img src="https://user-images.githubusercontent.com/20412850/40583676-56f2afb0-61ce-11e8-81f7-5dfaa1de1515.png" width="60%"></center>
<br><br>

이 문제는 nvidia-docker로 컨테이너를 생성하였을 때 발생하는 <point>nvidia-container-runtime 문제</point>로 docker engine에 nvidia-runtime을 등록해주어야 한다고 한다. github 홈페이지를 보고 아래의 명령어를 터미널에 붙여 넣어 docker engine을 다시 설정하여 문제를 해결하였다.<br>

* nvidia-container-runtime : https://github.com/NVIDIA/nvidia-container-runtime

<br>

{% highlight shell %}
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo tee /etc/systemd/system/docker.service.d/override.conf << EOF
 [서비스]
ExecStart =
ExecStart = / usr / bin / dockerd --host = fd : // --add-runtime = nvidia = / usr / bin / nvidia-container-runtime
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
{% endhighlight %}

<br><br><br><br><br>


> <subtitle> conference

* https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html
* https://sehoi.github.io/2017-01-06/docker-basic/
* https://www.tensorflow.org/install/install_linux#InstallingDocker
* https://github.com/NVIDIA/nvidia-docker/wiki/Installation-(version-1.0)
* https://github.com/NVIDIA/nvidia-container-runtime
