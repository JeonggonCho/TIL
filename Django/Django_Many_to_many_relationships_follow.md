# M:N 관계 (Follow 기능)

# 목차

1. [Profile](#1-profile)
    1. [프로필 페이지](#1-1-프로필-페이지)
        - [프로필 url 설정](#--프로필-url-설정)
        - [프로필 view 함수 작성](#--프로필-view-함수-작성)
        - [프로필 템플릿 작성](#--프로필-템플릿-작성)
2. [User & User](#2-user--user)
    1. [Many to many relationships](#2-1-many-to-many-relationships)
    2. [Follow 구현](#2-2-follow-구현)
        - [User 모델 수정](#--user-모델-수정)
        - [Follow 기능 url 설정](#--follow-기능-url-설정)
        - [Follow 기능 view 함수 작성](#--follow-기능-view-함수-작성)
        - [프로필 유저의 팔로잉/팔로워 수 및 팔로우/언팔로우 버튼 출력](#--프로필-유저의-팔로잉팔로워-수-및-팔로우언팔로우-버튼-출력)

<br>
<br>

## 1. Profile

### 1-1. 프로필 페이지

### - 프로필 url 설정

```python
# accounts/urls.py

urlpatterns = [
    ...,
    path('profile/<username>', views.profile, name='profile'),
]
```

<br>

### - 프로필 view 함수 작성

```python
# accounts/views.py

from django.contrib.auth import get_user_model


def profile(request, username):
    User = get_user_model()
    person = User.objects.get(username=username)
    context = {
        'person': person,
    }
    return render(request, 'accounts/profile.html', context)
```

<br>

### - 프로필 템플릿 작성

```html
<!--accounts/profile.html-->

<h1>{{ person.username }}님의 프로필</h1>

<hr>

<h2>{{ person.username }}님의 게시글</h2>
{% for article in person.article_set.all %}
<div>{{ article.title }}</div>
{% endfor %}

<hr>

<h2>{{ person.username }}님의 댓글</h2>
{% for comment in person.comment_set.all %}
<div>{{ comment.content }}</div>
{% endfor %}

<hr>

<h2>{{ person.username }}님이 좋아요한 게시글</h2>
{% for article in person.like_articles.all %}
<div>{{ article.title }}</div>
{% endfor %}
```

```html
<!--articles/index.html-->

<a href="{% url 'accounts:profile' user.username %}">내 프로필</a>

<p>작성자 : <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></p>
```

<br>
<br>

## 2. User & User

### 2-1. Many to many relationships

- 유저는 0명 이상의 다른 유저와 관련됨
- 유저는 다른 유저로부터 0개 이상의 팔로우를 받을 수 있고, 유저는 0명 이상의 다른 유저들에게 팔로잉을 할 수 있음

<br>

### 2-2. Follow 구현

### - User 모델 수정

```python
# accounts/models.py

class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='follewers')
```

<br>

### - Follow 기능 url 설정

```python
# accounts/urls.py

urlpatterns = [
    ...,
    path('<int:user_pk>/follow/', views.follow, name='follow'),
]
```

<br>

### - Follow 기능 view 함수 작성

```python
# accounts/views.py

@login_required
def follow(request, user_pk):
    User = get_user_model()
    person = User.objects.get(pk=user_pk)
    if person != request.user:
        if person.followers.filter(pk=request.user.pk).exists():
            person.followers.remove(request.user)
        else:
            person.followers.add(request.user)
    return redirect('accounts:profile', person.username)
```

<br>

### - 프로필 유저의 팔로잉/팔로워 수 및 팔로우/언팔로우 버튼 출력

```html
<!--accounts/profile.html-->

<div>
    <div>
        팔로잉 : {{ person.followings.all|length }} / 팔로워 : {{ person.followers.all|length }}
    </div>
    {% if request.user != person %}
    <div>
        <form action="{% url 'accounts:follow' person.pk %}" method="POST">
            {% csrf_token %}
            {% if request.user in person.followers.all %}
            <input type="submit" value="Unfollow">
            {% else %}
            <input type="submit" value="Follow">
            {% endif %}
        </form>
    </div>
    {% endif %}
</div>
```