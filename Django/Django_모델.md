# Model

모델이란 데이터에 대한 `정보소스`로서 저장된 데이터의 필드와 동작을 포함하는 `클래스`이며, 각각의 모델은 `하나의 DB 테이블에 매핑(mapping : 서로 연결됨)`된다.

각각의 모델은 파이썬 클래스로, django.db.models.Model에 속합니다.

<br>
<br>

---

## (1) model 클래스 생성

```python
# 앱폴더 / models.py
# 예시(Todo 리스트 작성 앱)

from django.db import models


class Todo(models.Model):
    content = models.CharField(max_length=80)
    completed = models.BooleanField(default=False)
    priority = models.IntegerField(default=3)
    created_at = models.DateField(auto_now_add=True)
    deadline = models.DateField(null=True)
```

기존 SQL과 달리 Django 프레임워크에서는 `파이썬의 클래스`를 이용하여, `DB 테이블`을 만들게 된다.

- 테이블명 : Todo
- 필드명 : content / complete / priority / created_at / deadline
- 필드 타입 : CharField / BooleanField / IntegerField / DateField
- 제약 조건 : max_length / default / auto_now_add / null


<필드 타입>

필드 타입은 무수히 많으므로 모든 필드를 암기하기는 어려우며, 필요한 필드를 찾아서 사용해야한다.

ex) 필드 타입 예시

1. CharField() : `길이의 제한이 있는 문자열`로 필드 최대길이를 한정하기 위한 `'max_length'`를 필수인자로 가진다.

2. TextField() : `글자수가 많은 문자열` 로 최대길이를 지정하지 않는다.

3. DateTimeField() : `날짜` 및 `시간`을 넣을 때 사용한다.
    - `auto_now` : 데이터가 저장될 때마다 현재 날짜 및 시간을 저장하는 인자
    - `auto_now_add` : 데이터가 처음 생성될 때만 현재 날짜 및 시간을 저장하는 인자

<br>
<br>

---
## (2) Migrations

model 클래스에서 발생한 `변경사항(필드 생성, 추가 등등)`을 DB에 최종적으로 `반영`하기위한 과정을 Migrations이라고 한다.

마이그레이션 과정은 git을 통해 ① add 한 후, ② commit하는 과정과 같이 `2단계를 통해서 DB에 반영`된다.

![](../img/django_migrations.jpg)

<migrations 과정>

<br>

- STEP1 : 모델에 작성된 클래스를 migration 파일로 만든다.

```bash
$ python manage.py makemigrations
```

- STEP2 : 최종적으로 DB 테이블에 적용한다.

```bash
$ python manage.py migrate
```

<br>

![](../img/django_db.png)

<db sqlite3 에 형성된 DB테이블>

- 테이블 이름은 앱이름_클래스명으로 이루어진다.

<br>
<br>

---
## (3) Admin site(관리자 사이트)

Django의 경우, 관리자 인터페이스를 `기본적으로 제공`하고 있다. 따라서 데이터를 `테스트`하거나 확인하기에 유용하다.

<br>
<br>

### 1) admin 계정 생성

```bash
$ python manage.py createsuperuser
```

명령어를 입력하면, `관리자 이름`, `이메일`, `비밀번호`, `비밀번호 확인`을 입력하게 된다.

이메일의 경우, 선택사항이며, 비밀번호는 보안을 위해 터미널에 출력되지 않으므로 무시하고 입력하면 된다.

최종적으로 계정생성이 완료되면, DB의 `auth_user` 테이블에 admin계정이 추가되게 된다.

<br>
<br>

### 2) admin에 모델 클래스 등록

```bash
# todos / admin.py

from django.contrib import admin
from .models import Todo

admin.site.register(Todo)
# 암기 : '관리자' '사이트'에 '등록'한다.
```

admin.py에 모델 클래스를 `등록`해야 관리자 사이트에서 확인할 수 있다.

등록이 완료되면, 관리자 사이트를 통해서 생성, 읽기, 수정, 삭제인 `CRUD 테스트`가 가능하다. 변경사항은 db.sqlite3 파일에서도 확인할 수 있다.

<br>
<br>

---
## (4) 테이블을 수정할 경우

<이미 migrations 된 테이블에 필드를 추가하는 경우>

1. 기존의 models.py에 생성된 클래스에 필드를 추가하고, 동일하게 makemigrations 를 진행한다.

2. 터미널에서 추가하는 필드의 기본값의 설정이 필요하다는 안내창이 뜨게 된다.

![](../img/django_update.png)

<기본값을 위한 2가지 선택사항>

    1번의 경우, Django를 통해 자동으로 기본값을 입력받는 방법

    2번의 경우, 안내에서 나와서 본인이 직접 기본값을 설정하는 방법

3. 이 후, makemigrations 완료하면, 앱의 migrations 폴더에 파일이 생성된다.

4. 최종적으로 migrate하면 변경사항이 DB 테이블에 적용된다.