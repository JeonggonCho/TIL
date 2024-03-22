# Apollo Client 소스코드

## 목차

1. [소스코드 작성](#1-소스코드-작성)
    1. [client.ts](#1-1-clientts)
    2. [main.tsx](#1-2-maintsx)
    3. [todos.ts](#1-3-todosts)
    4. [types.ts](#1-4-typests)

<br/>
<br/>

## 1. 소스코드 작성

### 1-1. client.ts

- src/apollo/client.ts 작성
- [Apollo 공식 사이트 - Configuring the Apollo Client cache](https://www.apollographql.com/docs/react/caching/cache-configuration/)

```ts
// src/apollo/client.ts

import {ApolloClient, InMemoryCache} from "@apollo/client";

const client = new ApolloClient({
    uri: "http://localhost:3001/",
    cache: new InMemoryCache(),
});

export default client;
```

- 많은 옵션을 이용할 수 있음

<br/>

### 1-2. main.tsx

- src/main.tsx 작성
- apollo client 객체를 Provider 컴포넌트로 React 연결해줌으로써 GraphQL API를 사용할 수 있게 됨

```tsx
// src/main.tsx

import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.tsx";
import "./index.css";

// ApolloProvider 컴포넌트 가져오기
import {ApolloProvider} from "@apollo/client";

// client.ts 모듈에서 client 가져오기
import client from "./apollo/client.ts";

ReactDOM.createRoot(document.getElementById("root")!).render(
    <React.StrictMode>
        {/*@apollo/client에서 제공하는 ApolloProvider를 사용하고
        client 속성에 client.ts에서 가져온 client 객체 연결*/}
        <ApolloProvider client={client}>
            <App/>
        </ApolloProvider>
    </React.StrictMode>
);
```

<br/>

### 1-3. todos.ts

- src/apollo/todos.ts에 `query`와 `mutation` 작성
- `@apollo/client` 패키지에서 제공하는 `gql`이라는 template literal tag(``)를 사용해 `JavaScript 문자열을 GraphQL 구문으로 변환해줌`

```ts
// src/apollo/todos.ts

// 템플릿 리터럴 태그 gql 가져오기
import {gql} from "@apollo/client";

// query 작성
// 모든 todos 데이터 가져오기
export const GET_TODOS = gql`
    query getTodos {
        allTodos {
            text
            checked
            id
        }
    }
`;

// mutation 작성
// 할 일 추가하기
export const ADD_TODO = gql`
    // 필수로 필요한 타입 뒤에 느낌표("!") 붙이기
    mutation addTodo($text: String!,$checked: Boolean!) {
        createTodo(text: $text, checked: $checked) {
            // 생성 후 반환받을 데이터
            text
            checked
            id
        }
    }
`;

// 할 일 업데이트하기
export const UPDATE_TODO = gql`
    // 수정은 text만 checked만 또는 둘 다 할 수 있기에 타입에 느낌표 작성하지 않기, id는 필수
    mutation updateTodo($text: String, $checked: Boolean, $id: ID!) {
        updateTodo(text: $text, checked: $checked, id: $id) {
            // 업데이트 후 반환받을 데이터
            text
            checked
            id
        }
    }
`;

// 할 일 삭제하기
export const REMOVE_TODO = gql`
    mutation removeTodo($id: ID!) {
        removeTodo(id: $id) {
            id
        }
    }
`;
```

<br/>

### 1-4. types.ts

- src/types.ts 작성
- TypeScript `인터페이스` 작성

```ts
// src/types.ts

// todo 리스트 하나 객체
export interface IList {
    text: string;
    checked: boolean;
    id: number;
    __typename?: string;
}

// todos 전체 배열
export interface AllTodosCache {
    allTodos: IList[];
}
```