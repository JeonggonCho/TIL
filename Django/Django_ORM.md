# ORM
ORM이란, Object-Relational-Mapping(객체 지향 연결)의 줄임말로 객체지향 프로그래밍 언어(ex. 파이썬, 자바스크립트 등)을 사용하여 이를 지원하지 않는 시스템과의 `호환을 가능하게 하도록 데이터를 변환`해주는 프로그래밍 기술을 말한다.

Django에서는 파이썬을 통하여, SQL언어를 사용하는 DB를 조작할 수 있도록 한다.

<br>
<br>

---
## (1) QuerySet API

QuerySet API는 ORM에서 데이터를 조작하는데 사용하는 다양한 `도구모음`이다.

```python
# QuerySet API 구문

[Model class name].objects.[QuerySet API]

ex) Article.objects.all()
```

참고!! [Django - QuerySet API](https://docs.djangoproject.com/en/4.1/ref/models/querysets/)

<br>
<br>

## 1) Query

`쿼리`란 DB에 보내는 일종의 `요청`으로 SQL에서의 SELECT문을 예로 들 수 있다.

Django에서는 파이썬으로 작성된 코드가 ORM을 통해 SQL로 변환된 후, DB에 전달되며, 응답을 `QuerySet의 자료 형태로 반환`된다.

<br>
<br>

## 2) QuerySet

QuerySet은 DB에서 응답받은 데이터모음이다. 이는 리스트의 형태로 순회가능한 데이터이다.

단, 단일한 객체를 반환 받을 경우, QuerySet이 아닌, 모델 클래스의 인스턴스로 반환된다.

<br>
<br>

---
## (2) ORM 생성

ORM을 활용하기 위해서는 초기세팅이 필요하다.

```bash
# 1. 패키지 설치

$ pip install ipython
$ pip install django_extensions


# 2. Django 확장프로그램 앱에 등록

# settings.py

INSTALLED_APPS = [
  'django_extensions',
  ...
]


# 3. 패키지 최신화

$ pip freeze > requirements.txt
```

```bash
# Django에서 파이썬 shell 실행

$ python manage.py shell_plus
```

```bash
# 데이터 객체 생성하기

# 방법1
# ex)

$ article = Article() # 클래스(Article)에서 인스턴스(article) 할당
$ article.title = 'first' # 인스턴스 변수(title)에 값 할당
$ article.content = 'Django!' # 인스턴스 변수(content)에 값 할당
$ article.save() # 할당한 값 DB에 저장

# 방법2
# ex)

$ article = Article(title='first', content='Django!') # 변수를 할당한 클래스를 통해 인스턴스 생성
$ article.save() # 저장 메서드

# 방법3
# ex)

$ Article.objects.create(title='first', content='Django!') # QuerySet API의 create() 메서드를 통해 DB에 생성 및 저장

```

<br>
<br>

---
## (3) ORM 조회

```bash
# 조회

# 전체 데이터 조회 all()메서드

$ Article.objects.all()


# 단일 데이터 조회 get()메서드
# 메서드 안에 고유성을 보장하는 특징을 삽입

$ Article.objects.get(pk=1)

# 특정 조건 데이터 조회 filter() 메서드
# 메서드 안에 특정 조건을 삽입

$ Article.objects.filter(content='Django!')