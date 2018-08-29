---
layout: post
title: 텐서플로우(Tensorflow) GPU 버전 설치하기 - Windows 10
comments: true
categories : [Python/Tensorflow]
tags: [Tensorflow]
---

<br><br>
연구실 내 컴퓨터에 드디어 GPU가..!! 로컬에서 Tensorflow GPU를 사용하기 위해 험난했던 GPU 버전 설치 방법을 남긴다...<br>
<point>반드시 tensorflow 공식 문서를 확인해야한다!!!</point> 추천 cuda버전, cudnn버전, anaconda일때 파이썬 몇 버전 써야하는지, native pip 일때 파이썬 몇 버전을 써야하는지 적혀있다. 자신의 환경에 맞춰서 공식문서를 보고 파이썬 버전을 잘 선택해야한다. 또한, 선택하는 운영체제에 따라서도 버전이 다르다. <br><br>
나는 <point>Windows 10 / Anaconda</point> 를 사용하여 설치할 예정이다.<br><br>

* Tensorflow 공식 홈페이지 : https://www.tensorflow.org/
<br><br>

> <subtitle> 1. CUDA Toolkit 설치 </subtitle>

텐서플로우 홈페이지에가서 install 버튼을 눌러보면 친절하게 <point>NVIDIA CUDA xx 설치하세요</point> 라고 나와있다.<br><br>
<center><img src="https://user-images.githubusercontent.com/20412850/39426642-9610c13c-4cbb-11e8-872b-f3bbfa89ee5b.png" width="60%"></center>
<br><br>

나는 여기에 맞춰서 <point>CUDA 9.0</point> 버전을 설치하였다. 현재는 배포하는 버전은 9.1 버전이여서 [CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)에 가서 CUDA 9.0을 받았다.<br>
* NVIDIA Cuda Toolkit 설치 : https://developer.nvidia.com/cuda-toolkit-archive

<center><img src="https://user-images.githubusercontent.com/20412850/39426815-3cd9bb18-4cbc-11e8-9c45-f3342ae3a0d9.png" width="60%"></center><br><br>

설치가 완료 되고 재부팅한다음에 <point>환경변수</point> 가 제대로 설치 되었나 확인해야한다.<br>
* 시스템 환경변수 편집 -> 고급 -> 환경변수 클릭<br><br>

<center><img src="https://user-images.githubusercontent.com/20412850/39426945-c9fb33fa-4cbc-11e8-9fd5-c414a6fca79e.png" width="60%"></center><br><br>
위와 같이 <point>CUDA_PATH</point> 가 추가되어있으면 OK이다!<br>
만약에 CUDA버전을 여러개 설치한다면 CUDA_PATH_V9_0과 같이 구분되며 최근 설치한 버전이 CUDA_PATH로 들어간다고 한다.<br><br>

> <subtitle> 2. cuDNN 다운로드 </subtitle>

이것도 역시 텐서플로우 홈페이지에 명시되어 있다. <point>cuDNN은 7.0.x버전</point> 으로 다운로드 받았다. cuDNN을 받으려면 DEVELOPER 홈페이지에 가입이 필요하다. 먼저 가입을 하고 다운로드 받는다.<br>
* cuDNN 다운로드 : https://developer.nvidia.com/rdp/cudnn-archive
<br><br>

CUDA 버전에 유념해서 다운로드 받아야한다. 나는 <point>cuDNN v7.0.5 for CUDA 9.0</point> 을 받았다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/39427302-40264032-4cbe-11e8-8a4c-5260fe748cdc.png" width="60%"></center><br><br>

압축파일을 압축 해제후 <point>cuda 폴더</point> 에 들어가보면 다음과 같은 <point>3개</point> 의 파일이 있다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/39428641-17dc22ae-4cc3-11e8-845d-7bc6c224fe20.png" width="60%"></center><br><br>
이 세개 파일을 모두 CUDA_PATH 경로의 폴더에 추가해준다. CUDA_PATH 경로는 시스템 환경변수에 가면 확인 할 수 있다. 나는 아래와 같다.<br>
{% highlight shell %}
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0
{% endhighlight %}
<br><br>

즉, <point>cuda 폴더 안의 내용을 CUDA_PATH 경로에 ctrl+c / ctrl+v 해서 복사</point> 하면 완료!<br><br>


> <subtitle> 3. Anaconda를 이용해 Tensorflow 설치 </subtitle>

나는 아나콘다(Anaconda)가 이미 설치되어 있는 상태이기 때문에 아나콘다를 이용하여 설치하였다.<br> 아나콘다가 없다면 아나콘다를 설치하거나 python3.5버전(홈페이지에서 운영체제 별로 지원하는 파이썬 버전으로 설치 할 것)을 설치하면된다.<br>
일단 현재 버전의 아나콘다는 python3.6을 기준으로하기 때문에 python3.5 버전을 가진 새로운 아나콘다 환경을 만들어 주어야 한다.<br>
아직 python 3.6은 지원하지 않는 것으로 보이므로(2018.8.29 기준으로 지원) <point>반드시!! python3.5 버전</point> 으로 환경설정을 해주어야 한다.<br>
일단 윈도우 검색창에 Anaconda를 검색하여 <point>Anaconda Prompt</point> 를 켠다. 나는 환경이름을 cuda로 설정했다.<br>

환경활성화는 Windows Powershell에서는 안되고 conda prompt에서만 가능하다!<br>

{% highlight shell %}
conda create -n cuda pip python=3.5 /*환경 만들기(python 3.5) -> y/n 나오면 y 누를것.*/
activate cuda  /*환경 활성화*/
{% endhighlight %}
<br>

<center><img src="https://user-images.githubusercontent.com/20412850/39428040-f36eec96-4cc0-11e8-9705-c8c9e9ca368a.png" width="60%"></center><br>
activate 하였을 때 다음과 같이 <point>(cuda)</point> 로 변경되어야 한다. 다음 아래의 명령어를 실행하여 <point>Tensorflow GPU 버전</point> 을 설치한다.<br>

{% highlight shell %}
python -m pip install --upgrade pip /*일단 pip 를 업그레이드 시켜준다.*/
pip install --ignore-installed --upgrade tensorflow-gpu /*텐서플로우 GPU 버전 설치*/
{% endhighlight %}
<br><br>

<point>Tesorflow CPU 버전</point> 은 아래와 같은 명령어로 설치하면 된다.<br>
{% highlight shell %}
pip install --ignore-installed --upgrade tensorflow
{% endhighlight %}
<br><br>

설치가 끝나면 jupyter notebook도 설치하고 jupyter notebook을 켜서 코드가 잘 돌아가는지 확인한다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/39429658-851e5f1e-4cc6-11e8-993e-d2f1905a39f0.png" width="80%"></center><br>
<center><img src="https://user-images.githubusercontent.com/20412850/39429707-b09162b8-4cc6-11e8-94b7-be1836953fab.png" width="120%"></center><br>

위에 처럼 예시 코드가 잘 돌아가면 GPU 버전 설치 완료!!

<br><br><br>

아래의 명령어들은 참고 명령어 사항이다.<br>
{% highlight shell %}
conda info --envs /*아나콘다 환경 확인하기*/
conda remove --name 삭제하려는환경이름 --all /*아나콘다 환경 삭제하기*/
{% endhighlight %}
<br>

<center><img src="https://user-images.githubusercontent.com/20412850/39428118-37314c26-4cc1-11e8-9307-414b5af90864.png" width="60%"></center><br>
위와 같이 모든 아나콘다 환경이 표시가 된다. 내가 현재 사용하고 있는 환경은 ```*``` 로 표시된다.<br><br>

아나콘다 환경 활성화/비활성화는 아래와 같은 명령어로 실행 할 수 있다.<br>
{% highlight shell %}
activate cuda /*cuda라는 이름을 가진 아나콘다 환경 활성화*/
deactivate cuda /*cuda라는 이름을 가진 아나콘다 환경 비 활성화*/
{% endhighlight %}
<br><br><br>

> **Reference**
* https://www.tensorflow.org/install/install_windows
* http://solarisailab.com/archives/1581
* http://hiuaa.tistory.com/39
* http://dwfox.tistory.com/85

<br><br><br><br><br>
