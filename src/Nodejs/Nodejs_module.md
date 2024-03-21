# 모듈

## 목차

1. [모듈](#1-모듈)
    1. [모듈을 사용하는 이유](#1-1-모듈을-사용하는-이유)
    2. [Common JS 모듈 시스템 (CJS)](#1-2-common-js-모듈-시스템-cjs)
        - [CJS 모듈의 특징](#cjs-모듈의-특징)
    3. [ES 모듈 시스템 (ESM)](#1-3-es-모듈-시스템-esm)
        - [ESM 사용법](#esm-사용법)
        - [ESM 특징](#esm-특징)
    4. [HTTP 서버 구축](#1-4-http-서버-구축)
        - [HTTP 모듈 이용하기](#--http-모듈-이용하기)
    5. [모듈 생성하기](#1-5-모듈-생성하기)
        - [예시) HTTPS 모듈 생성하기](#--예시-https-모듈-생성하기)
    6. [모듈 내보내기 여러 종류](#1-6-모듈-내보내기-여러-종류)
    7. [모듈 캐싱](#1-7-모듈-캐싱)

<br/>
<br/>

## 1. 모듈

- 모듈은 코드를 구성하고 `재사용 가능한` 단위로 분할하는데 사용되는 개념
- 모듈은 일반적으로 `하나의 파일`에 `정의`되며, 해당 파일 안에서 필요한 변수, 함수, 클래스 등을 내보내고(export) 가져올(import) 수 있다.
- 즉, Node.js에서 module은 `필요한 함수들의 집합`을 의미함
- JavaScript에서 사용하는 모듈 시스템은 대표적으로 `Common JS 모듈 시스템`과 `ES 모듈 시스템`이 있다.

<br/>

### 1-1. 모듈을 사용하는 이유

- 프로그램은 여러 모듈들이 모여서 만들어짐

```
여러가지 모듈 (http모듈 + axios모듈 + path모듈 + console모듈 + ...) = 프로그램
```

<br/>

- 모듈의 장점

1. 재사용성
2. 연관성있는 코드끼리 모아서 코드를 정리하고 관리할 수 있음
3. 모듈 전체를 가져오지 않고 필요한 함수나 변수, 클래스만 선택하여 가져올 수 있음

<br/>

### 1-2. Common JS 모듈 시스템 (CJS)

```javascript
// calc.js

const add = (a, b) => a + b;
const sub = (a, b) => a - b;

module.exports = {
  moduleName: "calc module",
  add: add,
  sub: sub,
};
```

```javascript
// index.js

const calc = require("./calc");

console.log(calc);

// 출력
/*
{
    moduleName: "calc module",
    add: [Function: add],
    sub: [Function: sub],
};
*/
```

- Node.js에서는 `module.exports`를 사용하여 `객체 단위`로 모듈을 내보내기 할 수 있음
- 내보낸 모듈은 다른 파일에서 `require('경로');`를 통해 가져올 수 있음
- 하지만, 이는 Node.js의 내장함수를 활용한 방법으로 Vanila JavaScript에선 사용이 제한 될 수 있음

<br>

### - CJS 모듈의 특징

- Node.js 초기부터 지원
- require()은 즉시 스크립트를 실행하는 구조임
- top-level await이 불가능하므로 `동기적으로 작동`함
- 동기적으로 작동하므로 `Promise를 리턴하지 않고`, module.exports에 `설정된 값만을 리턴`함
- 가져오는 `순서에` 따라서 스크립트를 실행함

<br>

### 1-3. ES 모듈 시스템 (ESM)

- `import`로 모듈에 접근하고 `export`로 모듈을 내보냄
- `ECMA Script`에서 지원하는 방식임
- `Node 14`에서는 CJS, ESM이 공존하는데 동시에 사용하기 위해서는 별도의 처리가 필요함
- 모듈 시스템을 CJS(기본 값)에서 ESM으로 변경할 경우, JavaScript 일부 동작이 변경되어 `호환성에 문제`가 발생할 수 있음

<br>

### - ESM 사용법

1. `package.json`에 `"type": "module"`을 설정

```javascript
// package.json

{
  "type"
:
  "module",
}
```

<br>

2. 모듈에 접근할 때, import 사용

```javascript
// import 사용

import util from "utils";
import {add} from "utils";
import {add as add_func} from "utils";
```

<br>

3. 모듈 내보낼 때, export 사용

   <named exports (이름있는 내보내기) 사용>

    ```javascript
    // named exports 사용
    // calculator.js

    export const sum = (x, y) => x + y;
    export const multiply = (x, y) => x * y;
    ```

    - `named exports` : 이름을 지정하여 내보내기
    - named exports는 명명된 이름으로만 모듈을 불러올 수 있음
    - 여러 개의 함수, 변수, 또는 클래스 등을 동일한 모듈에서 내보내기 가능

    <br>

    ```javascript
    import { sum, multiply } from "./calculator";
    import { sum as sum_func } from "./calculator.js"; // 다른 별칭으로 수정

    console.log(sum(2, 4)); // 출력: 6
    console.log(sum_func(2, 4)); // 출력: 6
    ```

    - 사용할 때 `중괄호({})를 사용`하여 각각의 항목을 명시적으로 가져와야 함
    - `as`를 사용해 명명된 이름을 다른 별칭으로 수정할 수 있음

    <br>
    <br>

   <default exports (기본 내보내기) 사용>

    ```javascript
    // default exports 사용
    // module.js

    export default (x, y) => x + y;

    // ----------------------------------------------------

    // module.js
    const subtract = (a, b) => a - b;
    export default subtract;
    ```

    - `default exports` : 별도의 이름을 저장하지 않고 내보냄
    - 모듈 당 `하나`의 기본 내보내기만 할 수 있음

    <br>

    ```javascript
    // index.js

    import sub from "./module";
    ```

    - 사용할 때 `중괄호({}) 없이` 가져와야 함
    - 이름이 없는 export이므로, import할 때 `내가 지정한 이름`을 사용 가능

    <br>
    <br>

   <혼합 사용>

    ```javascript
    // module.js

    const subtract = (a, b) => a - b;
    export { subtract as default, add, multiply };
    ```

    ```javascript
    // index.js

    import subtract, { add, multiply } from "./module";
    ```

    - 기본적으로 `subtract`가 `기본 내보내기`로 사용
    - `add`와 `multiply`는 `이름이 있는 내보내기`로 사용
    - 이러한 모듈 시스템은 코드를 모듈화하고 재사용성을 높이는 데 도움이 됨

<br>

### - ESM 특징

- top-level await을 지원하므로 module loader가 `비동기 환경에서 실행`됨
- 스크립트를 바로 실행하는 CJS와 달리, import / export 구문을 찾아 `스크립트를 파싱함`
- 파싱단계에서 import / export 에러를 감지 할 수 있음
- 모듈을 병렬(비동기)로 다운로드하지만 실행은 순차적(동기)으로 함
- import / export를 지원하지 않는 브라우저가 있기에 ESM 사용을 위해 번들러가 필요

<br/>

### 1-4. HTTP 서버 구축

### - HTTP 모듈 이용하기

- HTTP 모듈은 Node.js의 built-in 모듈 (코어 모듈)로서 해당 모듈을 사용하여 `서버를 구현`할 수 있음
- `3000번 포트` 이용하여 로컬 호스트에 브라우저 접근 시, "hello, world" 문구 출력하기

```js
// HTTP 모듈로 서버 만들기 예시

// 모듈 가져오기
const http = require('http');

// 3000번 포트 이용
const port = 3000;

// http 서버 만들기 - createServer() 메서드
// req : 요청 세부 정보 제공하며 이를 통해 요청 헤더 및 데이터에 액세스 가능 (http.IncomingMessage 객체)
// res : 클라이언트에 반환할 데이터를 채우는데 사용 (httpServerResponse 객체)
const server = http.createServer((req, res) => {
  res.statusCode = 200; // 성공적 응답을 위해 상태코드를 200으로 설정
  res.setHeader('Content-Type', 'text/html'); // res.setHeader('콘텐츠 유형', '텍스트/html')
  res.end('<h1>hello, world</h1>'); // end() 로 내용 추가 및 응답 종료
});

// 서버는 지정된 포트 3000번 에서 수신 대기
// 서버가 준비되면 수신 콜백함수 호출
server.listen(port, () => {
  console.log(`Server running at port ${port}`);
});
```

<br/>

### 1-5. 모듈 생성하기

### - 예시) HTTPS 모듈 생성하기

- HTTP와 HTTPS의 차이 : HTTPS는 데이터를 `암호화`하여 요청을 보내고 서버에서 암호화되어 온 결과 데이터를 `복호화함`
- 실제 기능을 구현하는 것이 아닌 모듈 구조를 학습하기

<br/>

1. 필요한 것

    - `https.js` : request.js + response.js
    - `request.js` : 요청에 필요한 함수 포함
    - `response.js` : 데이터 결과를 가져오는데 필요한 함수 등을 포함

<br/>

2. https.js 파일

```js
// https.js

function makeRequest(url, data) {
  // 요청 보내기
  // 데이터 return 하기
}
```

<br/>

3. request.js 파일

```js
// request.js

// 예시로 암호화 함수 encrypt 생성
// 해당 함수는 data를 받음
function encrypt(data) {
  return "encrypted data";
}

// send 함수는 url과 data를 받아 data를 암호화하고 콘솔에 메세지 출력
function send(url, data) {
  const encryptedData = encrypt(data);
  console.log(`${encryptedData} is being sent to ${url}`);
}
```

<br/>

4. response.js 파일

```js
// response.js

function decrypt(data) {
  return "decrypted data";
}

function read() {
  return decrypt('data');
}
```

<br/>

5. module 키워드를 사용하여 필요한 함수 내보내기

```js
// request.js

...
module.exports = {
  send
}
```

```js
// response.js

...
module.exports = {
  read
}
```

<br/>

6. 내보낸 함수, 객체, 클래스 가져와서 사용하기

```js
// https.js

const request = require('./request'); // request에 send 담김
const response = require('./response'); // response에 read 담김

function makeRequest(url, data) {
  // 요청 보내기
  request.send(url, data);
  // 데이터 return 하기
  return response.read();
}

const responseData = makeRequest('https://naver.com', 'any data');
console.log(responseData);


// 출력
// encrypt data is being sent to https://naver.com
// decrypted data
```

<br/>

### 1-6. 모듈 내보내기 여러 종류

- 상수 내보내기

```js
module.exports.A = 1;

// 또는

const A = 1;

module.exports = {
  A
};
```

<br/>

- 함수 내보내기

```js
module.exports.add = function add(a, b) {
  return a + b;
}

// 또는

// module 키워드 생략
exports.add = function add(a, b) {
  return a + b;
}

// 또는

function add(a, b) {
  return a + b;
}

module.exports = {
  add
};
```

<br/>

### 1-7. 모듈 캐싱

- 모듈에서 다른 모듈을 가져올 때(load), ES 모듈과 CommonJS 모듈 모두 해당 모듈을 캐싱하게 됨
- 따라서 한 번, 가져오면 다시 로드할 필요가 없음