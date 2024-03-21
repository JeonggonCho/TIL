# SQL 기초

## 목차

1. [SQL 소개](#1-sql-소개)
    1. [SQL(Structured Query Language)](#1-1-sqlstructured-query-language)
    2. [SQL Syntax](#1-2-sql-syntax)
2. [SQL Statements](#2-sql-statements)
    1. [SQL Statements 예시](#2-1-sql-statements-예시)
    2. [SQL Statements 유형](#2-2-sql-statements-유형)
    3. [정리](#2-3-정리)
3. [용어정리](#3-용어정리)
    1. [Query](#3-1-query)
    2. [SQL 표준](#3-2-sql-표준)

<br>
<br>

## 1. SQL 소개

### 1-1. SQL(Structured Query Language)

-   데이터 베이스에 정보를 `저장`하고 `처리`하기 위한 `프로그래밍 언어`
-   테이블 형태로 `구조화된` `관계형` 데이터 베이스에 `요청`을 함

```
- 프로그래밍 언어 : 컴퓨터와의 대화
- SQL : 관계형 데이터 베이스와의 대화
```

```sql
-- ex)

-- 구글에 작성하는 질문

"how old is earth?"


-- SQL에서 다음과 같이 작성

SELECT age FROM solar_system WHERE name = 'earth';

-- 해석: solar_system 테이블에서 name이 'earth'인 항목을 찾아서 그 항목의 age를 조회
```

<br>

### 1-2. SQL Syntax

```sql
SELECT column_name FROM table_name;
```

-   SQL 키워드는 대소문자를 구분하지 않음
    -   하지만 대문자로 작성하는 것을 권장(명시적 구분)
-   각 SQL Statements의 끝에는 세미콜론(;)이 필요
    -   세미콜론은 각 SQL Statements를 구분하는 방법

<br>
<br>

## 2. SQL Statements

-   SQL 언어를 구성하는 가장 기본적인 코드블록

<br>

### 2-1. SQL Statements 예시

```sql
SELECT column_name FROM table_name;
```

-   해당 코드는 `SELECT Statement`라고 함
-   이 구문은 SELECT, FROM 2개의 키워드로 구성

<br>

### 2-2. SQL Statements 유형

-   데이터 베이스에서 `수행목적`에 따라 `4가지` 범주로 나뉨

```
- DDL : 데이터 정의
- DQL : 데이터 검색
- DML : 데이터 조작
- DCL : 데이터 제어
```

<br>

|                 유형                 |                    역할                    |                SQL 키워드                |
| :----------------------------------: | :----------------------------------------: | :--------------------------------------: |
|  DDL<br/>(Data Definition Language)  |      데이터의 기본 구조 및 형식 변경       |        CREATE<br/>DROP<br/>ALTER         |
|    DQL<br/>(Data Query Language)     |                데이터 검색                 |                  SELECT                  |
| DML<br/>(Data Manipulation Language) |     데이터 조작<br/>(추가, 수정, 삭제)     |       INSERT<br/>UPDATE<br/>DELETE       |
|   DCL<br/>(Data Control Language)    | 데이터 및 작업에 대한<br/>사용자 권한 제어 | COMMIT<br/>ROLLBACK<br/>GRANT<br/>REVOKE |

<br>

### 2-3. 정리

-   SQL은 데이터 베이스와 상호작용하고 데이터 베이스에서 데이터를 반환하기 위한 언어
-   단순히 SQL문법을 암기하고 상황에 따라 실행만 하는 것이 아닌 SQL을 통해 관계형 데이터 베이스를 잘 이해하고 다루는 방법이 중요

<br>
<br>

## 3. 용어정리

### 3-1. Query

-   질의, 질문
-   데이터 베이스로부터 정보를 요청하는 것
-   일반적으로 SQL로 작성하는 코드를 쿼리문(SQL문)이라 함

<br>

### 3-2. SQL 표준

-   SQL은 미국 국립 표준 협회(ANSI)와 국제 표준화 기구(ISO)에 의해 표준이 채택
-   널리 사용되는 모든 RDBMS에서 SQL 표준을 지원
-   RDBMS별로 독자적인 기능에 따라서 표준을 벗어나는 문법이 존재함
