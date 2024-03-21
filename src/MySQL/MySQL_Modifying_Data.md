# 데이터 조작하기

## 목차

1. [Insert data into table](#1-insert-data-into-table)
    1. [INSERT syntax](#1-1-insert-syntax)
        - [ex1) INSERT 사용하여 단일 데이터 삽입](#ex1-insert-사용하여-단일-데이터-삽입)
        - [ex2) INSERT 사용하여 다중 데이터 삽입](#ex2-insert-사용하여-다중-데이터-삽입)
        - [ex3) INSERT 사용하여 데이터 삽입 시, 날짜 자동입력](#ex3-insert-사용하여-데이터-삽입-시-날짜-자동입력)
2. [Update data in table](#2-update-data-in-table)
    1. [UPDATE syntax](#2-1-update-syntax)
        - [ex1) UPDATE 사용하여 1개의 필드 값 수정](#ex1-update-사용하여-1개의-필드-값-수정)
        - [ex2) UPDATE 사용하여 2개의 필드 값 수정](#ex2-update-사용하여-2개의-필드-값-수정)
        - [ex3) UPDATE 사용하여 필드 값 수정(REPLACE 활용)](#ex3-update-사용하여-필드-값-수정replace-활용)
3. [Delete data from table](#3-delete-data-from-table)
    1. [DELETE syntax](#3-1-delete-syntax)
        - [ex1) DELETE 사용하여 레코드 삭제 1](#ex1-delete-사용하여-레코드-삭제-1)
        - [ex2) DELETE 사용하여 레코드 삭제 2](#ex2-delete-사용하여-레코드-삭제-2)

<br>
<br>

## 1. Insert data into table

### 1-1. INSERT syntax

```SQL
INSERT INTO table_name (c1, c2, ...)
VALUES (v1, v2, ...);
```

-   테이블 레코드 삽입
-   INSERT INTO 절 다음에 테이블 이름과 괄호 안에 필드 목록을 작성
-   VALUES 키워드 다음 괄호 안에 해당 필들에 삽입할 값 목록을 작성

<br>

### - 예시 기초 테이블 생성

```SQL
-- articles 테이블

CREATE TABLE articles (
    id INT AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL,
    content VARCHAR(200) NOT NULL,
    createAt DATE NOT NULL,
    PRIMARY KEY (id)
);
```

<br>

### ex1) INSERT 사용하여 단일 데이터 삽입

-   articles 테이블에 각 필드에 적합한 데이터 입력
    -   createdAt 필드 값은 2000년 1월 1일이며 title과 content 필드 값은 자율

정답)

| id  | title | content | createdAt  |
| :-: | :---: | :-----: | :--------: |
|  1  | hello |  world  | 2000-01-01 |

```SQL
INSERT INTO
    articles (title, content, createdAt)
VALUES
    ('hello', 'world', '2000-01-01');
```

<br>

### ex2) INSERT 사용하여 다중 데이터 삽입

-   articles 테이블의 각 필드에 적합한 데이터 3개 넣기
    -   모든 필드 값 자율

정답)

| id  | title  | content  | createdAt  |
| :-: | :----: | :------: | :--------: |
|  1  | hello  |  world   | 2000-01-01 |
|  2  | title1 | content1 | 1900-01-01 |
|  3  | title2 | content2 | 1800-01-01 |
|  4  | title3 | content3 | 1700-01-01 |

```SQL
INSERT INTO
    articles (title, content, createdAt)
VALUES
    ('title1', 'content1', '1900-01-01'),
    ('title2', 'content2', '1800-01-01'),
    ('title3', 'content3', '1700-01-01');
```

<br>

### ex3) INSERT 사용하여 데이터 삽입 시, 날짜 자동입력

-   articles 테이블에 각 필드에 적합한 데이터 입력
    -   createdAt 필드에는 현재 작성하는 날짜가 자동으로 입력, 나머지는 자율

정답)

| id  |  title  |  content  | createdAt  |
| :-: | :-----: | :-------: | :--------: |
|  1  |  hello  |   world   | 2000-01-01 |
|  2  | title1  | content1  | 1900-01-01 |
|  3  | title2  | content2  | 1800-01-01 |
|  4  | title3  | content3  | 1700-01-01 |
|  5  | mytitle | mycontent | 현재 날짜  |

```SQL
INSERT INTO
    articles (title, content, createdAt)
VALUES
    ('mytitle', 'mycontent', CURDATE());
```

-   `CURDATE()` : 현재 날짜를 반환, MySQL이 제공하는 Date Function 중 하나

<br>
<br>

## 2. Update data in table

### 2-1. UPDATE syntax

```SQL
UPDATE table_name
SET column_name = expression,
[WHERE
  condition];
```

-   테이블 레코드 수정
-   SET 절 다음에 수정할 필드와 새 값을 지정
-   WHERE 절에서 수정할 레코드를 지정하는 조건 작성(WHERE 절 미 작성시, 모든 레코드 수정)

<br>

### ex1) UPDATE 사용하여 1개의 필드 값 수정

-   articles 테이블 1번 레코드의 title 필드 값을 'newTitle'로 변경

정답)

| id  |  title   |  content  | createdAt  |
| :-: | :------: | :-------: | :--------: |
|  1  | newTitle |   world   | 2000-01-01 |
|  2  |  title1  | content1  | 1900-01-01 |
|  3  |  title2  | content2  | 1800-01-01 |
|  4  |  title3  | content3  | 1700-01-01 |
|  5  | mytitle  | mycontent | 현재 날짜  |

```SQL
UPDATE
    articles
SET
    title = 'newTitle'
WHERE
    id = 1;
```

<br>

### ex2) UPDATE 사용하여 2개의 필드 값 수정

-   articles 테이블 2번 레코드의 title, content 필드 값을 자유롭게 변경

정답)

| id  |   title   |   content   | createdAt  |
| :-: | :-------: | :---------: | :--------: |
|  1  | newTitle  |    world    | 2000-01-01 |
|  2  | newTitle2 | newContent2 | 1900-01-01 |
|  3  |  title2   |  content2   | 1800-01-01 |
|  4  |  title3   |  content3   | 1700-01-01 |
|  5  |  mytitle  |  mycontent  | 현재 날짜  |

```SQL
UPDATE
    articles
SET
    title = 'newTitle',
    content = 'newContent'
WHERE
    id = 2;
```

<br>

### ex3) UPDATE 사용하여 필드 값 수정(REPLACE 활용)

-   articles 테이블 모든 레코드의 content 필드 값 중 문자열 'content'를 'TEST'로 변경

정답)

| id  |   title   |   content   | createdAt  |
| :-: | :-------: | :---------: | :--------: |
|  1  | newTitle  |    world    | 2000-01-01 |
|  2  | newTitle2 | newContent2 | 1900-01-01 |
|  3  |  title2   |    TEST2    | 1800-01-01 |
|  4  |  title3   |    TEST3    | 1700-01-01 |
|  5  |  mytitle  |   myTEST    | 현재 날짜  |

```SQL
UPDATE
    articles
SET
    content = REPLACE(content, 'content', 'TEST');
```

-   `REPLACE()` : 지정된 문자열 변경, MySQL이 제공하는 String Functions 중 하나

<br>
<br>

## 3. Delete data from table

### 3-1. DELETE syntax

```SQL
DELETE FROM table_name
[WHERE
  condition];
```

-   테이블 레코드 삭제
-   DELETE FROM 절 다음에 테이블 이름 작성
-   WHERE 절에서 삭제할 레코드 지정하는 조건 작성(WHERE 절을 작성하지 않으면, 모든 레코드 삭제)

<br>

### ex1) DELETE 사용하여 레코드 삭제 1

-   articles 테이블의 1번 레코드 삭제

정답)

| id  |   title   |   content   | createdAt  |
| :-: | :-------: | :---------: | :--------: |
|  2  | newTitle2 | newContent2 | 1900-01-01 |
|  3  |  title2   |    TEST2    | 1800-01-01 |
|  4  |  title3   |    TEST3    | 1700-01-01 |
|  5  |  mytitle  |   myTEST    | 현재 날짜  |

```SQL
DELETE FROM
    articles
WHERE
    id = 1;
```

<br>

### ex2) DELETE 사용하여 레코드 삭제 2

-   articles 테이블에서 가장 최근 작성된 레코드 2개 삭제

정답)

| id  | title  | content | createdAt  |
| :-: | :----: | :-----: | :--------: |
|  3  | title2 |  TEST2  | 1800-01-01 |
|  4  | title3 |  TEST3  | 1700-01-01 |

```SQL
DELETE FROM
    articles
ORDER BY
    createdAt DESC
LIMIT 2;
```
