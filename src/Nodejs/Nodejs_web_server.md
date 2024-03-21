# Node.js로 웹 서버 생성하기

## 목차

1. [웹 서버 생성하기](#1-웹-서버-생성하기)
    1. [createServer() 메서드](#1-1-createserver-메서드)
    2. [server 객체](#1-2-server-객체)
    3. [requestListener 함수](#1-3-requestlistener-함수)
    4. [req](#1-4-req)
    5. [res](#1-5-res)
    6. [서버에서 클라이언트로 텍스트가 아닌 JavaScript 객체 보내기](#1-6-서버에서-클라이언트로-텍스트가-아닌-javascript-객체-보내기)
2. [HTTP Routing](#2-http-routing)
    1. [라우팅 생성하기](#2-1-라우팅-생성하기)
3. [POST 요청으로 데이터 추가하기](#3-post-요청으로-데이터-추가하기)
    1. [데이터 전달 - fetch](#3-1-데이터-전달---fetch)
    2. [서버에서 처리](#3-2-서버에서-처리)

<br/>
<br/>

## 1. 웹 서버 생성하기

- `HTTP Core 모듈`을 이용해 생성하기

```js
// 웹 서버 생성 예시

const http = require('http');

const PORT = 3000;

const server = http.createServer((req, res) => {
  // requestListener 함수

  // writeHead()는 한 번만 호출되어야 하며 end() 호출되기 전에 호출되어야 함
  // 상태 코드와 응답 헤더를 클라이언트에 보냄
  res.writeHead(200, {
    'Content-Type': 'text/plain'
  });

  // 데이터가 로드되었음을 서버에 알림
  res.end('Hello!');
});

server.listen(PORT, () => {
  console.log(`Listening on port ${PORT}...`);
});

// 127.0.0.1 => localhost
```

<br/>

### 1-1. createServer() 메서드

- http.createServer([options][requestListener]);로 구성
- http.createServer() 메서드는 `server 객체를 생성함`
- server 객체는 `EventEmitter`를 기반으로 만들어졌음 (Observer 디자인 패턴)
    - ex) server.on('request', 콜백함수);
- [Node.js 공식 사이트 - http.createServer()](https://nodejs.org/api/http.html#httpcreateserveroptions-requestlistener)

<br/>

### 1-2. server 객체

- server 객체는 `컴퓨터의 포트를 수신`하고 요청이 만들어질 때마다 `requestListener라는 함수를 실행`할 수 있음
- `server.listen()` : 서버 실행
- `server.close()` : 서버 종료
- server.on('request', 콜백함수), server.on('connection', 콜백함수), ... (close, upgrade,...) 등
- [Node.js 공식 사이트 - server 객체 프로퍼티 및 메서드](https://nodejs.org/api/http.html#serverclosecallback)

<br/>

### 1-3. requestListener 함수

- `서버가 요청을 받을 때마다 호출되는 함수`
- 사용자의 요청과 사용자에 대한 응답을 처리함

<br/>

### 1-4. req

- req(request), res(response) 객체는 `Node.js가 전달해 줌`
- request 객체는 `IncomingMessage의 인스턴스`
- IncomingMessage 객체는 서버에 대한 요청을 나타냄
- [Node.js 공식 사이트 - request 객체 프로퍼티 및 메서드](https://nodejs.org/api/http.html#requestabort)

| request 객체가 가지는 프로퍼티 또는 메서드 | 설명                            |
|-----------------------------|-------------------------------|
| headers                     | 헤더의 이름과 값을 담은 키-값의 쌍의 객체를 리턴  |
| httpVersion                 | 클라이언트에서 보내진 HTTP 버전을 리턴       |
| method                      | request 메서드(GET, POST...)를 리턴 |
| rawHeaders                  | request 헤더의 배열을 리턴            |
| rawTrailers                 | request 트레일러 키-값의 배열을 리턴      |
| setTimeout()                | 밀리세컨즈 초 후에 특정 함수를 호출함         |
| statusCode                  | HTTP response 상태 코드를 리턴       |
| socket                      | 연결을 위한 소켓 객체를 리턴              |
| trailers                    | 트레일러를 담은 객체를 리턴               |
| url                         | request URL 문자열을 리턴           |
| ...                         | ...                           |

<br/>

### 1-5. res

- serverResponse 객체는 requestListener 함수의 `두 번째 매개변수`로 전달됨
- 클라이언트에 웹 페이지를 제공하기 위해 response 객체를 사용함
- [Node.js 공식 사이트 - response 객체 프로퍼티 및 메서드](https://nodejs.org/api/http.html#responseaddtrailersheaders)

| response 객체가 가지는 프로퍼티 또는 메서드 | 설명                                      |
|------------------------------|-----------------------------------------|
| addTrailers()                | HTTP trailing 헤더 추가하기                   |
| end()                        | 서버가 응답이 완료된 것으로 간주해야 한다는 신호             |
| finished                     | response가 완료되면 true, 아니면 false를 리턴      |
| getHeader()                  | 헤더의 값을 리턴함                              |
| headersSent                  | 헤더가 보내지면 true, 아니면 false를 리턴            |
| removerHeader()              | 지정된 헤더 삭제                               |
| sendDate                     | 기본은 true, Date 헤더가 보내지지 않아야하면 false로 설정 |
| setHeader()                  | 지정된 헤더 설정하기                             |
| setTimeout                   | 소켓의 타임아웃 값을 지정된 밀리초 단위로 설정              |
| statusCode                   | 클라이언트에 전달될 상태 코드 설정                     |
| statusMessage                | 클라이언트에 전달될 상태 메세지 설정                    |
| write()                      | 클라이언트에 문자, 문자열 보내기                      |
| writeContinue()              | 클라이언트에 HTTP Continue 메세지 보내기            |
| writeHead()                  | 클라이언트에 상태와 response 헤더 보내기              |
| ...                          | ...                                     |

<br/>

### 1-6. 서버에서 클라이언트로 텍스트가 아닌 JavaScript 객체 보내기

- 헤더의 Content-Type 변경 후, res.end()에 문자열 넣기

```js
// 웹 서버에서 JavaScript 객체 보내기

const server = http.createServer((req, res) => {
  // requestListener 함수
  res.writeHead(200, {
    'Content-Type': 'application/json' // Content-Type 부분 수정
  });

  // 데이터가 로드되었음을 서버에 알림
  res.end(JSON.stringify({
    a: "a",
    b: "b"
  }));
});
```

<br/>
<br/>

## 2. HTTP Routing

- 페이지마다 다른 컨텐츠를 보여줄 수 있도록 경로 분리하기

<br/>

### 2-1. 라우팅 생성하기

- 라우팅 예시로 만들기
- localhost/port`/home` 연결 시, JavaScript 객체 보여주기
- localhost/port`/about` 연결 시, html 텍스트 보여주기
- localhost/port`/dlskd(이상한 경로)` 연결 시, 404 에러 보여주기
- `분기 처리`를 해주면 됨

```js
// 웹 서버에 라우팅 적용하기

const server = http.createServer((req, res) => {

  // '/home' 접속 시
  if (req.url === '/home') {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'application/json');
    // 또는 writeHead로 한 번에 처리가능
    // res.writeHead(200, {
    //   'Content-Type': 'text/plain'
    // });
    res.end(JSON.stringify({
      a: "a",
      b: "b"
    }));

    // '/about' 접속 시  
  } else if (req.url === '/about') {
    res.setHeader('Content-Type', 'text/html');
    res.write('<html>');
    res.write('<body>');
    res.write('<h1>About Page</h1>');
    res.write('</body>');
    res.write('</html>');
    res.end();

    // 그 외의 접속 시
  } else {
    res.statusCode = 404;
    res.end();
  }

});
```

<br/>
<br/>

## 3. POST 요청으로 데이터 추가하기

<p align="center">
   <img src="../assets/img/Nodejs_post_request.png" alt="post_request" width="600"><br/>
   <span>POST로 데이터 추가하기</span>
</p>

<br/>

### 3-1. 데이터 전달 - fetch

- fetch() 메서드를 이용하여 request 보내기

```js
// POST 요청으로 데이터 보내기

fetch('http://localhost:3000/home', {
  method: 'POST',
  body: JSON.stringify({c: "c"})
});
```

<br/>

### 3-2. 서버에서 처리

```js
// 웹 서버

if (req.method === 'POST' && req.url === '/home') {
  // (data)는 요청의 body 데이터를 받음 --> 해당 데이터는 buffer 형식
  req.on('data', (data) => {
    console.log('data', data);
    // 출력
    // data <Buffer 7b 22 63 22 3a 22 ...>

    // buffer 데이터 문자열로 바꾸기
    const stringfiedData = data.toString();
    console.log('stringfiedData', stringfiedData);
    // 출력
    // stringfiedData {"c":"c"}

    // 기존 데이터 객체 dataObject에 객체화한 요청 데이터 JSON.parse(stringfiedData)를 넣기
    Object.assign(dataObject, JSON.parse(stringfiedData));
  })
} else {
...
}
```

- `Object.assign([대상 객체], [출처 객체])` : 출처 객체의 모든 속성을 대상 객체에 붙여넣기(덮어씀)하고 대상 객체를 반환