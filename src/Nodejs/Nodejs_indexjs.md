# index.js 파일

## 목차

1. [index.js 파일이란?](#1-indexjs-파일이란)
    1. [index.js 파일 사용](#1-1-indexjs-파일-사용)
    2. [지향하는 방법](#1-2-지향하는-방법)

<br/>
<br/>

## 1. index.js 파일이란?

- Node.js를 만든 개발자 "Ryan Dahl"이 Node.js에서 후회하는 요소 중 하나로 index.js를 꼽았으며, 이유는 `index.js`가 `모듈의 로딩 시스템을 복잡`하게 만들기 때문이라고 함

- 모든 소스 `모듈`들을 `lib` 폴더에 `index.js` 파일과 함께 넣어주고 소스 모듈들을 내보내기하여 index.js 파일에 모두 불러와 하나로 결합한 뒤, 사용
- 즉, index.js 파일은 여러 모듈을 `하나로 합치는 역할`을 함

<br/>

### 1-1. index.js 파일 사용

- 로딩 시스템이 복잡해지기에 사용 지양

```js
// lib/index.js

module.exports = {
  request: require('./request'),
  response: require('./response')
}
```

```js
// https.js (다른 모듈)

const lib = require('./lib');

function makeRequest(url, data) {
  lib.request.send();
  return lib.response.read();
}
```

<br/>

### 1-2. 지향하는 방법

- index.js 파일을 경유하지 않고 바로 불러와 사용하기

```js
// https.js

const {send} = require('./lib/request');
const {read} = require('./lib/response');

function makeRequest(url, data) {
  send();
  return read();
}
```