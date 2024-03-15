# POST 요청 - express.json()

## 목차

1. [POST 요청하기](#1-post-요청하기)
    1. [문제점 - POST 요청으로 들어오는 body가 안 받아짐](#1-1-문제점---post-요청으로-들어오는-body가-안-받아짐)
    2. [해결방안 - bodyParser 모듈, express.json()](#1-2-해결방안---bodyparser-모듈-expressjson)
    3. [POST 요청 시, body가 없을 때 조건 처리](#1-3-post-요청-시-body가-없을-때-조건-처리)

<br/>
<br/>

## 1. POST 요청하기

- POST 요청을 보내서 기존 user 배열 데이터에 새로운 user 추가하기

```js
// 유저 데이터

const users = [
  {
    id: 0,
    name: 'minsoo'
  },
  {
    id: 1,
    name: 'jeonggon'
  }
];
```

- post() 메서드를 통해 라우팅하기

```js
// POST 요청

app.post('/users', (req, res) => {
  const newUser = {
    name: req.body.name,
    id: users.length
  }
  users.push(newUser);

  res.json(newUser);
})
```

<br/>

### 1-1. 문제점 - POST 요청으로 들어오는 body가 안 받아짐

- HTTP 요청은 헤더와 본문(body)로 이루어져 있는데 본문은 HTML 폼 데이터, JSON, XML 등의 형식으로 보낼 수 있음
- Express.js는 기본적으로 HTTP 요청의 본문을 파싱하지 않음
- 따라서 `req.body`를 보면 `undefined`로 데이터가 제대로 받아지지 않는 결과를 보여줌

```js
// frontend
axios.post('/products', {
  name: 'phone', description: "It's new"
})

// backend
const express = require('express');
const app = express();
app.post('/products', (req, res) => {
  console.log('req.body.name : ', req.body.name);
})

// 출력
// undefined
```

<br/>

### 1-2. 해결방안 - bodyParser 모듈, express.json()

- 기존에는 `bodyParser 모듈`을 이용하여 POST 요청 시, body를 받을 수 있음
- Express.js 버전 4.16.0부터 Express에 들어있는 `내장 미들웨어 함수(express.json)`로 bodyParser 모듈을 대체할 수 있음

```js
// bodyParser 미들웨어 모듈 사용

const express = require('express');
const bodyParser = require('body-parser');

const app = express();

// JSON 형식의 요청 본문을 파싱하기 위한 body-parser 미들웨어 추가
app.use(bodyParser.json());

// 사용자 생성 엔드포인트
app.post('/users', (req, res) => {
  // 이하 코드는 유사하게 유지
});
```

```js
// express.json() 사용

// frontend
axios.post('/products', {
  name: 'phone', description: "It's new"
})

// backend
const express = require('express');
const app = express();

app.use(express.json()); // 미들웨어로 express.json() 함수 사용하여 등록

app.post('/products', (req, res) => {
  console.log('req.body.name : ', req.body.name);
})

// 출력
// req.body.name : phone
```

<br/>

### 1-3. POST 요청 시, body가 없을 때 조건 처리

- 조건문을 통해 body의 내용 유무를 확인하고 없으면 에러 메세지의 json을 response로 전달하는 것을 리턴함

```js
app.post('/users', (req, res) => {

  if (!req.body.name) {
    return res.status(400).json({
      error: "Missing user name"
    });
  }

  const newUser = {
    name: req.body.name,
    id: users.length
  }
  users.push(newUser);
  res.json(newUser);
})
```