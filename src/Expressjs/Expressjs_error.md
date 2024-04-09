# Express 에러 처리

## 목차

1. [에러 처리](#1-에러-처리)
    1. [에러 처리기](#1-1-에러-처리기)
    2. [비동기 요청 에러](#1-2-비동기-요청-에러)
    3. [next](#1-3-next)

<br/>
<br/>

## 1. 에러 처리

### 1-1. 에러 처리기

- 중간에 에러가 발생하면 해당 error는 `에러 처리기`로 전달되고 이후 미들웨어는 생략되고 에러 처리기에서 에러 메시지를 처리함
- `에러 처리기가 없을 경우`, 에러를 처리할 수 없어 서버가 `crash` 됨 (서비스 장애 발생)

```js
// 에러 처리기 사용 예시

const app = require('express')();

// 에러 발생
// 에러가 발생하게되면 Express는 이 에러를 에러 처리기(handler)로 보냄
app.get('*', function (req, res, next) {
  throw new Error('Error!!');
});

// 에러 발생 시, 에러가 에러 처리기로 바로 이동하기 때문에 이후의 미들웨어는 생략됨
app.get('*', function (req, res, next) {
  console.log('middleware!!');
});

// 에러 처리기
// 에러 처리기는 4개의 인자를 받으며 에러가 발생한 미들웨어의 error를 첫번째 인자로 받음
app.use(function (error, req, res, next) {
  res.json({message: error.message});
});
```

<br/>

### 1-2. 비동기 요청 에러

- 원래는 에러 처리기를 이용하여 에러를 처리할 수 있지만, `비동기 요청으로 인한 에러`는 위와 같이 처리하게 되면 `에러 처리기에서 에러 메시지를 받지 못하기` 때문에 서버가 `crash` 됨

```js
// 비동기 요청 에러 예시

const app = require('express')();

// setImmediate() 메서드로 비동기로 에러를 발생시키기
app.get('*', function (req, res, next) {
  setImmediate(() => {
    throw new Error('Error!!');
  });
});

// 에러 처리기가 있지만 에러를 처리하지 못함
app.use(function (error, req, res, next) {
  res.json({message: error.message});
});

app.listen(3000);
```

<br/>

### 1-3. next

- `비동기 요청`으로 인한 에러는 미들웨어의 3번째 인자인 `next`를 이용해야 함
- next는 `강제로` 에러 처리기에 `에러 메시지를 보내는 역할`을 함

```js
// next로 비동기 요청 에러 처리 예시

const app = require('express')();

app.get('*', function (req, res, next) {
  setImmediate(() => {
    next(new Error('Error!!'));
  });
});

app.use(function (error, req, res, next) {
  res.json({message: error.message});
});

app.listen(3000);
```