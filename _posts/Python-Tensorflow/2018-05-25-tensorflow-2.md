---
layout: post
title: 텐서플로우(Tensorflow) 설치를 위한 CUDA / cuDNN 설치 - Ubuntu 16.04 LTS Server(Titan XP)  
comments: true
categories : [Python/Tensorflow]
tags: [Tensorflow]
---

<br><br>
서버 사양은 <point> Ubuntu 16.04 / titan XP </point> 이고, 설치할 버전은 <point> CUDA 9.0 / cuDNN 7.0 </point> 이다.
<br><br>

> <subtitle> 1. NVIDIA 드라이버 설치 확인 및 Compute Capability 확인

아래의 명령어를 통해 드라이버 설치와 그래픽카드를 확인 할 수 있다.<br>

{% highlight shell %}
$ nvidia-smi
{% endhighlight %}
<br><br>

<center><img src="https://user-images.githubusercontent.com/20412850/40532078-44b5b16c-6039-11e8-923b-b94512544107.png" width="80%"></center>
<br><br>

CUDA 설치 이전에 GPU의 <point>Compute Capability</point>를 지원하는 지 확인하여야 한다. Compute Capability에 따라 설치 할 수 있는 CUDA 버전은 NVIDIA 홈페이지에서 확인이 가능하다.<br><br>

* NVIDIA 홈페이지 : https://developer.nvidia.com/cuda-gpus
<br><br>

<center><img src="https://user-images.githubusercontent.com/20412850/40532263-d5c04a14-6039-11e8-9e01-0666d5468c77.png" width="30%"><br>NVIDIA 홈페이지  - CUDA-Enabled GeForce Products 발췌</center>
<br><br>

CUDA 9.0 버전은 <point>Compute capability 3.0~7.x</point> 까지 지원한다.<br><br>


> <subtitle> 2. 패키지 리스트 추가


아래의 명령어로 패키지 소스 리스트를 추가하고 업데이트 한다.<br>

{% highlight shell %}
$ sudo apt-get update && sudo apt-get install sudo gnupg
$ sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
$ sudo sh -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64 /" >> /etc/apt/sources.list.d/cuda.list'
$ sudo sh -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" >> /etc/apt/sources.list.d/cuda.list'
$ cat /etc/apt/sources.list.d/cuda.list
$ sudo apt-get update
{% endhighlight %}
<br>

아래의 CUDA와 cuDNN의 버전은 [텐서플로우 홈페이지](https://www.tensorflow.org/install/install_linux#InstallingAnaconda)를 참고하여 설치하였다.<br><br>

> <subtitle> 3. CUDA 9.0 설치

아래의 명령어를 이용하여 <point>CUDA 9.0</point>을 설치한다.<br>

{% highlight shell %}
$ sudo apt-get install cuda-9-0
{% endhighlight %}
<br><br>

> <subtitle> 4. cuDNN 7.0 설치

아래의 명령어를 이용하여 <point>cuDNN 7.0</point>을 설치한다.<br>

{% highlight shell %}
$ sudo apt-get install libcudnn7-dev
{% endhighlight %}
<br><br>

> <subtitle> 5. 버전 확인

* CUDA 설치 확인

{% highlight shell %}
$ cat /usr/local/cuda/version.txt
{% endhighlight %}
<br>

* cuDNN 설치 확인

{% highlight shell %}
$ cat /usr/include/cudnn.h | grep -E "CUDNN_MAJOR|CUDNN_MINOR|CUDNN_PATCHLEVEL"
{% endhighlight %}

<br><br><br>

> <subtitle> refernece

* https://hiseon.me/2018/03/11/cuda-install/
* https://developer.nvidia.com/cuda-gpus
* https://www.tensorflow.org/install/install_linux#InstallingAnaconda
<br><br><br><br><br>
