# Form

## 목차

1. [HTML Form](#1-html-form)
    1. [유효성 검사](#1-1-유효성-검사)
2. [Django Form](#2-django-form)
    1. [Django Form 사용](#2-1-django-form-사용)
        - [Form class 선언](#form-class-선언)
        - [Form class를 적용한 new 로직](#form-class를-적용한-new-로직)
        - [HTML에서 사용](#html에서-사용)
        - [Form rendering option](#form-rendering-option)
3. [Widgets](#3-widgets)
4. [Django ModelForm](#4-django-modelform)
    1. [ModelForm 사용](#4-1-modelform-사용)
        - [ModelForm class 선언](#modelform-class-선언)
        - [Meta class](#meta-class)
        - [fields 및 exclude 속성](#fields-및-exclude-속성)
    2. [ModelForm을 적용한 create 로직](#4-2-modelform을-적용한-create-로직)
        - [is_valid() 메서드](#is_valid-메서드)
    3. [ModelForm을 적용한 edit 로직](#4-3-modelform을-적용한-edit-로직)
    4. [ModelForm을 적용한 update 로직](#4-4-modelform을-적용한-update-로직)
        - [save() 메서드](#save-메서드)
5. [참고](#5-참고)
    1. [ModelForm 키워드 인자 data와 instance](#5-1-modelform-키워드-인자-data와-instance)
    2. [Widget의 속성 사용하기](#5-2-widget의-속성-사용하기)
    3. [Meta class](#5-3-meta-class)

<br>
<br>

## 1. HTML Form

-   사용자로부터 form 요소를 통해 데이터를 받고 있으나 비정상적인 혹은 악의적인 요청을 확인하지 않고 `모두 수용`
-   데이터 형식이 맞는지에 대한 `유효성 검증`이 필요하다.

<br>

### 1-1. 유효성 검사

-   수집한 데이터가 `정확하고 유효한지` 확인하는 과정
    -   ex) 아이디, 패스워드, 이메일...
-   이러한 유효성 검사에는 입력 값, 형식, 중복, 범위, 보안 등 부가적인 `많은 것에 대한 고려`가 필요함
-   Django에서는 이러한 과정과 기능에 대해 지원해주는 도구를 제공

<br>
<br>

## 2. Django Form

-   사용자의 입력 데이터를 수집 및 유효성을 검증하고 처리를 수행하기 위한 도구
-   유효성 검사를 `단순화하고 자동화`하는 기능 제공

<br>

### 2-1. Django Form 사용

### - Form class 선언

```python
# articles/forms.py

from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField()
```

-   Django Form은 `forms.py` 파일에서 클래스로 관리함
-   `필드 타입`을 지정할 수 있음
    -   [Django - Field-types](https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-types)

<br>

### - Form class를 적용한 new 로직

```python
# articles/views.py

from .forms import ArticleForm

def new(request):
    form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/new.html', context)
```

-   forms.py에서 생성한 ArticleForm을 views.py에 import하여 사용
-   모델에서 QuerySet API를 통해 조회한 데이터를 context를 통해서 html파일로 넘겨주는 것과 동일하게 import한 해당 클래스 ArticleForm을 form 인스턴스로 생성하고 context에 담아 html파일로 렌더링시 전달하여 사용가능

<br>

### - HTML에서 사용

```html
<!-- articles/new.html -->

<h1>NEW</h1>
<form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %} {{ form }}
    <input type="submit" />
</form>
```

-   html에서 `{{ <form이름> }}`과 같이 중괄호 2개를 사용하여 해당 form을 사용가능

<br>

### - Form rendering option

```html
<!-- articles/new.html -->

<h1>NEW</h1>
<form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %} {{ form.as_p }}
    <input type="submit" />
</form>
```

-   form에 `as_p`, `as_div`, `as_ul`, `as_table`과 같이 각 필드를 어떤 html요소로 rendering할지 지정하는 옵션을 제공한다.

<br>
<br>

## 3. Widgets

-   HTML `input` element의 표현을 담당

<br>

```python
# articles/forms.py

from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField(widget=forms.Textarea)
```

-   해당 Form의 input요소의 속성 및 `출력되는 부분`을 변경하는 것
-   즉 `field`는 `유효성을 검증`하며 `widget`은 단순한 `렌더링을 처리`하는 차이가 있다.
    -   [Django - widgets](https://docs.djangoproject.com/en/5.0/ref/forms/widgets/)

<br>
<br>

## 4. Django ModelForm

-   작성된 model을 기반으로 Form을 구성
-   `Form` : 사용자 입력 데이터를 데이터 베이스에 저장하지 않을 경우
    -   ex) 로그인
-   `ModelForm` : 사용자의 입력 데이터를 데이터 베이스에 저장해야하는 경우
    -   ex) 회원가입

<br>

### 4-1. ModelForm 사용

### - ModelForm class 선언

```python
# articles/forms.py

from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    class Meta:
        model = Article
        fields = '__all__'
```

-   기존의 AricleForm 수정

<br>

### - Meta class

-   ModelForm의 정보를 작성하는 곳

<br>

### - fields 및 exclude 속성

-   fields와 exclude 속성으로 모델들을 명시적으로 지정하여 사용가능
-   fields와 exclude는 함께 사용되지 않아야 한다.
-   하나를 선택하여 사용하거나 둘 다 사용하지 않는 것 지향함
-   이러한 속성을 사용하여 모델 폼을 정의하면, Django는 해당 모델의 필드에 맞추어 폼을 생성하고, 이를 기반으로 사용자 입력을 처리함

<br>

<fields 속성>

```python
# articles/forms.py

class ArticleForm(forms.ModelForm):
    class Meta:
        model = Article
        field = ('title',)
        # title 필드만 사용
```

-   `fields` : 사용할 모델 필드를 명시적으로 지정하는 데 사용
-   이 속성에 포함된 필드만이 해당 ModelForm에서 사용 가능하다.

<br>

<exclude 속성>

```python
# articles/forms.py

class AricleForm(forms.ModelForm):
    class Meta:
        model = Article
        exclude = ('title',)
```

-   `exclude` : 모델에서 제외하고 싶은 필드들을 명시하는 데 사용
-   모든 모델 필드를 가져오되 특정 필드만 사용하지 않도록 할 때 유용

<br>

### 4-2. ModelForm을 적용한 create 로직

```python
# articles/views.py

from .forms import ArticleForm

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

-   생성 요청을 받아 ArticleForm에 POST요청의 객체를 담아 form 인스턴스를 생성
-   form 인스턴스가 유효하다면(is_valid 메서드 사용), 인스턴스를 저장하고, detail 페이지로 리다이렉트 시킨다.
-   유효하지 않다면(GET요청), 생성 페이지에 form을 담아 렌더링

<br>

### - is_valid() 메서드

-   여러 유효성 검사를 실행하고 데이터의 유효성 여부를 boolean(True/False)로 반환함

<br>

### 4-3. ModelForm을 적용한 edit 로직

```python
# articles/views.py

def edit(request, pk):
    article = Article.objects.get(pk=pk)
    form = ArticleForm(instance=article)
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/edit.html', context)
```

```html
<!-- articles/edit.html -->

<h1>EDIT</h1>
<form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %} {{ form.as_p }}
    <input type="submit" />
</form>
```

-   QuerySet API 메서드 get을 이용하여 해당 pk값을 가지는 article 데이터를 찾아 article 변수에 담는다.
-   article 변수를 ArticleForm의 인스턴스로 담아 form 생성
-   article과 form을 context에 담아 edit페이지 렌더링시 전달

<br>

### 4-4. ModelForm을 적용한 update 로직

```python
# articles/views.py

def update(request. pk):
    article = Article.objects.get(pk=pk)
    form = ArticleForm(request.POST, instance=article)
    if form.is_valid():
        form.save()
        return redirect('articles:detail', article.pk)
    context = {
        'form': form,
    }
    return render(request, 'articles/edit.html', context)
```

-   pk를 통해 수정할 article 데이터를 찾아 article 변수에 저장
-   form에 request.POST와 함께 instance=article을 ArticleForm에 넣어준다.
-   `instance=article`을 안써주면 `새로운 글을 생성`(기존 내용 가져오기)
-   해당 form이 유효하면 저장하고 상세 페이지로 리다이렉트한다.
-   유효하지 않으면 수정 페이지에 form을 담아 전달

<br>

### - save() 메서드

-   데이터 베이스 객체를 만들고 저장
-   키워드 `instance` 여부를 통해 `생성`과 `수정`을 결정

```python
# create
form = ArticleForm(request.POST)
form.save()

# update
form = ArticleForm(request.POST, instance=article)
form.save()
```

<br>
<br>

## 5. 참고

### 5-1. ModelForm 키워드 인자 data와 instance

![ModelForm](../assets/img/django_ModelForm_data_instance.png)

-   ModelForm의 생성자 함수를 살펴보면 self 다음 인자로 data를 받기에 data는 키워드를 사용해주지 않아도 되지만, `instance`는 `중간에 위치한 인자`이기에 instance 키워드를 명시적으로 사용하여 instance가 기존 내용을 받도록 해주어야 함

<br>

### 5-2. Widget의 속성 사용하기

```python
# articles/forms.py

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label="제목",
        widget=forms.TextInput(
            attrs={
                'class': '클래스 적용(css 커스텀)',
                'placeholder': 'title을 입력해주세요',
            }
        )
    )

    class Meta:
        model = Article
        fields = '__all__'
```

-   widget의 `attrs`를 사용하여 class, placeholder등 `input 속성`을 지정할 수 있다.

<br>

```python
# articles/forms.py

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label="제목",
        widget=forms.TextInput(
            attrs={
                'class': 'my-title',
                'placeholder': '제목을 입력해주세요.',
                'maxlength': 10,
            }
        ),
    )

    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={
                'class': 'my-content',
                'placeholder': '내용을 입력해주세요.',
                'rows': 5,
                'cols': 50,
            }
        ),
        error_message={'required': '내용을 입력해주세요.'},
    )

    class Meta:
        model = Article,
        fields = '__all__'
```

<widget의 attrs 활용예시>

<br>

### 5-3. Meta class

-   클래스 내부에서 사용하는 클래스로 파이썬에서는 `Inner class(내부 클래스)`, `Nested class(중첩 클래스)`라고 함
-   모델의 정보를 추상화하여 Meta라는 이름의 내부 클래스에 작성하도록 Django의 ModelForm이 설계되어있으므로 `역할과 사용법에 집중`하는 것을 추천함
