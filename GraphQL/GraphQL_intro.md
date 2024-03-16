# GraphQL 소개

## 목차

1. [GraphQL이란?](#1-graphql이란)
    1. [GraphQL 요청과 REST 요청 비교](#1-1-graphql-요청과-rest-요청-비교)
    2. [GraphQL 처리 과정](#1-2-graphql-처리-과정)
2. [GraphQL 사용 이유](#2-graphql-사용-이유)
    1. [왜 REST가 있는데 GraphQL을 사용하는가?](#2-1-왜-rest가-있는데-graphql을-사용하는가)
    2. [GraphQL 장점](#2-2-graphql-장점)
        - [병렬 개발 작업 가능](#--병렬-개발-작업-가능)
        - [Overfetching과 Underfetching을 막아줌](#--overfetching과-underfetching을-막아줌)

<br/>
<br/>

## 1. GraphQL이란?

- 2012년 페이스북에 의해 개발된 API를 위한 쿼리 언어
- API의 데이터에 대한 이해하기 쉬운 설명을 제공하고 `클라이언트가 필요한 것을 "정확하게" 요청할 수 있는 능력`을 제공함

<br/>

### 1-1. GraphQL 요청과 REST 요청 비교

| GraphQL                                          | REST                                   |
|--------------------------------------------------|----------------------------------------|
| 요청을 보낼 때, 쿼리 언어를 이용해서 보내며 어떠한 것을 받을 것인지 예상할 수 있음 | 어떤 요청인지에 대한 메서드와 엔드포인트는 어떻게 되는지 요청을 보냄 |

```graphql
# GraphQL 요청

{
    assets(<album_id>) {
    id,
    url,
    comments {
        text
    }
}
}
```

```
REST 요청

GET /albums/:album_id/assets
```

<br/>

### 1-2. GraphQL 처리 과정

1. 데이터 묘사

```typescript
type Project
{
    name: String
    tagline: String
    contributors: [User]
}
```

<br/>

2. 필요한 데이터 선별적으로 클라이언트에 요청

```graphql
{
    project(name: "GraphQL") {
        tagline
    }
}
```

<br/>

3. 서버에서 예측한 데이터 받기

```json
{
  "project": {
    "tagline": "A query language for APIs"
  }
}
```

<br/>

## 2. GraphQL 사용 이유

### 2-1. 왜 REST가 있는데 GraphQL을 사용하는가?

- GraphQL은 REST에서 부족한 점을 `보완`하기 위해 탄생함
- GraphQL이 REST에 비해서 우수하다가 아닌 서로의 특징과 장단점이 있기에 `상황에 알맞은 것을 사용`하는 것이 중요함

<br/>

### 2-2. GraphQL 장점

### - 병렬 개발 작업 가능

- 주로 개발 과정을 살펴보면 백엔드 개발자가 REST API를 개발하면 프론트엔드 개발자가 해당 API를 호출해서 받아온 데이터를 화면에 보여줌
- 하지만, GraphQL을 이용하면 프론트엔드 개발과 백엔드 개발 `전체 프로세스를 병렬로 작업할 수 있음`

<br/>

### - Overfetching과 Underfetching을 막아줌

- 사용자의 특정 데이터만 필요한 상황에서 REST API의 엔드포인트가 사용자의 `모든 정보를 반환하면 불필요한 데이터까지 포함되어 과다요청이 발생함`
- 이는 대역폭을 낭비하고 성능저하를 야기 할 수 있음
- 사용자의 이름과 사용자가 작성한 포스트를 요청해야할 때, REST API의 엔드포인트가 사용자 정보와 포스트 정보를 `따로 제공`한다면, `2번의 요청을 보내야함`
- 이는 추가적인 네트워크 비용과 지연을 초래할 수 있음