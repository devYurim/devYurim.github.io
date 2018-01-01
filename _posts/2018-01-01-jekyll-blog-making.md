---
layout: post
title: 지킬(Jekyll)을 이용하여 깃허브(Github) 블로그 개설하기
comments: true
tags: [jekyll]
---


<center>

이번에 `Github`로 블로그를 이전하면서 `Jekyll`을 이용하여 블로그를 개설 하였다.  
원래 Tistory 블로그를 사용하였는데 코드 하이라이팅이나 에디터가 맘에 들지 않아서 옮기기로 결정하였다.<br>
또한, 대학교 4학년이고 이제 석사 과정을 준비중인 나에게 뭔가 프로그래머스러운(?) 전문적인 블로그가 필요하다고 생각하였고 활발한 오픈소스 활동을 할 수 있는 곳이 `Github` 이였다.<br><br>

이번에 블로그를 옮기면서 커스터마이징 하는데 정말 힘들었다.... <br>
2학년때 `HTML/CSS` 과목을 듣긴했는데.... 공부좀 열심히 해둘걸 그랬다..ㅎㅎ<br>
테마를 다운 받아서 사용했음에도 불구하고 커스터마이징 하는데 꼬박 2일이 걸렸다.<br>
그럼에도 불구하고 `Github` 블로그를 사용하는 이유는 내 입맛대로 레이아웃을 짤 수 있어서 좋은 것 같다.<br><br>

`Github` 블로그 개설은 `지킬(Jekyll)`이라는 깃허브 페이지에서 지원하는 블로그 정적 생성기를 이용하였다.<br><br>

<h3 style="color:#DF0174"> 가이드 작성은 `MAC OS`를 기준으로 작성 되었습니다.</h4><br>

</center>

> ** 1.Git 및 Github 저장소(Repository) 개설 **


일단 Github 블로그를 개설하려면 [Git](https://git-scm.com)이라는 프로그램을 설치하여야 한다. 블로그 관리는 로컬에서 이루어 지며 내 로컬 디렉토리를 Git저장소로 만들기 위하여 `Git` 이라는 프로그램이 필요하다.

* Git 설치 링크 : https://git-scm.com <br><br>

본인의 Github 계정으로 들어가 자신의 계정 이름으로 저장소를 생성하여야 한다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/34465884-154956b8-ef05-11e7-8eb4-967d677e6027.png" width="60%"><br>
</center> <br><br>

clone하고 싶은 장소로 루트를 이동하여 아래의 명령어로 저장소를 clone을 한다.
```

$ git clone https://github.com/devYurim/devYurim.github.io.git
```
<br>

> ** 2. 지킬(Jekyll) 설치 **


블로그를 설치하려면 [jekyll](https://jekyllrb-ko.github.io)을 설치하여야 한다.
`루비(ruby)`가 없다면 루비를 설치하고 진행하여야 한다.<br>
맥OS에는 루비가 이미 설치되어 있다. 루비가 설치되어 있다면 아래의 명령어로 지킬을 설치한다.<br>


```

$ sudo gem install jekyll
```

클론 한 디렉토리 경로로 이동하여 아래의 명령어를 이용해 지킬 블로그를 설치한다.<br>
**( 지킬 블로그 테마를 적용하려면 3. 지킬 테마 설치 참고 )**<br>

```

# 해당 디렉토리에 설치 됨.
$ jekyll new .
```
블로그를 로컬에서 실행하려면 아래의 명령어를 이용하여 실행할 수 있다.<br>

```

$ jekyll build
$ jekyll serve
```
명령어를 실행한 후 http://localhost:4000 으로 접속하면 블로그가 개설이 된 것을 확인 할 수 있다.<br>
github에 적용하려면 해당 디렉토리에서 아래와 같이 명령어를 입력하여 저장소에 커밋 및 푸쉬를 하면 된다.
```

$ git add .
$ git commit -m "커밋 내용"
$ git push origin master
```
https://계정이름.github.io 로 접속하면 블로그가 적용된 것을 볼 수 있다.<br><br>

> ** 3. 지킬 테마(Theme) 설치 **

2번 처럼 설치하면 처음부터 끝까지 모두 블로그를 커스터마이징을 해야한다. 하지만, 미리 만들어진 테마를 이용하면 기존 테마에서 조금만 변경하면 원하는대로 커스터마이징이 가능하다. 아래의 사이트에서 여러가지 테마를 무료로 다운받고 수정하여 사용할 수 있다.<br>

* 지킬 테마 모음 사이트 : http://jekyllthemes.org

원하는 스타일의 테마를 다운 받고 폴더 안에 있는 파일들을 git과 연결된 로컬 디렉토리 안에 넣고 커밋 및 푸쉬를 하면 테마가 내 블로그에 적용이 된다.<br>
**(github commit 및 로컬 실행 관련해서는 2번 참고)**<br>

디렉터리 구조를 간략히 살펴보면
> * _includes : 블로그 삽입 html
* _layouts : 블로그 레이아웃 html
* _posts : 블로그 컨텐츠들
* _sass : css 파일들
* _site : 지킬로 빌드된 블로그 내용들
* assets : 이미지파일, js파일, css파일들
* config.yml : 블로그 생성에 필요한 config가 적힌 파일들_


내가 적용한 테마는 [Type Theme](https://rohanchandra.github.io/type-theme/) 이다. 지금 사용하고 있는 테마는 반응형 테마에 블로그 디자인도 심플하고 태그 기능, 검색기능, 소셜 플러그인도 잘 지원할 뿐더러 코드 하이라이팅 기능과 수학 수식 작성 기능도 지원하여 매우 만족스럽게 사용중이다!<br><br>

지금 내가 사용하는 테마를 포함해서 몇가지 테마를 더 추천하자면 아래와 같다.

1. [Indigo Minimalist Jekyll Template](http://koppl.in/indigo)
2. [Jasper2](https://myjekyll.github.io/jasper2/)
3. [Hydeout](https://fongandrew.github.io/hydeout/)

나는 1번의 Indigo minimalist 테마 디자인이 맘에들어서 현재 적용한 테마에서 디자인 부분만 비슷하게 약간 수정하였다 :)<br>
이렇게 자신이 원하는대로 수정이 가능한 점은 좋은 것 같다. 다만 HTML/CSS에 익숙하지 않다면 진입장벽이 높은건 어쩔수 없는것 같다.

<br>
<br>
<br>
<br>
<br>
