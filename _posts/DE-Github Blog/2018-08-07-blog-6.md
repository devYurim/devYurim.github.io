---
layout: post
title: 지킬(Jekyll) 블로그 카테고리(category) 만들기
comments: true
categories : [Development Environment/Github Blog]
tags: [Github Blog]
---

<br><br><p>포스트 개수가 많아지다 보니 관리가 힘들어서 카테고리를 만들었다. 카테고리 만든 방법을 간단하게 정리하였다.</p><br><br>

> <subtitle> 1. _layouts 폴더에 category.html 파일 생성 </subtitle>


먼저 _layouts 폴더에 <point>category.html</point> 파일을 생성해준다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/43759600-a57e6fc4-9a5a-11e8-8de3-b9fd7d4691cf.png" width="80%"></center><br><br>

category.html은 category 이름에 맞는 포스트들의 타이트들을 리스트로 보여준다. 코드는 아래와 같다.<br>

{% highlight html %}
---
layout: default
---
<ul class="posts-list">
  {% raw %}
  {% assign category = page.category | default: page.title %}
  {% for post in site.categories[category] %}
    <li>
      <h3>
        <a href="{{ site.baseurl }}{{ post.url }}">
          {{ post.title }}
        </a>
        <small>{{ post.date | date_to_string }}</small>
      </h3>
    </li>
  {% endfor %}
  {% endraw %}
</ul>
{% endhighlight %}

<br><br>

> <subtitle> 2. _includes 폴더의 index.html 파일 수정</subtitle>

<br>
아래의 내용과 같이 수정한다.<br>

{% highlight html %}
<header class="site-category">
  <ul>
    {% raw %}
    {% assign pages_list = site.pages %}
    {% for node in pages_list %}
      {% if node.title != null %}
        {% if node.layout == "category" %}
          <li><a class="category-link {% if page.url == node.url %} active{% endif %}"
          href="{{ site.baseurl }}{{ node.url }}">{{ node.title }}</a></li>
        {% endif %}
      {% endif %}
    {% endfor %}
    {% endraw %}
</ul>
</header>
{% endhighlight %}

<br><br>

> <subtitle>3. category 폴더 생성 </subtitle>

<br>
(맨 바깥의 디렉토리) 계정명.github.io 폴더 안에 <point>category</point> 폴더를 만들어준 다음 원하는 카테고리 명의 markdown 파일을 추가해준다. <br>
<center><img src="https://user-images.githubusercontent.com/20412850/43759845-6fee187c-9a5b-11e8-9f09-9be929a5b3a6.png" width="40%"></center><br><br>

마크다운 파일 내용은 아래와 같다.<br><br>

{% highlight markdown %}
---
layout: category
title: 여기에 카테고리 이름 입력!
---

{% endhighlight %}
<br><br>

예를들어, docker 카테고리를 만들고 싶다면 category 폴더의 docker.md의 내용은 다음과 같다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/43760274-88df8d92-9a5c-11e8-8860-22ecf8097bd5.png" width="60%"></center><br><br>

> <subtitle>4. 블로그 포스트에 category 추가하기</subtitle>

<br>
위의 세가지 셋팅 후 포스트를 작성 시 <point>카테고리 항목만 추가</point>하여 주면 카테고리 지정이 된다.<br>
<center><img src="https://user-images.githubusercontent.com/20412850/43760664-bc1e00f2-9a5d-11e8-8c2f-0f8c7ac335d6.png" width="60%"></center><br><br>
위의 이미지처럼 categories 항목을 추가해주고 원하는 카테고리 이름을 작성해주면 된다. (복수 카테고리도 가능하다.)<br><br>

주의 해야 할 점은 반드시! <point>category 폴더에 원하는 category</point>를 만들어주고 포스트에 같은 카테고리 명을 입력해야 한다.<br><br>

<br><br><br><br><br>
