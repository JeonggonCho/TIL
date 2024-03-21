# View의 ORM

## 목차

1. [사전세팅](#1-사전세팅)
    1. [app URLs 분할 및 연결](#1-1-app-urls-분할-및-연결)
    2. [index 페이지 작성](#1-2-index-페이지-작성)
2. [조회](#2-조회)
    1. [전체 게시글 조회](#2-1-전체-게시글-조회)
    2. [단일 게시글 조회](#2-2-단일-게시글-조회)
3. [생성](#3-생성)
    1. [생성 view 함수 로직](#3-1-생성-view-함수-로직)
        - [new 로직 작성](#new-로직-작성)
        - [create 로직 작성](#create-로직-작성)
4. [HTTP request methods](#4-http-request-methods)
    1. [redirect()](#4-1-redirect)
    2. [GET & POST](#4-2-get--post)
        - [GET method](#get-method)
        - [POST method](#post-method)
        - [CSRF](#csrf)
        - [Security Token (CSRF Token)](#security-token-csrf-token)
5. [삭제](#5-삭제)
6. [수정](#6-수정)
    1. [수정 view 함수 로직](#6-1-수정-view-함수-로직)
        - [edit 로직 작성](#edit-로직-작성)
        - [update 로직 작성](#update-로직-작성)

<br>
<br>

## 1. 사전세팅

### 1-1. app URLs 분할 및 연결

-   프로젝트 폴더에서 articles 앱으로 articles.urls 분배

```python
# crud/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
]
```

-   articles 앱에서 url 받아 연결

```python
# articles/urls.py

from django.urls import path

app_name = 'articles'
urlpatterns = []
```

<br>

### 1-2. index 페이지 작성

<articles 앱 url 작성>

```python
# articles/urls.py

from django.urls import path
from . import views

app_name = 'articles'
urlpatterns = [
    path('', views.index, name='index'),
]
```

<br>

<articles 앱 view 함수 작성>

```python
# articles/views.py

def index(request):
    return render(request, 'articles/index.html')
```

<br>

<articles 앱 index 템플릿 작성>

```html
<!-- articles/index.html -->

<h1>Articles</h1>
```

<br>
<br>

## 2. 조회

### 2-1. 전체 게시글 조회

<articles 앱의 views에서 index 함수 정의>

```python
# articles/views.py

from .models import Article

def index(request):
    articles = Article.objects.all()
    context = {
        'articles' : articles,
    }
    return render(request, 'articles/index.html', context)
```

-   `models`에서 Article 모델을 가져오고, QuerySet API 메서드 `.all()`을 사용하여 모든 데이터 객체를 가져와 articles 변수에 담기
-   `context` 객체를 생성하여 articles 키에 데이터 객체를 담은 변수 articles를 값으로 받기
-   렌더링하는 템플릿 articles/index.html에 context 보내기

<br>

<articles앱의 index.html에서 보낸 articles 객체 데이터 사용>

```html
<!-- articles/index.html -->

<h1>Articles</h1>
<hr />
{% for article in articles %}
<p>글 번호: {{ article.pk }}</p>
<a href="{% url 'articles:detail' article.pk %}">
    <p>글 제목: {{ article.title }}</p>
</a>
<p>글 내용: {{ article.content }}</p>
<hr />
{% endfor%}
```

-   DTL 태그 for 구문을 사용하여 articles 객체를 순회하고 점표기법으로 각 데이터의 번호, 제목, 내용에 접근하여 렌더링
-   url 태그로 각 글의 제목을 누르면 상세페이지로 이동
    -   detail 함수에서 사용할 pk인자를 article.pk로 담아 보내기

<br>

### 2-2. 단일 게시글 조회

<url로 디테일 페이지 설정>

```python
# articles/urls.py

urlpatterns = [
    ...
    path('<int:pk>/', views.detail, name='detail'),
]
```

<br>

<디테일 페이지 처리하는 view의 detail 함수 정의>

```python
# articles/views.py

from .models import Article

def detail(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article,
    }
    return render(request, 'articles/detail.html', context)
```

-   models에서 Article 모델을 가져와 QuerySet API 메서드 중 `.get()` 메서드를 사용한다.
-   함수의 인자로 `request 요청`과 함께 어떤 게시글을 조회할 지, 찾기위한 Unique 값인 `pk(primary key)`를 받는다.
-   따라서 get에서 pk값이 인자로 보낸 pk와 같은 모델 데이터를 article 변수에 담기
-   context 객체에 article 키에 변수 article을 값으로 지정
-   detail.html 렌더링 페이지에 context 보내기

<br>

<받은 article 데이터 렌더링>

```html
<!-- templates/articles/detail.html -->

<h2>DETAIL</h2>
<h3>{{ article.pk }} 번째 글</h3>
<hr />
<p>제목: {{ article.title }}</p>
<p>내용: {{ article.content }}</p>
<p>작성 시각: {{ article.created_at }}</p>
<p>수정 시각: {{ article.updated_at }}</p>
<hr />
<a href="{% url 'articles:index' %}">[back]</a>
```

-   점표기법을 통해 접근하여 렌더링한다.

<br>
<br>

## 3. 생성

### 3-1. 생성 view 함수 로직

-   2가지의 view 함수가 필요하다.
    -   `입력을 받는` 페이지를 렌더링 `new`
    -   `데이터를 DB에 저장`하는 `create`

<br>

### - new 로직 작성

<url 생성>

```python
# articles/urls.py

urlpatterns = [
    ...
    path('new/', views.new, name='new'),
]
```

<br>

<view 함수에서 new 템플릿으로 이동시키는 함수 생성>

```python
# articles/views.py

def new(request):
    return render(request, 'articles/new.html')
```

-   articles의 new.html만 렌더링하면 되기에 따로 context를 생성하거나 보내지 않음

<br>

<사용자의 입력을 받는 new 템플릿 생성>

```html
<!-- templates/articles/new.html -->

<h1>NEW</h1>
<form action="{% url 'articles:create' %}" method="GET">
    <div>
        <label for="title">Title: </label>
        <input type="text" name="title" id="title" />
    </div>
    <div>
        <label for="content">Content: </label>
        <textarea name="content" id="content"></textarea>
    </div>
    <input type="submit" />
</form>
<hr />
<a href="{% url 'articles:index' %}">[back]</a>
```

<br>

### - create 로직 작성

<url 생성>

```python
# articles/urls.py

urlpatterns = [
    ...
    path('create/', views.create, name='create'),
]
```

<br>

<템플릿 작성>

```html
<!-- templates/articles/create.html -->

<h1>게시글이 문제없이 작성되었습니다.</h1>
```

<br>

<생성된 정보를 저장할 create 함수 생성>

```python
# articles.views.py

def create(request):
    title = request.GET.get('title')
    content = request.GET.get('content')

    # 저장 방법1
    # article = Article() 인스턴스 생성
    # article.title = title 제목 담기
    # article.content = content 내용 담기
    # article.save() 저장


    # 저장 방법2
    article = Article(title=title, content=content) # 인스턴스 생성 및 정보 담기
    article.save # 저장


    # 저장 방법3
    # Article.objects.create(title=title, content=content) 생성, 담기, 저장을 한 번에 실행

    return render(request, 'articles/create.html')
```

-   new.html의 form에서 입력받은 내용은 `method="GET"`으로 url의 `request`에 담겨서 전달됨
-   따라서 `request.GET.get()`으로 request 자체를 참조함
-   ORM 구문을 활용하여 model에 저장하기
-   저장이 완료되면 create.html로 보내진다.

<br>
<br>

## 4. HTTP request methods

-   게시글 작성 후, 완료를 나타내는 `페이지를 렌더링`하는 것은 `작성의 요청`에서는 적절한 응답이 아니다.

<br>

### 4-1. redirect()

-   인자에 작성된 주소로 다시 요청을 보냄

```python
# views.py

from django.shortcuts import render, redirect

def create(request):
    title = request.GET.get('title')
    content = request.GET.get('content')
    article = Article(title=title, content=content)
    article.save()

    return redirect('articles:detail', article.pk)
```

-   redirect 호출을 통해 작성된 그 article 상세 페이지를 조회하는 url을 실행함
-   두 번째 인자로 해당 기사의 pk값이 필요

<br>

### 4-2. GET & POST

-   데이터에 `어떤 요청(행동)`을 원하는지를 나타내는 것

<br>

### - GET method

-   특정 리소스를 `조회`하는 요청
-   반드시 데이터를 가져올 경우에만 사용해야 함
-   GET으로 데이터를 전달하면 `Query String` 형식으로 보내짐

```
ex) http://127.0.0.1:8000/articles/create/?title=제목&content=내용
```

-   조회하는 데이터 : ? 뒤의 title=제목&content=내용

<br>

![네이버검색](../assets/img/http_get_naver_search.png)

<네이버 검색 시 GET method를 통해 Query String 형태로 url에 담겨 전달>

<br>

### - POST method

-   특정 리소스에 `변경사항`을 만드는 요청
-   HTTP Body에 데이터가 담겨 보내진다.

```
ex) http://127.0.0.1:8000/articles/create/?title=제목&content=내용
```

-   변경하는 데이터 : ? 뒤의 title=제목&content=내용

<br>

```html
<!-- templates/articles/new.html -->

<form action="{% url 'articles:create' %}" method="POST">...</form>
```

-   이 경우, html의 form 태그의 method 속성을 `method="POST"`로 해야함

<br>

```python
# articles/views.py

def create(request):
    title = request.POST.get('title')
    content = request.POST.get('content')
    ...
```

-   view 함수에서도 request 데이터 참조 시, `request.POST.get()`으로 POST사용
-   하지만 이 상태에서 실행 시, `403 에러`가 발생
-   `CSRF Token이 누락되었다`는 메시지가 뜨게 됨

<br>

![403에러](../assets/img/http_post_403_error.png)

<POST method 사용 시 403 에러 발생>

<br>

### - CSRF

-   `Cross-Site-Request-Forgery`의 약자
-   사이트 간 요청 위조의 뜻을 가지고 있음
-   웹페이지의 `보안에 취약`하여 수정, 삭제 등의 작업을 하게 만드는 공격 방법으로 위험하다.

<br>

### - Security Token (CSRF Token)

-   대표적인 CSRF 방어 방법으로 서버는 사용자 입력 데이터에 `임의의 난수 값(Token)`을 부여함
-   매 요청마다 해당 Token을 포함하여 전송시키도록 함
-   서버에서 요청 받을 때마다 전달된 Token 값이 `유효한지 검증`하여 응답을 수행함

<br>

```html
<!-- templates/articles/new.html -->

<form action="{% url 'articles:create' %}" method="POST">{% csrf_token %}</form>
```

-   html에서 DTL 태그인 `csrf_token` 태그를 사용하여 사용자에게 토큰 값을 부여하고 이후, 요청마다 토큰 값도 함께 서버로 전송되도록 Django에서 지원함
-   `POST method`의 경우, 데이터 베이스에 변경사항을 만드는 요청이므로 `위변조의 위험을 방지`하기 위해 토큰 값으로 신원확인을 함

<br>

![csrf_token](../assets/img/http_post_csrf_token.png)

<csrf token을 부여함>

<br>
<br>

## 5. 삭제

<삭제 요청하는 url 생성>

```python
# articles/urls.py

urlpatterns = [
    path('<int:pk>/delete/', views.delete, name='delete'),
]
```

<br>

<삭제를 수행하는 delete 함수 생성>

```python
# articles/views.py

def delete(request, pk):
    article = Article.objects.get(pk=pk)
    article.delete()
    return redirect('articles:index')
```

-   어떤 게시글을 삭제할 지 찾기위해 pk 값을 url로 같이 받음
-   pk=pk로 해당 게시글을 찾고 delete 수행
-   삭제 후, index페이지로 리다이렉트된다.

<br>

```html
<!-- articles/detail.html -->

<form action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %} ...
    <input type="submit" value="DELETE" />
</form>
```

-   html에서 form을 통해 delete url로 보내고 method는 POST로 설정
-   POST method 방식을 사용하기에 `csrf_token` 반드시 추가해야함

<br>
<br>

## 6. 수정

### 6-1. 수정 view 함수 로직

-   2가지의 view 함수가 필요하다.
    -   `수정되는 입력을 받는` 페이지를 렌더링 `edit`
    -   ` 수정한 데이터를 DB에 저장`하는 `update`

<br>

### - edit 로직 작성

<수정 페이지로 이동을 요청하는 url 작성>

```python
# articles/urls.py

urlpatterns = [
    path('<int:num>/edit/', views.edit, name="edit"),
]
```

<br>

<기존의 데이터를 담은 수정 페이지를 렌더링해주는 edit 함수 작성>

```python
# articles/views.py

def edit(request, pk):
    article = Article.objects.get(pk=pk)
    context = {
        'article': article,
    }
    return render(request, 'articles/edit.html', context)
```

-   pk로 어떤 기사를 수정할지 모델에서 찾아 article 변수에 담기
-   context 객체에 키 'article'에 변수 article을 값으로 지정
-   edit.html을 렌더링하고 context를 함께 전달하기

<br>

<edit 템플릿 렌더링하기>

```html
<!-- articles/edit.html -->

<h1>EDIT</h1>
<form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %}
    <div>
        <label for="title">Title: </label>
        <input type="text" name="title" id="title" value="{{ article.title }}" />
    </div>
    <div>
        <label for="content">Content: </label>
        <textarea name="content" id="content">{{ article.content }}</textarea>
    </div>
    <input type="submit" />
</form>
```

-   form을 생성하고 변경되는 정보를 보내기 때문에 method는 POST로 지정
-   POST method는 csrf_token 반드시 추가
-   수정 시, 이전에 작성된 내용이 출력되도록 `input`의 value 속성에는 기존의 데이터 `article.title`을 넣고 `textarea` 태그는 `article.content`를 받음

<br>

### - update 로직 작성

<수정 정보를 저장을 요청하는 url 생성>

```python
# articles/urls.py

urlpatterns = [
    path('<int:num>/update/', views.update, name='update'),
]
```

<br>

<수정을 저장할 update 함수 작성>

```python
# articles/views.py

def update(request, pk):
    article = Article.objects.get(pk=pk)
    article.title = request.POST.get('title')
    article.content = request.POST.get('content')
    article.save()
    return redirect('articles:detail', article.pk)
```

-   받은 pk로 모델 Article에서 해당 기사를 찾아 `title`, `content`에 request.POST 객체의 정보를 담아 저장한다.
-   저장이 완료되면 해당 기사의 상세 페이지로 리다이렉트된다.
