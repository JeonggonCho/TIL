# Apollo Server

## 목차

1. [Apollo Server V3](#1-apollo-server-v3)
    1. [불필요한 패키지 삭제](#1-1-불필요한-패키지-삭제)
    2. [Apollo Server를 위한 패키지 설치](#1-2-apollo-server를-위한-패키지-설치)
    3. [server.js 소스코드 수정](#1-3-serverjs-소스코드-수정)
    4. [Query your server - Apollo Sandbox-tools](#1-4-query-your-server---apollo-sandbox-tools)
2. [Apollo Server V4](#2-apollo-server-v4)
    1. [불필요한 패키지 삭제](#2-1-불필요한-패키지-삭제)
    2. [Apollo Server V4를 위한 패키지 설치](#2-2-apollo-server-v4를-위한-패키지-설치)
    3. [server.js 소스코드 수정](#2-3-serverjs-소스코드-수정)

<br/>
<br/>

## 1. Apollo Server V3

- 기존 Express.js로 만든 웹 서버를 Apollo Server V3로 변환하기

<br/>

### 1-1. 불필요한 패키지 삭제

- express-graphql 패키지는 불필요함

```bash
$ npm uninstall express-graphql
```

```js
// 기존 server.js

const express = require("express");
// 해당 패키지 삭제했기에 import 삭제
// const { graphqlHTTP } = require("express-graphql");
const {makeExecutableSchema} = require("@graphql-tools/schema");
const {loadFilesSync} = require("@graphql-tools/load-files");
const path = require("path");

const PORT = 4000;
const app = express();

const loadedTypes = loadFilesSync("**/*", {
  extensions: ["graphql"],
});

const loadedResolvers = loadFilesSync(path.join(__dirname, "**/*.resolvers.js"));

const schema = makeExecutableSchema({
  typeDefs: loadedTypes,
  resolvers: loadedResolvers,
});

// 서버 코드 삭제
// app.use(
//   "/graphql",
//   graphqlHTTP({
//     schema: schema,
//     graphiql: true,
//   })
// );

app.listen(PORT, () => {
  console.log(`Running a GraphQL API server at http://localhost:${PORT}/graphql`);
});

```

<br/>

### 1-2. Apollo Server를 위한 패키지 설치

- Apollo Server를 위한 패키지는 다양하며 각 프로젝트 및 환경에 맞는 Apollo Server 패키지를 이용하면 됨
- [Apollo 공식 사이트 - 서버 패키지](https://www.apollographql.com/docs/apollo-server/v3/integrations/middleware/#all-supported-packages)

```bash
$ npm install apollo-server-express
```

<br/>

### 1-3. server.js 소스코드 수정

- apollo-server-express의 ApolloServer 사용

```js
// server.js

const express = require('express');
const path = require('path');
const {makeExecutableSchema} = require('@graphql-tools/schema');
const {loadFilesSync} = require('@graphql-tools/load-files');
// ApolloServer 가져오기
const {ApolloServer} = require('apollo-server-express');
const loadedTypes = loadFilesSync('**/*', {extensions: ['graphql']});
const loadedResolvers = loadFilesSync(path.join(__dirname, '**/*.resolvers.js'));

// Apollo 서버를 위한 함수 생성
async function startApolloServer() {
  const app = express();

  // 스키마 작성 (타입, 리졸버 연결)
  const schema = makeExecutableSchema({
    typeDefs: loadedTypes,
    resolvers: loadedResolvers,
  });

  // 아폴로 서버에 스키마 넣기 (기존에는 graphqlHTTP에 넣었었음)
  // 아폴로 서버 객체는 모든 미들웨어와 Graphql 요청을 다루는 로직들을 담고 있음
  const server = new ApolloServer({
    schema
  });

  // 앞서 만든 서버를 시작하기 (await으로 동기적으로 실행)
  await server.start();

  // 실행된 아폴로 미들웨어를 app express 서버와 연결하기
  // incoming request를 "/graphql" 경로에서 처리
  server.applyMiddleware({app, path: '/graphql'});

  // app을 4000번 포트를 통해 listen하도록 하기
  app.listen(4000, () => {
    console.log(`Running a GraphQL API server...`);
  });
}

// Apollo 서버 호출
startApolloServer();
```

<br/>

<p align="center">
    <img src="../assets/img/Apollo_server.png" width="700" alt="Apollo_server"><br/>
    <span>localhost:4000/graphql로 접속</span>
</p>

<br/>

### 1-4. Query your server - Apollo Sandbox-tools

- localhost:4000/graphql로 접속한 페이지 중앙의 `Query your server` 클릭
- `GraphiQL`과 유사한 페이지를 볼 수 있음
- 일종의 Apollo를 위한 GraphiQL로서 sandbox를 이용하며, Postman처럼 많은 기능을 가지고 있음
- Field, Type, Query, Response, status, 응답시간, 응답 데이터 용량 등 많은 것을 확인할 수 있음

<br/>

<p align="center">
    <img src="../assets/img/Apollo_query_your_server.png" width="700" alt="Apollo_query_your_server"><br/>
    <span>Query your server로 접속</span>
</p>

<br/>
<br/>

## 2. Apollo Server V4

- Apollo Server V3에서 `V4로 마이그레이션 하기`
- [Apollo 공식 사이트 - Migrating to Apollo Server 4](https://www.apollographql.com/docs/apollo-server/migration)

<p align="center">
    <img src="../assets/img/Apollo_migration_flow_chart.png" width="600" alt="Apollo_migration_flow_chart"><br/>
    <span>Apollo Server 마이그레이션 Flow Chart</span>
</p>

<br/>

1. `apollo-server` 패키지를 사용하는가? --> Yes) `startStandaloneServer` 함수를 사용
2. `apollo-server-express` 패키지를 사용하는가? --> Yes) `expressMiddleware` 함수를 사용
3. 둘 다 아닐 경우, 통합 지원 커뮤니티 참조

<br/>

### 2-1. 불필요한 패키지 삭제

- apollo-server-express 패키지는 불필요함

```bash
$ npm uninstall apollo-server-express
```

<br/>

### 2-2. Apollo Server V4를 위한 패키지 설치

- `@apollo/server`, `cors`, `body-parser` 패키지가 필요함
- 설치 시, dependency 등의 이유로 Error가 발생할 수 있기에 이럴 경우 `-f` 플래그를 통해 강제 설치함

```bash
$ npm install @apollo/server cors body-parser -f
```

<br/>

### 2-3. server.js 소스코드 수정

- ApolloServer(), cors(), json(), expressMiddleware() 사용하기

```js
// server.js

const express = require("express");
const {makeExecutableSchema} = require("@graphql-tools/schema");
const {loadFilesSync} = require("@graphql-tools/load-files");
const path = require("path");

// 추가된 새로운 모듈들 가져오기
const {ApolloServer} = require('@apollo/server');
const cors = require('cors');
const {json} = require('body-parser');
const {expressMiddleware} = require('@apollo/server/express4');

const loadedTypes = loadFilesSync("**/*", {
  extensions: ["graphql"],
});

const loadedResolvers = loadFilesSync(path.join(__dirname, "**/*.resolvers.js"));

async function startApolloServer() {
  const PORT = 4000;
  const app = express();

  const schema = makeExecutableSchema({
    typeDefs: loadedTypes,
    resolvers: loadedResolvers,
  });

  const server = new ApolloServer({
    schema,
  });

  await server.start();

  // 기존 "applyMiddleware()"부분을 use()를 사용하여 Apollo middleware를 express 서버와 연결하기
  app.use(
    '/graphql',
    cors(),
    json(),
    expressMiddleware(server, {
      context: async ({req}) => ({token: req.headers.token}),
    }),
  );

  app.listen(PORT, () => {
    console.log('Running a GraphQL API server...');
  })
}

startApolloServer();
```

<br/>

<p align="center">
    <img src="../assets/img/Apollo_server_V4.png" width="700" alt="Apollo_server_V4"><br/>
    <span>Apollo Server V4로 마이그레이션 후, /graphql 접속</span>
</p>