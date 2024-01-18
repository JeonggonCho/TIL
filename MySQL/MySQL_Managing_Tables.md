# 테이블 관리하기

## 목차

1. [Create a table](#1-create-a-table)
    1. [CREATE TABLE syntax](#1-1-create-table-syntax)
        - [CREATE TABLE examples](#create-table-examples)
    2. [대표적인 MySQL 데이터 타입](#1-2-대표적인-mysql-데이터-타입)
    3. [Constraints(제약 조건)](#1-3-constraints제약-조건)
    4. [AUTO_INCREMENT attribute](#1-4-auto_increment-attribute)
        - [AUTO_INCREMENT 특징](#auto_increment-특징)
2. [Delete a table](#2-delete-a-table)
    1. [DROP TABLE syntax](#2-1-drop-table-syntax)
3. [Modifying table fields](#3-modifying-table-fields)
    1. [ALTER TABLE](#3-1-alter-table)
    2. [ALTER TABLE ADD syntax](#3-2-alter-table-add-syntax)
        - [ex1) ADD 사용하여 테이블에 단일 필드 추가](#ex1-add-사용하여-테이블에-단일-필드-추가)
        - [ex2) ADD 사용하여 테이블에 다중 필드 추가](#ex2-add-사용하여-테이블에-다중-필드-추가)
    3. [ALTER TABLE MODIFY syntax](#3-3-alter-table-modify-syntax)
        - [ex1) MODIFY 사용하여 테이블의 단일 필드 수정](#ex1-modify-사용하여-테이블의-단일-필드-수정)
        - [ex2) MODIFY 사용하여 테이블의 다중 필드 수정](#ex2-modify-사용하여-테이블의-다중-필드-수정)
    4. [ALTER TABLE CHANGE COLUMN syntax](#3-4-alter-table-change-column-syntax)
        - [ex1) CHANGE COLUMN 사용하여 필드 변경](#ex1-change-column-사용하여-필드-변경)
    5. [ALTER TABLE DROP COLUMN syntax](#3-5-alter-table-drop-column-syntax)
        - [ex1) DROP COLUMN 사용하여 단일 필드 삭제](#ex1-drop-column-사용하여-단일-필드-삭제)
        - [ex2) DROP COLUMN 사용하여 다중 필드 삭제](#ex2-drop-column-사용하여-다중-필드-삭제)
4. [참고](#4-참고)
    1. [반드시 NOT NULL 제약을 사용해야 할까?](#4-1-반드시-not-null-제약을-사용해야-할까)

<br>
<br>

## 1. Create a table

### 1-1. CREATE TABLE syntax

```SQL
CREATE TABLE table_name (
    column_1 data_type,
    column_2 data_type,
    ...,
    constraints
);
```

-   테이블 생성
-   각 필드에 적용할 데이터 타입(data type) 작성
-   테이블 및 필드에 대한 제약조건(constraints) 작성

<br>

### - CREATE TABLE examples

```SQL
-- ex) 테이블 생성 예시

CREATE TABLE examples (
    examId INT AUTO_INCREMENT,
    lastName VARCHAR(50) NOT NULL,
    firstNAME VARCHAR(50) NOT NULL,
    PRIMARY KEY (examId)
);

------------------------------------------

-- 테이블 구조 확인

SHOW COLUMNS FROM examples;
```

-   데이터 타입
    -   ex) INT, VARCHAR(50)...
-   제약 조건
    -   ex) NOT NULL, PRIMARY KEY...
-   속성
    -   ex) AUTO_INCREMENT...

<br>

### 1-2. 대표적인 MySQL 데이터 타입

| 타입          | 의미   | 예시                |
| ------------- | ------ | ------------------- |
| Numeric       | 숫자형 | INT, FLOAT, ...     |
| String        | 문자형 | VARCHAR, TEXT, ...  |
| Date and Time | 날짜형 | DATE, DATETIME, ... |

<br>

-   문자형 VARCHAR 와 CHAR 비교

> -   `VARCHAR(50)` : 가변 길이 문자열을 의미, 50byte까지 넣을 수 있음(한글은 2byte를 차지하기에 25자리까지 가능)
>     -   "1234" 문자열을 넣을 경우, VARCHAR는 "1234" 4개의 문자가 입력

> -   `CHAR(50)` : 고정 길이 문자열을 의미, 50byte로 고정
>     -   "1234' 문자열을 넣을 경우, CHAR는 "1234**\*\***\_\_\_**\*\*** ..."로 남은 문자열을 공백으로 채움

<br>

### 1-3. Constraints(제약 조건)

-   데이터 `무결성(데이터의 정확성과 일관성을 보증)`을 지키기 위해 데이터를 입력받을 때, 실행하는 검사 규칙

<br>

### - 대표적인 MySQL Constraints

-   PRIMARY KEY

    -   해당 필드를 키본 키로 지정

-   NOT NULL
    -   해당 필드에 NULL 값을 저장하지 못하도록 지정

<br>

### 1-4. AUTO_INCREMENT attribute

-   테이블의 기본 키에 대한 번호 자동 생성

<br>

### - AUTO_INCREMENT 특징

-   기본 키 필드에 사용
    -   고유한 숫자를 부여
-   시작 값은 1이며, 데이터 입력 시 값을 생략하면 자등으로 1씩 증가
-   이미 사용한 값을 재사용하지 않음
-   기본적으로 NOT NULL 제약 조건을 포함

<br>
<br>

## 2. Delete a table

### 2-1. DROP TABLE syntax

```SQL
DROP TABLE table_name;
```

-   테이블 삭제
-   DROP TABLE statement 이후, 삭제할 테이블명 작성

<br>
<br>

## 3. Modifying table fields

### 3-1. ALTER TABLE

-   테이블 필드 조작(생성, 수정, 삭제)

| 문법                          | 의미           |
| ----------------------------- | -------------- |
| ALTER TABLE **ADD**           | 필드 추가      |
| ALTER TABLE **MODIFY**        | 필드 속성 변경 |
| ALTER TABLE **CHANGE COLUMN** | 필드 이름 변경 |
| ALTER TABLE **DROP COLUMN**   | 필드 삭제      |

<br>

### 3-2. ALTER TABLE ADD syntax

```SQL
-- ex) 필드 추가 예시

ALTER TABLE
    table_name
ADD
    new_column_name column_definition;
```

-   ADD 키워드 이후 추가하고자 하는 새 필드 이름과 데이터 타입 및 제약 조건 작성

<br>

### - 예시 기초 테이블 생성

```SQL
-- examples 테이블

CREATE TABLE examples (
    examId INT AUTO_INCREMENT,
    lastName VARCHAR(50) NOT NULL,
    firstName VARCHAR(50) NOT NULL,
  PRIMARY KEY (examId)
);
```

<br>

### ex1) ADD 사용하여 테이블에 단일 필드 추가

-   examples 테이블에 country 필드 추가(단, country 필드는 가변 길이 문자열 최대 100자이며, NULL 값을 허용하지 않음)

정답)

|   Field   |     Type     | Null | Key | Default |     Extra      |
| :-------: | :----------: | :--: | :-: | :-----: | :------------: |
|  examId   |     int      |  NO  | PRI |  NULL   | auto_increment |
| lastName  | varchar(50)  |  NO  |     |  NULL   |                |
| firstName | varchar(50)  |  NO  |     |  NULL   |                |
|  country  | varchar(100) |  NO  |     |  NULL   |                |

```SQL
ALTER TABLE
    examples
ADD
    country VARCHAR(100) NOT NULL;
```

<br>

### ex2) ADD 사용하여 테이블에 다중 필드 추가

-   examples 테이블에 age와 address 필드 추가
    -   단, age 필드는 정수 타입, NULL 값을 허용하지 않음
    -   address 필드는 가변 길이 문자열 최대 100자이며, NULL 값을 허용하지 않음

정답)

|   Field   |     Type     | Null | Key | Default |     Extra      |
| :-------: | :----------: | :--: | :-: | :-----: | :------------: |
|  examId   |     int      |  NO  | PRI |  NULL   | auto_increment |
| lastName  | varchar(50)  |  NO  |     |  NULL   |                |
| firstName | varchar(50)  |  NO  |     |  NULL   |                |
|  country  | varchar(100) |  NO  |     |  NULL   |                |
|    age    |     int      |  NO  |     |  NULL   |                |
|  address  | varchar(100) |  NO  |     |  NULL   |                |

```SQL
ALTER TABLE examples
ADD age INT NOT NULL,
ADD address VARCHAR(100) NOT NULL;
```

<br>

### 3-3. ALTER TABLE MODIFY syntax

```SQL
ALTER TABLE
    table_name
MODIFY
    column_name column_definition;
```

-   MODIRY 키워드 이후 변경하고자 하는 필드 이름 그리고 데이터 타입 및 제약 조건 작성

<br>

### ex1) MODIFY 사용하여 테이블의 단일 필드 수정

-   examples 테이블의 address 필드를 가변 길이 문자열 최대 50자까지 그리고 NULL 값을 허용하지 않도록 변경

정답)

|   Field   |     Type     | Null | Key | Default |     Extra      |
| :-------: | :----------: | :--: | :-: | :-----: | :------------: |
|  examId   |     int      |  NO  | PRI |  NULL   | auto_increment |
| lastName  | varchar(50)  |  NO  |     |  NULL   |                |
| firstName | varchar(50)  |  NO  |     |  NULL   |                |
|  country  | varchar(100) |  NO  |     |  NULL   |                |
|    age    |     int      |  NO  |     |  NULL   |                |
|  address  | varchar(50)  |  NO  |     |  NULL   |                |

```SQL
ALTER TABLE
    examples
MODIFY
    address VARCHAR(50) NOT NULL;
```

<br>

### ex2) MODIFY 사용하여 테이블의 다중 필드 수정

-   examples 테이블의 lastName, firstName 필드를 가변 길이 문자열 최대 10자까지 그리고 NULL 값을 허용하지 않도록 변경

정답)

|   Field   |     Type     | Null | Key | Default |     Extra      |
| :-------: | :----------: | :--: | :-: | :-----: | :------------: |
|  examId   |     int      |  NO  | PRI |  NULL   | auto_increment |
| lastName  | varchar(10)  |  NO  |     |  NULL   |                |
| firstName | varchar(10)  |  NO  |     |  NULL   |                |
|  country  | varchar(100) |  NO  |     |  NULL   |                |
|    age    |     int      |  NO  |     |  NULL   |                |
|  address  | varchar(50)  |  NO  |     |  NULL   |                |

```SQL
ALTER TABLE examples
MODIFY lastName VARCHAR(10) NOT NULL,
MODIFY firstName VARCHAR(10) NOT NULL;
```

<br>

### 3-4. ALTER TABLE CHANGE COLUMN syntax

```SQL
ALTER TABLE
    table_name
CHANGE COLUMN
    original_name new_name column_definition;
```

-   CHANGE COLUMN 키워드 이후, 기존 필드 이름, 변경하고자 하는 필드 이름, 그리고 데이터 타입 및 제약조건 작성

<br>

### ex1) CHANGE COLUMN 사용하여 필드 변경

-   examples 테이블의 country 필드의 이름을 state로 변경
    -   데이터 타입 및 제약 조건은 기존과 동일

정답)

|   Field   |     Type     | Null | Key | Default |     Extra      |
| :-------: | :----------: | :--: | :-: | :-----: | :------------: |
|  examId   |     int      |  NO  | PRI |  NULL   | auto_increment |
| lastName  | varchar(10)  |  NO  |     |  NULL   |                |
| firstName | varchar(10)  |  NO  |     |  NULL   |                |
|   state   | varchar(100) |  NO  |     |  NULL   |                |
|    age    |     int      |  NO  |     |  NULL   |                |
|  address  | varchar(50)  |  NO  |     |  NULL   |                |

```SQL
ALTER TABLE
    examples
CHANGE COLUMN
    country state VARCHAR(100) NOT NULL;
```

<br>

### 3-5. ALTER TABLE DROP COLUMN syntax

```SQL
ALTER TABLE
    table_name
DROP COLUMN
    column_name;
```

-   DROP COLUMN 키워드 이후, 삭제할 필드 이름 작성

<br>

### ex1) DROP COLUMN 사용하여 단일 필드 삭제

-   examples 테이블의 address 필드 삭제

정답)

|   Field   |     Type     | Null | Key | Default |     Extra      |
| :-------: | :----------: | :--: | :-: | :-----: | :------------: |
|  examId   |     int      |  NO  | PRI |  NULL   | auto_increment |
| lastName  | varchar(10)  |  NO  |     |  NULL   |                |
| firstName | varchar(10)  |  NO  |     |  NULL   |                |
|   state   | varchar(100) |  NO  |     |  NULL   |                |
|    age    |     int      |  NO  |     |  NULL   |                |

```SQL
ALTER TABLE
    examples
DROP COLUMN
    address;
```

<br>

### ex2) DROP COLUMN 사용하여 다중 필드 삭제

-   examples 테이블의 state와 age 필드 삭제

정답)

|   Field   |    Type     | Null | Key | Default |     Extra      |
| :-------: | :---------: | :--: | :-: | :-----: | :------------: |
|  examId   |     int     |  NO  | PRI |  NULL   | auto_increment |
| lastName  | varchar(10) |  NO  |     |  NULL   |                |
| firstName | varchar(10) |  NO  |     |  NULL   |                |

```SQL
ALTER TABLE examples
DROP COLUMN state,
DROP COLUMN age;
```

<br>
<br>

## 4. 참고

### 4-1. 반드시 NOT NULL 제약을 사용해야 할까?

-   `그렇지 않음`
-   데이터 베이스를 사용하는 프로그램에 따라서 NULL을 저장할 필요가 없는 경우가 많기에 `가능한한 NOT NULL로 정의`함
-   `'값이 없음'`의 표현을 테이블에 기록하는 것은 `0`이나 `빈문자열` 등을 사용하는 것으로 대체하는 것을 권장
