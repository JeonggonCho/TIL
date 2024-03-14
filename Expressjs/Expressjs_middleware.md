# middleware

## 목차

1. [middleware란?](#1-middleware란)
    1. [use()](#1-1-use)
    2. [next()](#1-2-next)
2. [middleware 생성하기](#2-middleware-생성하기)

<br/>
<br/>

## 1. middleware란?

<p align="center">
    <img src="../img/Expressjs_middleware.png" width="600" alt="Expressjs_middleware"><br/>
    <span>middleware</span>
</p>

- Express는 자체 기능이 최소화된 라우팅 및 미들웨어 웹 프레임워크로 Express 애플리케이션은 본질적으로 `일련의 미들웨어 기능 호출임`
- 미들웨어 기능은 애플리케이션의 요청, 응답 주기에서 요청 객체(req), 응답 객체(res), next 미들웨어 함수에 접근할 수 있는 기능
- `next 미들웨어 기능`은 일반적으로 `next`라는 변수로 표시함

```js
// middleware 사용 예시

const express = require('express');
const app = express();

app.use((req, res, next) => { // use() : 미들웨어 등록
  console.log('Time:', Date.now());
  next(); // next() : 다음 미들웨어로 이동
})
```

<br/>

### 1-1. use()

- 미들웨어를 등록할 때 사용하는 메서드

<br/>

### 1-2. next()

- 다음 미들웨어로 이동할 때 사용하는 함수

<br/>
<br/>

## 2. middleware 생성하기

- 요청에 대한 로그를 남기는 미들웨어 생성하기

```js
const app = express();

app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
})
```