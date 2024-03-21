# 모듈화 - graphql-tools

## 목차

1. [graphql-tools](#1-graphql-tools)
    1. [스키마 분리 - schema, load-files](#1-1-스키마-분리---schema-load-files)
        - [graphql-tools/schema 패키지 설치](#--graphql-toolsschema-패키지-설치)
        - [graphql-tools/schema 적용하기](#--graphql-toolsschema-적용하기)
        - [스키마 소스코드 나눠주기 (모듈화 하기)](#--스키마-소스코드-나눠주기-모듈화-하기)
        - [graphql-tools/load-files 패키지 설치](#--graphql-toolsload-files-패키지-설치)
        - [loadFilesSync로 server.js에 가져오기](#--loadfilessync로-serverjs에-가져오기)
    2. [value 분리](#1-2-value-분리)
        - [데이터 소스코드 나눠주기 (모듈화 하기)](#--데이터-소스코드-나눠주기-모듈화-하기)
        - [server.js로 데이터 모듈 가져오기](#--serverjs로-데이터-모듈-가져오기)

<br/>
<br/>

## 1. graphql-tools

- GraphQL 소스코드가 한 파일(server.js)에 있으면 복잡해지고, 관리 및 유지보수가 어려워질 수 있음
- 따라서, `관련된 부분들끼리 모듈화` 해줄 때 사용하는 도구로서 graphql-tools 사용
- graphql-tools는 GraphQL 파일들을 분리해놓으면 `다시 하나로 모아 합쳐주는 도구임`

<br/>

### 1-1. 스키마 분리 - schema, load-files

### - graphql-tools/schema 패키지 설치

- [npmjs 공식 사이트 - graphql-tools/schema](https://www.npmjs.com/package/@graphql-tools/schema)

```bash
$ npm install @graphql-tools/schema
```

<br/>

### - graphql-tools/schema 적용하기

- `graphql`의 `buildSchema`를 `graphql-tools`의 `makeExecutableSchema`로 변경

```js
// server.js

// 패키지 가져오기
const {makeExecutableSchema} = require("@graphql-tools/schema");

// 스키마 구조 schemaString(아무 이름 상관없음) 새로 만들기
const schemaString = `
    type Query {
        posts: [Post]
        comments: [Comment]
    }

    type Post {
        id: ID!
        title: String!
        description: String!
        comments: [Comment]
    }

    type Comment {
        id: ID!
        text: String!
        likes: Int

    }
`;

// makeExecutableSchema의 typeDefs에 schemaString 넣기
const schema = makeExecutableSchema({
  typeDefs: [schemaString]
});
```

<br/>

### - 스키마 소스코드 나눠주기 (모듈화 하기)

- comments/comments.graphql 폴더 및 파일 생성
- posts/posts.graphql 폴더 및 파일 생성

```js
// server.js의 기존 스키마

const schemaString = `
    type Query {
        posts: [Post]
        comments: [Comment]
    }

    type Post {
        id: ID!
        title: String!
        description: String!
        comments: [Comment]
    }

    type Comment {
        id: ID!
        text: String!
        likes: Int

    }
`;
```

- 위의 스키마 코드를 각각 분리하여 작성

```graphql
# posts/posts.graphql

type Query {
    posts: [Post]
}

type Post {
    id: ID!
    title: String!
    description: String!
    comments: [Comment]
}
```

```graphql
# comments/comments.graphql

type Query {
    comments: [Comment]
}

type Comment {
    id: ID!
    text: String!
    likes: Int
}
```

<br/>

### - graphql-tools/load-files 패키지 설치

- server.js로 앞서 분리한 스키마 코드들을 불러와야하는데 이러한 경우, graphql-tools의 load-files 사용
- [npmjs 공식 사이트 - graphql-tools/load-files](https://www.npmjs.com/package/@graphql-tools/load-files)

```bash
$ npm install @graphql-tools/load-files
```

<br/>

### - loadFilesSync로 server.js에 가져오기

- graphql-tools/load-files는 조건에 만족하는 파일을 불러 올 때 사용함

```js
// server.js

// 패키지 가져오기
const {loadFilesSync} = require("@graphql-tools/load-files");

// loadFilesSync로 현재 폴더(__dirname)에 있는 모든 폴더(**) 속의
// graphql 확장자로 끝나는 모든 파일(*) 불러오기
const loadedTypes = loadFilesSync('**/*', {
  extensions: ['graphql'],
});

const schema = makeExecutableSchema({
  typeDefs: loadedTypes
});
```

<br/>

### 1-2. value 분리

- 현재 rootValue 하나에 데이터 값이 모두 보관되어 있는데 이를 분리하기

<br/>

### - 데이터 소스코드 나눠주기 (모듈화 하기)

- comments/comments.model.js 파일 생성
- posts/posts.model.js 파일 생성

```js
// server.js의 기존 데이터

const root = {
  posts: [
    {
      id: "post1",
      title: "It is a first post",
      description: "It is a first post description",
      comments: [
        {
          id: "comment1",
          text: "It is a first comment",
          likes: 1,
        },
      ],
    },
    {
      id: "post2",
      title: "It is a second post",
      description: "It is a second post description",
      comments: [],
    },
  ],
  comments: [
    {
      id: "comment1",
      text: "It is a first comment",
      likes: 1,
    },
  ],
};
```

- 위의 데이터 코드를 각각 분리하여 작성
- module.exports를 통해 내보내기

```js
// posts/posts.model.js

module.exports = [
  {
    id: "post1",
    title: "It is a first post",
    description: "It is a first post description",
    comments: [
      {
        id: "comment1",
        text: "It is a first comment",
        likes: 1,
      },
    ],
  },
  {
    id: "post2",
    title: "It is a second post",
    description: "It is a second post description",
    comments: [],
  },
];
```

```js
// comments/comments.model.js

module.exports = [
  {
    id: "comment1",
    text: "It is a first comment",
    likes: 1,
  },
];
```

<br/>

### - server.js로 데이터 모듈 가져오기

```js
// server.js

const root = {
  posts: require("./posts/posts.model"),
  comments: require("./comments/comments.model"),
};
```