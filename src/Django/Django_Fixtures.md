# Fixtures

## 목차

1. [fixtures](#1-fixtures)
    1. [초기 데이터](#1-1-초기-데이터)
    2. [초기 데이터 생성하기 - dumpdata](#1-2-초기-데이터-생성하기---dumpdata)
    3. [초기 데이터 불러오기 - loaddata](#1-3-초기-데이터-불러오기---loaddata)
        - [fixtures 기본 경로](#--fixtures-기본-경로)
        - [loaddata 순서 주의](#--loaddata-순서-주의)
2. [참고](#2-참고)
    1. [모든 모델 한 번에 dumpdata](#2-1-모든-모델-한-번에-dumpdata)

<br>
<br>

## 1. fixtures

- Django가 데이터 베이스로 가져오는 방법을 알고있는 데이터 모음
- Django가 직접 생성하기에 데이터 베이스 구조에 맞게 작성됨
- Django는 fixtures를 이용하여 모델에 `초기 데이터를 제공`함

<br>

### 1-1. 초기 데이터

- A, B 개발자가 GitHub를 이용하여 협업할 경우, `.gitignore` 설정으로 인해 DB는 GitHub에 업로드되지 않음
- 하지만, 프로젝트 개발 시, 준비된 데이터로 데이터 베이스를 미리 채우는 것이 필요할 수 있음

<br>

### 1-2. 초기 데이터 생성하기 - dumpdata

- 데이터 추출하여 초기 데이터를 `생성`하는 명령어
- 데이터 베이스의 모든 데이터를 출력하여 여러 모델을 하나의 `JSON` 파일로 만듦

<br>

```bash
# dumpdata 작성 구조

$ python manage.py dumpdata [app_name[.ModelName] app_name[.ModelName] ...] > filename.json


# 작성 예시

$ python manage.py dumpdata --indent 4 articles.article > articles.json
```

- 의미 : articles 앱의 article 모델을 담은 aricles.json의 초기 데이터 생성
- `--indent 4` : 해당 구문을 넣으면 json 파일에 탭(4칸 들여쓰기)을 적용하여 `깔끔하게 출력`되도록 할 수 있음 (해당 구문이 없을 경우, 한 줄로 늘여진 json 파일 생성됨)

<br>

![생성된 json 파일](../assets/img/django_making_fixtures.png)

<생성된 json 파일 확인>

<br>

### 1-3. 초기 데이터 불러오기 - loaddata

- 생성한 fixtures 데이터를 데이터 베이스에 `불러오기`

<br>

```bash
# loaddata 작성 구조

$ python manage.py loaddata filename.json ...


# 작성 예시

$ python manage.py loaddata articles.json comments.json
```

<br>

### - fixtures 기본 경로

- `app_name/fixtures/` : Django는 설치된 모든 app의 디렉토리에서 fixtures 폴더 이후의 경로를 통해 fixtures 파일을 찾아서 불러옴

```
# 해당 위치로 fixtures 파일을 이동 시킴

articles/
    fixtures/
        articles.json
        comments.json
        ...
```

- 기존의 `db.sqlite3` 파일을 `삭제`하고 `migrate` 진행

<br>

### - loaddata 순서 주의

- loaddata를 한 번에 실행하지 않고, 하나씩 진행한다면, `모델 관계`에 따라서 `순서가 중요함`
- ex) comment는 article에 대한 key 및 user에 대한 key가 필요함, article은 user에 대한 key가 필요함
- 서로 참조 관계에 따라 `user -> article -> comment` 순으로 loaddata를 진행해야 오류가 발생하지 않음

```bash
$ python manage.py loaddata users.json
$ python manage.py loaddata articles.json
$ python manage.py loaddata comments.json
```

<br>
<br>

## 2. 참고

### 2-1. 모든 모델 한 번에 dumpdata

```bash
# 3개 모델을 하나의 JSON 파일로 dump 하기

$ python manage.py dumpdata --indent 4 articles.article articles.comment accounts.user > data.json


# 모든 모델을 하나의 JSON 파일로 dump 하기

$ python manage.py dumpdata --indent 4 > data.json
```