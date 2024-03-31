# Middleware

## 목차

1. [Redux Middleware](#1-redux-middleware)
2. [로깅 미들웨어 만들기](#2-로깅-미들웨어-만들기)
    1. [로깅 미들웨어 함수 생성](#2-1-로깅-미들웨어-함수-생성)
        - [미들웨어 등록](#--미들웨어-등록)
        - [store 생성 코드 수정](#--store-생성-코드-수정)

<br/>
<br/>

## 1. Redux Middleware

- Redux Middleware는 Action을 dispatch 전달하고 `reducer에 도달하는 순간 사이`에 `사전에 지정된 작업을 실행`할 수 있게 해주는 중간자
- ex) 로깅, 충돌 보고, 비동기 API와 통신, 라우팅 등...

<br/>

<p align="center">
    <img src="../../assets/img/Redux_middleware.gif" width="600" alt="middleware"><br/>
    <span>Redux Middleware 컨셉 다이어그램</span>
</p>

<br/>

```tsx
// 미들웨어 구조 예시

// 구조1
const loggerMiddleware1 = (store) => (next) => (action) => {
    // 코드 작성
};

// 구조2
const loggerMiddleware2 = function (store) {
    return function (next) {
        return function (action) {
            // 코드 작성
        };
    };
};

// 구조2는 구조1을 풀어서 작성한 것으로 동일한 함수임
```

<br/>

## 2. 로깅 미들웨어 만들기

- Redux를 이용할 때, 나오는 로그를 찍어주는 미들웨어 생성하기

<br/>

### 2-1. 로깅 미들웨어 함수 생성

### - 미들웨어 등록

- `applyMiddleware()` : redux 라이브러리에서 제공하는 하나 이상의 미들웨어를 받은 후, 함수를 리턴하는 함수 (미들웨어 등록)

```tsx
// src/index.tsx

import {applyMiddleware, createStore} from "redux";

const loggerMiddleware = (store: any) => (next: any) => (action: any) => {
    // 로그 기록 출력
    console.log("store", store);
    console.log("action", action);

    // 다음 미들웨어로 넘어가기
    next(action);
};

// loggerMiddleware 등록
const middleware = applyMiddleware(loggerMiddleware);
```

<br/>

### - store 생성 코드 수정

```tsx
// src/index.tsx

import {applyMiddleware, createStore} from "redux";

const loggerMiddleware = (store: any) => (next: any) => (action: any) => {
    console.log("store", store);
    console.log("action", action);
    next(action);
};

const middleware = applyMiddleware(loggerMiddleware);

// 생성 후 등록한 미들웨어를 store 생성 시, 3번째 인자로 넣어주기
// creatStore의 2번째 인자는 preloadedState(any)로 초기 상태임
const store = createStore(rootReducer, undefined, middleware);
```

<br/>

<p align="center">
    <img src="../../assets/img/Redux_loggerMiddleware.gif" width="700" alt=""><br/>
    <span>로깅 미들웨어를 통한 로그 출력</span>
</p>