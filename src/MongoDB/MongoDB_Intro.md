# MongoDB 소개

## 목차

1. [데이터 베이스](#1-데이터-베이스)
    1. [데이터 베이스 장점](#1-1-데이터-베이스-장점)
    2. [데이터 베이스 단점](#1-2-데이터-베이스-단점)
    3. [데이터 베이스의 필요성](#1-3-데이터-베이스의-필요성)
    4. [DBMS](#1-4-dbms)
2. [MongoDB](#2-mongodb)
    1. [MongoDB란?](#1-1-mongodb란)
    2. [기존 데이터 베이스 솔루션과의 차이](#1-2-기존-데이터-베이스-솔루션과의-차이)
    3. [NoSQL 개념](#2-3-nosql-개념)
        - [NoSQL 등장배경](#--nosql-등장배경)
    4. [SQL과 NoSQL 특징 비교 정리](#2-4-sql과-nosql-특징-비교-정리)
        - [ACID vs BASE](#--acid-vs-base)
3. [MongoDB Ecosystem](#3-mongodb-ecosystem)

<br/>
<br/>

## 1. 데이터 베이스

- 데이터 베이스는 여러 사람이 공유하여 사용할 목적으로 체계화하여 통합, 관리되는 데이터 집합

<br/>

### 1-1. 데이터 베이스 장점

- 데이터 중복 최소화
- 데이터 공유
- 일관성, 무결성, 보안성 유지
- 최신 데이터 유지
- 데이터의 표준화 가능
- 데이터의 논리적, 물리적 독립성
- 용이한 데이터 접근
- 데이터 저장 공간 절약

<br/>

### 1-2. 데이터 베이스 단점

- 전문가 필요
- 많은 비용 부담
- 데이터 백업 및 복구의 어려움
- 시스템이 복잡함
- 액세스가 집중되면 과부하 발생

<br/>

### 1-3. 데이터 베이스의 필요성

- 유저가 생성되거나 POST 요청을 통해 데이터 수정 시, 그 순간에는 데이터가 생성 및 추가되지만, `서버를 재시작하면 해당 데이터가 없어짐`
- 따라서 데이터 베이스를 통한 `영구적인 보관`이 필요해짐

<br/>

### 1-4. DBMS

- 계층형, 네트워크형, 관계형, 객체형 등이 있음
- 그 중 관계형(RDBMS, Relational DBMS)를 주로 사용하고 있음
- 대표적인 RDBMS는 Oracle, MySQL, PostgreSQL 등이 있음

![RDBMS](../assets/img/MongoDB_RDBMS.png)

<RDBMS 모습>

<br>
<br>

## 2. MongoDB

### 2-1. MongoDB란?

- MongoDB : 데이터 베이스 솔루션(DBMS)
- `많은 양`의 데이터를 효율적으로 저장할 수 있음 (Humongous)

<br>

### 2-2. 기존 데이터 베이스 솔루션과의 차이

- 데이터 베이스 솔루션은 `mySQL`, `postgreSQL`과 같이 수많은 방법이 있음
- 데이터 베이스 서버로 서로 다른 데이터 베이스를 실행할 수 있게 해줌
- Documents는 JavaScript의 객체(Object)와 유사하게 생김, 정확히는 `JSON 형식`을 사용
- SQL 기반 데이터 베이스는 스키마(데이터 요구사항)를 통해 저장할 데이터를 엄격하게 관리하지만, MongoDB는 스키마를 강제하지 않아서 `유연함` (`Schemaless`)
- 전혀 다른 데이터를 하나의 동일한 Collection에 저장할 수 있음
- 즉, 서버를 복잡하게 재구성하지 않고도 필요한 형식으로 데이터를 쿼리할 수 있음
- 반면, 지저분한 데이터가 될 수 있기에 주의해야 함

<br/>

```
구조

Database(ex. Shop) --> Collections(ex. Users, Menus, Orders) --> Documents(ex. {name:'Jeonggon'})
```

<br/>

![MongoDB 구조와 Schemaless](../assets/img/MongoDB_structure_schemaless.png)

<MongoDB 구조와 Schemaless>

<br/>

```mongodb-json
// JSON(BSON) 데이터 형식
// 키(key)-값(value)

{
    "name": "Jeonggon",
    "age": 27,
    "address":
        {
            "Nation": "Korea"
        },
    "hobbies": [
        { "name": "Cooking" },
        { "name": "Sports" }
    ]
}
```

- 값으로 `숫자`, `문자`, `참-거짓`, `중첩 데이터`, `배열` 등을 가질 수 있음
  - 중첩 데이터를 통해 데이터 사이의 `복잡한 관계`를 `하나의 동일한 문서`에 저장할 수 있음
  - SQL에서는 A테이블과 B테이블의 데이터를 찾기 위해서는 복합적인 JOIN을 작성해야하는데 MongoDB는 문서 하나에 가져올 수 있어서 효율적임
  - 데이터를 논리적으로 저장할 수 있음
- 데이터 형식을 `BSON`이라고도 하는데 이는 MongoDB가 JSON 데이터를 `바이너리 버전`으로 변환하기 때문

<br>

### 2-3. NoSQL 개념

- `Not only SQL`의 의미로 SQL만을 사용하지 않는(여러 유형의 DB 사용) 데이터 베이스 관리 시스템을 지칭
- NoSQL이 No RDBMS를 의미하지는 않음 (구체적인 정의가 없음)
- 단순히 `SQL` 기반 데이터 베이스와 `정반대`의 개념, 철학을 따르기 때문
- MongoDB는 `NoSQL` 솔루션임
- MongoDB와 CouchDB 간에도 사용하는 쿼리 언어가 다르지만 SQL이 아니기에 NoSQL로 묶임


| 분류      | 특징                                                                                 |
|---------|------------------------------------------------------------------------------------|
| SQL     | 데이터를 `정규화(normalization)`해서 저장하고 모든 테이블이 `스키마`를 가지고 있으며 여러 테이블에 데이터를 배포하고 `관계를 사용` |
| MongoDB | 데이터를 문서(Document)로 함께 저장하며 스키마를 강제하지 않음                                            |

- `관계(relations)`도 존재
- MongoDB는 컬랙션(테이블)이 적음
  - 대신 데이터를 함께 저장
  - 응용 프로그램이 데이터를 가져올 경우, 컬랙션1과 컬랙션2를 `병합할 필요가 없음`

<br/>

| SQL Database | NoSQL Database  |
|--------------|-----------------|
| MySQL        | MongoDB         |
| Oracle       | Redis           |
| PostgreSQL   | CouchDB         |
| MS-SQL       | Amazon DynamoDB |
| ...          | Apache HBase    |
| ...          | Cassandra       |
| ...          | Elasticsearch   |
| ...          | ...             |

<SQL과 NoSQL 데이터 베이스 분류>

<br/>

### - NoSQL 등장배경

1. 빅데이터의 등장으로 너무 많은 데이터를 처리해야 함 --> 데이터 처리 비용 증가
2. 다양한 형태의 데이터를 저장해야 함
3. 애자일 기법의 도입으로 변화되는 요구에 빠른 대응이 필요함

<br/>

### 2-4. SQL과 NoSQL 특징 비교 정리

| 비교           | SQL                                              | NoSQL                                                |
|:-------------|:-------------------------------------------------|:-----------------------------------------------------|
| 쿼리 언어        | Structured query language                        | No query language                                    |
| 데이터 베이스 타입   | 테이블                                              | Key-value, document, wide-column, graph              |
| 스키마          | Predefined                                       | Dynamic                                              |
| 확장성          | Vertical<br/>사용하던 장비를 더 좋은 장비로 교체<br/>비용이 많이 들어감 | Horizontal<br/>사용하던 것에 노드 또는 시스템을 더 해줌<br/>분산 저장을 지원 |
| ACID vs BASE | ACID                                             | BASE                                                 |

<br/>

### - ACID vs BASE

- ACID : 데이터가 일관되고 안정적임
  - `Atomic` : 원자성으로 모든 작업이 성공하거나 롤백됨
  - `Consistency` : 일관성으로 각 트랜잭션은 DB가 유효한 상태에서 다른 상태로 이동
  - `Isolation` : 격리로 트랜잭션 간에 간섭 없음
  - `Durability` : 지속성으로 트랜잭션 결과는 실패가 있더라도 영구적임

<br/>

- BASE : 복제된 데이터의 일관성보다 유동적이며, 가용성을 선호
  - `Basically Available` : 모든 사용자가 쿼리 수행 가능, 오류 발생 시, 완전히 중단되지 않음
  - `Soft State` : 데이터 베이스 상태는 시간이 지남에 따라 변경될 수 있음
  - `Eventually Consistent` : 작동하고 충분히 지나면 데이터 베이스가 일관성을 갖게 됨

<br>
<br>

## 3. MongoDB Ecosystem

![MongoDB ecosystem](../assets/img/MongoDB_ecosystem.png)

<MongoDB에서 제공하는 서비스>

<br>

1. `MongoDB 데이터 베이스` : 핵심 기능인 동시에 핵심 주제
2. `Self-Managed / Enterprise` : 개인용 / 기업용 솔루션
   - `CloudManager / OpsManager` : 데이터 베이스 관리 도구

3. `Atlas(Cloud)` : 클라우드 솔루션
4. `Mobile` : 모바일 솔루션으로 모바일에 MongoDB를 직접 설치해 데이터를 저장하고 인터넷 없이 작업가능
5. `Compass` : 그래픽 사용자 인터페이스(GUI)인 컴패스를 통해 데이터 베이스에 연결해 사용자 인터페이스로 데이터를 볼 수 있음
6. `BI Connectors`, `MongoDB Charts` : 데이터 사이언스를 위해 다양한 분석 툴을 연결해주는 도구
7. `Stitch`
    - 벡엔드 솔루션으로 서버 없는 쿼리 API를 제공
    - 클라이언트의 앱(ex.리액트) 안에서 데이터 베이스를 효율적으로 쿼리할 수 있게 해줌
    - 서버 없는 함수로 클라우드에서 명령형으로 코드 실행 가능
    - 데이터 베이스 트리거로 데이터 베이스에서 이벤트를 발생할 수 있게 해줌(ex.이메일 보내기)
    - 실시간 동기화(ex.클라우드 내 데이터 베이스-모바일 데이터 베이스 동기화)
