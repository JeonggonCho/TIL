# Express.js 기본 구조 코드

## 목차

1. [기본 구조 코드 생성](#1-기본-구조-코드-생성)
    1. [package.json 파일 생성](#1-1-packagejson-파일-생성)
    2. [사용할 모듈들 추가 - Express](#1-2-사용할-모듈들-추가---express)
    3. [server.js 만들기](#1-3-serverjs-만들기)
        - [Express를 사용하지 않고 Node.js만 이용](#--express를-사용하지-않고-nodejs만-이용)
        - [Express 사용](#--express-사용)

<br/>
<br/>

## 1. 기본 구조 코드 생성

### 1-1. package.json 파일 생성

- npm init 명령어를 통해 생성
- 프로젝트 정보, 프로젝트에서 사용하는 패키지들의 의존성을 관리하는 곳

```bash
$ npm init -y
```

<br/>

### 1-2. 사용할 모듈들 추가 - Express

- Node.js의 API를 단순화하고 유용한 기능은 추가하여 Node.js를 더 편리하게 사용할 수 있게 해주는 모듈

```bash
$ npm install express
```

<br/>

### 1-3. server.js 만들기

- server.js는 Node.js에서 진입점(시작점)이 되는 파일

<br/>

### - Express를 사용하지 않고 Node.js만 이용

```js
// 웹 서버

const http = require('http');

const PORT = 3000;

const server = http.createServer((req, res) => {
  res.writeHead(200, {
    'Content-Type': 'text/plain'
  })
  res.end('Hello World');
});

server.listen(PORT, () => {
  console.log(`Listening on port ${PORT}`);
})
```

<br/>

### - Express 사용

- Express를 사용하면 따로 status code와 Content-Type을 명시해주지 않아도 됨

```js
// server.js

const express = require('express'); // 모듈 불러오기

// Constants
const PORT = 8080; // Express 서버를 위한 포트 설정
const HOST = '0.0.0.0'; // 호스트 지정

// App
const app = express(); // Express 앱 생성
// "/" 경로로 요청이 오면 'Hello World'를 결과값으로 전달
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(PORT, () => { // 해당 포트, 호스트에서 HTTP 서버 시작
  console.log(`Running on http://${HOST}:${PORT}`)
});
```