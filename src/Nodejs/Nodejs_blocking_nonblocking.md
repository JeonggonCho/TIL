# Blocking & Non-blocking

## 목차

1. [Node.js에서 Blocking과 Non-blocking](#1-nodejs에서-blocking과-non-blocking)
    1. [Blocking](#1-1-blocking)
        - [Blocking Function](#--blocking-function)
    2. [Non-blocking](#1-2-non-blocking)
        - [Non-blocking Method](#--non-blocking-method)
    3. [Blocking 코드와 Non-blocking 코드 함께 사용 시, 발생할 수 있는 문제](#1-3-blocking-코드와-non-blocking-코드-함께-사용-시-발생할-수-있는-문제)
        - [문제 상황](#--문제-상황)
        - [해결 방안](#--해결-방안)

<br/>
<br/>

## 1. Node.js에서 Blocking과 Non-blocking

- [Node.js 공식사이트 - Blocking vs Non-blocking](https://nodejs.org/en/learn/asynchronous-work/overview-of-blocking-vs-non-blocking)

<br/>

### 1-1. Blocking

- `Blocking`은 Node.js 프로세스에서 `"추가 JS작업"`이 `"JS가 아닌 작업"`이 완료될 때까지 `기다려야하는 경우`를 의미함

<br/>

<p align="center">
    <img src="../assets/img/Nodejs_blocking.png" alt="blocking" width="350"/><br/>
    <span>Node.js에서 Blocking</span>
</p>

<br/>

### - Blocking Function

- `JSON.stringify()`, `window.alert()`, `fs.readFileSync()` 등...
- 해당 작업을 마쳐야 다음 작업을 수행할 수 있음

```javascript
// Blocking 예시

const fs = require('fs');
const data = fs.readFileSync('/file.md'); // 이 부분이 blocking 발생시킴
console.log(data);
console.log("hello"); // blocking 부분이 완료된 후, 실행됨
```

<br/>

### 1-2. Non-blocking

- Node.js 표준 라이브러리의 `모든 I/O 메서드`는 non-blocking 및 callback 함수를 허용하는 `비동기 버전을 제공함`

<br/>

### - Non-blocking Method

- `fs.readFile()` 등...
- 해당 작업을 `비동기적`으로 수행하게 됨

```javascript
// Non-blocking 예시

const fs = require('fs');
fs.readFile('/file.md', (err, data) => { // 비동기적으로 수행
  if (error) throw err;
  console.log(data);
});
console.log("hello"); // readFile이 완료되지 않아도 실행됨
```

<br/>

### 1-3. Blocking 코드와 Non-blocking 코드 함께 사용 시, 발생할 수 있는 문제

### - 문제 상황

```javascript
// 문제 발생 가능 상황

const fs = require('fs');
fs.readFile('/file.md', (err, data) => {
  if (err) throw err;
  console.log(data);
})
fs.unlinkSync('/file.md');
```

- 위 코드의 의도는 아래와 같을 수 있음

```
1. 파일 읽기 --> 2. 파일 지우기
```

- 하지만 실제로는 아래의 순서로 작업이 수행될 가능성이 높음

```
2. 파일 지우기 --> 1. 파일 읽기
```
<br/>

### - 해결 방안

- 따라서 올바른 순서를 보장하는 `fs.readFile()`의 `콜백 내`에서 `fs.unlink()`에 대한 Non-blocking 호출 배치

```javascript
// 문제 해결 방안

const fs = require('fs');
fs.readFile('/file.md', (readFileErr, data) => {
  if (readFileErr) throw readFileErr;
  console.log(data);
  fs.unlink('/file.md', (unlinkErr) => { // 파일 지우기 작업을 readFile 콜백에서 수행되도록 함
    if (unlinkErr) throw unlinkErr;
  });
})
```