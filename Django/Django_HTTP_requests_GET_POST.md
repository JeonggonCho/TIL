# HTTP requests (GET/POST)

## 목차

1. [HTTP requests 비교](#1-http-requests-비교)
2. [view 함수의 변화](#2-view-함수의-변화)
    1. [new와 create 함수 결합](#2-1-new와-create-함수-결합)
        - [기존에 분리되어있던 new 함수와 create 함수](#기존에-분리되어있던-new-함수와-create-함수)
        - [new 함수와 create 함수 합치기](#new-함수와-create-함수-합치기)
        - [불필요한 new url 제거](#불필요한-new-url-제거)
        - [new와 관련되었던 url 모두 수정](#new와-관련되었던-url-모두-수정)
        - [create 정리](#create-정리)
    2. [edit과 update 함수 결합](#2-2-edit과-update-함수-결합)
        - [합쳐진 update 함수](#합쳐진-update-함수)
        - [불필요한 edit url 제거](#불필요한-edit-url-제거)
        - [edit과 관련되었던 url 모두 수정](#edit과-관련되었던-url-모두-수정)
        - [update 정리](#update-정리)

<br>
<br>

## 1. HTTP requests 비교

-   생성화면을 요청하는 함수 new와 생성을 요청하는 create 함수 비교

|         공통점          |                        차이점                        |
| :---------------------: | :--------------------------------------------------: |
| 데이터 생성 로직을 구현 | new는 GET method 요청<br>create는 POST method를 요청 |

<br>
<br>

## 2. view 함수의 변화

### 2-1. new와 create 함수 결합

### - 기존에 분리되어있던 new 함수와 create 함수

```python
# articles/views.py

def new(request):
    form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'acticles/new.html', context)

def create(request):
    form = ArticleForm(request.POST)
    if form.is_valid():
        article = form.save()
        return redirect('articles:detail', article.pk)
    context = {
        'form': form,
    }
    return render(request, 'articles/new.html', context)
```

<br>

### - new 함수와 create 함수 합치기

```python
# articles/views.py

def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/create.html', context)
```

-   요청 받은 request의 method가 `조건문`으로 POST인지 GET인지에 따라서 `분기 처리`하도록 함
-   `POST`이면 form에 request.POST를 담고 해당 form이 유효하면 article로 저장하고 해당 pk값의 상세 페이지로 리다이렉트 시킴
    -   form이 유효하지 않으면 생성 페이지 new.html로 form을 담은 context와 함께 다시 렌더링
-   `GET`이면 생성 페이지 new.html로 form을 담은 context와 함께 렌더링

<br>

### - 불필요한 new url 제거

```python
# articles/urls.py

app_name = 'articles'
urlpatterns = [
    # path('new/', views.new, name='new'),
]
```

<br>

### - new와 관련되었던 url 모두 수정

```html
<!-- articles/index.html -->
<a href="{% url 'articles:create' %}">CREATE</a>

<!-- articles/create.html -->
<form action="{% url 'articles:create %}" method="POST">{% csrf_token %}</form>
```

<br>

### - create 정리

-   (GET) articles/create/ -> 게시글 생성 페이지로 이동
-   (POST) articles/create/ -> 게시글 생성

<br>

### 2-2. edit과 update 함수 결합

### - 합쳐진 update 함수

```python
# articles/views.py

def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            form.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm(instance=articlet)
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```

-   동일하게 request의 method에 따라서 POST의 경우와 GET의 경우를 분기 처리함

<br>

### - 불필요한 edit url 제거

```python
# articles/urls.py

app_name = 'articles'
urlpatterns = [
    # path('<int:pk>/edit/', views.edit, name='edit'),
]
```

<br>

### - edit과 관련되었던 url 모두 수정

```html
<!-- articles/detail.html -->
<a href="{% url 'articles:update' article.pk %}">UPDATE</a>

<!-- articles/update.html -->
<form action="{% url 'articles:update article.pk %}" method="POST">{% csrf_token %}</form>
```

<br>

### - update 정리

-   (GET) articles/pk/update/ -> 게시글 수정 페이지로 이동
-   (POST) articles/pk/update/ -> 게시글 수정
