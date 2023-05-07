# 단일 테이블 쿼리

## - 목차
1. [Querying data](#1-querying-data)
    - [SELECT](#1-select)
        - [SELECT syntax](#--select-syntax)
        - [ex1) 특정 단일 필드의 모든 데이터 조회](#--ex1-특정-단일-필드의-모든-데이터-조회)
        - [ex2) 다수 필드의 모든 데이터 조회](#--ex2-다수-필드의-모든-데이터-조회)
        - [ex3) 모든 필드의 모든 데이터 조회](#--ex3-모든-필드의-모든-데이터-조회)
        - [ex4) 필드에 별칭을 사용하여 데이터 조회](#--ex4-필드에-별칭-사용하여-데이터-조회)
        - [ex5) 연산을 하여 데이터 조회](#--ex5-연산을-하여-데이터-조회)
        - [정리](#--정리)
2. [Sorting data](#2-sorting-data)
    - [ORDER BY](#1-order-by)
        - [ORDER BY syntax](#--order-by-syntax)
        - [ex1) 기본 정렬](#--ex1-기본-정렬)
        - [ex2) 내림차순으로 정렬](#--ex2-내림차순으로-정렬)
        - [ex3) 이중 정렬](#--ex3-이중-정렬)
        - [ex4) 연산과 정렬](#--ex4-연산과-정렬)
        - [SELECT Statements 실행순서](#--select-statements-실행순서)
3. [Filtering data](#3-filtering-data)
4. [Grouping data](#4-grouping-data)

---

## (1) Querying data

### **1) SELECT**


: 테이블에서 데이터를 `조회`

### - SELECT syntax

```sql
SELECT select_list FROM table_name;
```

- `SELECT` 키워드 다음에 데이터를 선택하려는 `필드`를 하나 이상 지정
- `FROM` 키워드 다음에 데이터를 선택하려는 `테이블`의 이름을 지정


### - ex1) 특정 단일 필드의 모든 데이터 조회

- 다음의 테이블 employees에서 lastName 필드의 모든 데이터 조회

정답)

| lastName  |
|:---------:|
|  Murphy   |
| Patterson |
| Firrelli  |
|    ...    |
|   Nishi   |
|   Kato    |
|  Gerard   |

```sql
SELECT
    lastName
FROM
    employees;
```


### - ex2) 다수 필드의 모든 데이터 조회

- 다음의 테이블 employees에서 lastName, firstName 필드의 모든 데이터 조회

정답)

| lastName  | firstName |
|:---------:|:---------:|
|  Murphy   |   Diane   |
| Patterson |   Mary    |
| Firrelli  |   Jeff    |
|    ...    |    ...    |
|   Nishi   |   Mami    |
|   Kato    |  Yoshimi  |
|  Gerard   |  Martin   |

```sql
SELECT
    lastName, firstName
FROM
    employees;
```


### - ex3) 모든 필드의 모든 데이터 조회

- 다음 테이블 employees에서 모든 필드의 데이터 조회

정답)

| employNumber | lastName  | firstName | extension |       email        | officeCode | reportsTo |   jobTitle   |
|:------------:|:---------:|:---------:|:---------:|:------------------:|:----------:|:---------:|:------------:|
|     1002     |  Murphy   |   Diane   |   x5800   |  aa123@naver.com   |     1      |   null    |  President   |
|     1056     | Patterson |   Mary    |   x4611   |  aa456@naver.com   |     1      |   1002    |   VP Sales   |
|     1076     | Firrelli  |   Jeff    |   x9273   |  aa789@naver.com   |     1      |   1002    | VP Marketing |
|     ...      |    ...    |    ...    |    ...    |        ...         |    ...     |    ...    |     ...      |
|     1625     |   Kato    |  Yoshimi  |   x102    | aa101112@naver.com |     5      |   1621    |  Sales Rep   |
|     1702     |  Gerard   |  Martin   |   x2312   | aa131415@naver.com |     4      |   1102    |  Sales Rep   |

```sql
SELECT
    *
FROM
    employees;
```


### - ex4) 필드에 별칭 사용하여 데이터 조회

- 다음 테이블 employees에서 firstName 필드의 모든 데이터 조회
  - 단, 조회 시 firstName이 아닌 '이름'으로 출력 될 수 있도록 출력명 변경)

정답)

|   이름   |
|:------:|
|  Diane |
|  Mary  |
|  Jeff  |
|  ...   |
|  Mami  |
| Yoshimi |
| Martin |

```sql
SELECT
    firstName AS '이름'
FROM
    employees;

-- AS 를 사용하여 새로운 별칭을 지정할 수 있다.
```


### - ex5) 연산을 하여 데이터 조회

- 다음 테이블 orderdetails에서 productCode, 주문 총액 필드의 모든 데이터를 조회
  - 단, 주문 총액 필드는 quantityOrdered와 priceEach 필드를 곱한 결과값

정답)

| productCode |  주문 총액  |
|:-----------:|:-------:|
|  S18_1749   |  4080   |
|  S18_2248   | 2754.5  |
|  S18_4409   | 1660.12 |
|  S24_3969   | 1729.21 |
|     ...     |   ...   |
|  S18_1129   | 3898.1  |
|  S18_1889   | 2681.91 |
|  S18_1984   | 3527.8  |

```sql
SELECT
    ProductCode,
    quantityOrdered * priceEach AS '주문 총액'
FROM
    orderdetails;

-- Arithmetic Operators : 기본적인 사칙연산 사용 가능
```


### - 정리

- SELECT문을 사용하여 테이블의 데이터 조회 및 반환
- SELECT * (asterisk)를 사용하여 테이블의 모든 필드 선택


---

## (2) Sorting data

### **1) ORDER BY**


: 조회 결과의 레코드를 정렬


### - ORDER BY syntax

```sql
SELECT
    select_list
FROM
    table_name
ORDER BY
    column1 [ASC|DESC],
    column2 [ASC]DESC],
    ...;
```

- FROM clause 뒤에 위치
- 하나 이상의 컬럼을 기준으로 결과를 오름차순, 내림차순으로 정렬할 수 있음
  - ASC : 오름차순(기본값)
  - DESC : 내림차순


### - ex1) 기본 정렬

- 테이블 employees에서 firstName 필드의 모든 데이터를 오름차순으로 조회

정답)

| firstName |
|:---------:|
|   Andy    |
|  Anthony  |
|   Barry   |
|    ...    |
|    Tom    |
|  William  |
|  Yoshimi  |

```sql
SELECT
    firstName
FROM
    employees
ORDER BY
    firstName;
```


### - ex2) 내림차순으로 정렬

- 테이블 employees에서 firstName 필드의 모든 데이터를 내림차순으로 조회

정답)

| firstName |
|:--------:|
|  Yoshimi |
|  William |
|    Tom   |
|    ...   |
|  Barry   |
|  Anthony |
|   Andy   |

```sql
SELECT
    firstName
FROM
    employees
ORDER BY
    firstName DESC;
```


### - ex3) 이중 정렬

- 테이블 employees에서 lastName 필드를 기준으로 내림차순 정렬한 후, firstName 필드 기준으로 오름차순 정렬하여 조회

정답)

| lastName  | firstName |
|:---------:|:---------:|
|  Vanauf   |  George   |
|   Tseng   | Foon Yue  |
| Thompson  |  Leslie   |
| Patterson |   Mary    |
| Patterson |   Steve   |
| Patterson |  William  |
|    ...    |    ...    |

```sql
SELECT
    lastName, firstName
FROM
    employees
ORDER BY
    lastName DESC,
    firstName;
```


### - ex4) 연산과 정렬

- 테이블 orderdetails에서 totalSales 필드를 기준으로 내림차순으로 정렬한 다음 productCode, totalSales 필드의 모든 데이터를 조회
  - 단, totalSales 필드는 quantityOrdered와 priceEach 필드를 곱한 결과값

정답)

| productCode | totalSales |
|:-----------:|:----------:|
|  S10_4698   |  11503.14  |
|  S12_4675   |  11170.52  |
|  S18_1749   |  10723.6   |
|  S12_1099   |  10460.16  |
|  S10_1949   |  10286.4   |
|  S10_1949   |   10072    |
|     ...     |    ...     |

```sql
SELECT
    productCode,
    quantityOrdered * priceEach AS totalSales
FROM
    orderdetails
ORDER BY 
    totalSales DESC;
```


### - SELECT Statements 실행순서

1. 테이블에서 (FROM)
2. 조회하여 (SELECT)
3. 정렬 (ORDER BY)


---

## (3) Filtering data

### **1) Filtering data 관련 키워드**

- Clause
  - DISTINCT
  - WHERE
  - LIMIT

- Operator
  - BETWEEN
  - IN
  - LIKE
  - Comparison
  - Logical


---

## (4) Grouping data

