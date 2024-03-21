# MVC 패턴

## 목차

1. [MVC 패턴이란?](#1-mvc-패턴이란)
2. [앱 소스코드 MVC 패턴으로 만들기](#2-앱-소스코드-mvc-패턴으로-만들기)
    1. [MVC 각 폴더 생성](#2-1-mvc-각-폴더-생성)
    2. [controller 파일 생성](#2-2-controller-파일-생성)
    3. [유저 관련 로직 분류하기 - users.controller](#2-3-유저-관련-로직-분류하기---userscontroller)
    4. [server.js에 users.controller 가져오기](#2-4-serverjs에-userscontroller-가져오기)
    5. [유저 모델 파일 생성 - users.model](#2-5-유저-모델-파일-생성---usersmodel)
    6. [포스트 관련 로직 분류하기 - posts.controller](#2-6-포스트-관련-로직-분류하기---postscontroller)
    7. [server.js에 posts.controller 가져오기](#2-7-serverjs에-postscontroller-가져오기)

<br/>
<br/>

## 1. MVC 패턴이란?

- 소스코드의 복잡성을 해결하기 위해 패턴을 활용함
- `MVC(모델 - 뷰 - 컨트롤러)`는 프로그램의 로직을 상호 연결된 3개의 요소로 나누어 `사용자 인터페이스`, `데이터` 및 `논리 제어`를 개발하는 소프트웨어 아키텍처 패턴
- 소프트웨어 비즈니스 로직과 화면을 구분

<br/>

<p align="center">
    <img src="../assets/img/Expressjs_MVC.png" width="500" alt="Expressjs_MVC"><br/>
    <span>MVC 패턴</span>
</p>

1. `Model` : 데이터와 비즈니스 로직을 관리
2. `View` : 레이아웃과 화면을 처리
3. `Controller` : 명령을 모델과 뷰 부분으로 라우팅

<br/>
<br/>

## 2. 앱 소스코드 MVC 패턴으로 만들기

### 2-1. MVC 각 폴더 생성

- models 폴더, views 폴더, controllers 폴더 생성

<br/>

### 2-2. controller 파일 생성

- controllers/posts.controller.js
- controllers/users.controller.js

<br/>

### 2-3. 유저 관련 로직 분류하기 - users.controller

- user 관련 로직은 users.controller.js에 옮기기

```js
// controllers/users.controller.js

// 유저 모델 가져오기
const model = require('../models/users.model');

// 유저 전체 데이터 가져오기
function
getUsers(req, res) {
  res.send(model);
}

// 유저 1명 데이터 가져오기
function getUser(req, res) {
  const userId = Number(req.params.userId);
  const user = model[userId];
  if (user) {
    res.status(200).json(user);
  } else {
    res.status(404).json({error: "No User Found"});
  }
};

// 유저 데이터 추가하기
function postUser(req, res) {
  if (!req.body.name) {
    return res.status(400).json({
      error: "Missing user name"
    });
  }
  const newUser = {
    name: req.body.name,
    id: model.length
  };
  model.push(newUser);
  res.json(newUser);
};

// 생성한 함수 내보내기
module.exports = {
  getUsers,
  getUser,
  postUser
}
```

<br/>

### 2-4. server.js에 users.controller 가져오기

```js
// server.js

const express = require('express');
// 생성한 컨트롤러 모듈 가져오기
const usersController = require('./controllers/users.controller');

const PORT = 4000;

const app = express();

app.use(express.json());

// 유저 관련 라우팅
app.get('/users', usersController.getUsers);
app.get('/users/:userId', usersController.getUser);
app.post('/users', usersController.postUser);

app.listen(PORT, () => {
  console.log(`Running on port ${PORT}`);
});
```

<br/>

### 2-5. 유저 모델 파일 생성 - users.model

- models/users.model.js 파일 생성

```js
// models/users.model.js

const users = [
  {id: 0, name: 'minsoo'},
  {id: 1, name: 'jeonggon'}
]

module.exports = users;
```

<br/>

### 2-6. 포스트 관련 로직 분류하기 - posts.controller

- post 관련 로직은 posts.controller.js에 옮기기

```js
// controllers/posts.controller.js

function getPost(req, res) {
  res.send('<div><h1>Post Title</h1><p>This is a post</p></div>');
}

module.exports = {
  getPost
}
```

<br/>

### 2-7. server.js에 posts.controller 가져오기

```js
// server.js

const express = require('express');
const usersController = require('./controllers/users.controller');
// 생성한 컨트롤러 모듈 가져오기
const postsController = require('./controllers/posts.controller');

const PORT = 4000;

const app = express();

app.use(express.json());

app.get('/users', usersController.getUsers);
app.get('/users/:userId', usersController.getUser);
app.post('/users', usersController.postUser);

// 포스트 관련 라우팅
app.get('/posts', postsController.getPost);

app.listen(PORT, () => {
  console.log(`Running on port ${PORT}`);
});
```