# Event Emitter

## 목차

1. [event-driven 시스템](#1-event-driven-시스템)
    1. [Observer Design Pattern](#1-1-observer-design-pattern)
    2. [Event Emitter 클래스](#1-2-event-emitter-클래스)
    3. [Process 모듈](#1-3-process-모듈)

<br/>
<br/>

## 1. event-driven 시스템

- `브라우저(클라이언트)`에서 JavaScript의 경우, 클릭, 키보드 누르기, 마우스 움직임 등의 `이벤트를 통해 사용자와 상호작용`을 처리함
- `백엔드` 측에서 Node.js도 `event-driven 시스템`을 이용해서 작동됨
- 즉, Node.js도 어떠한 이벤트 발생 시, 그에 따른 액션을 실행시킬 수 있음

```
About Node.js

As an asynchronous "event-driven" JavaScript runtime
```

출처 : [Node.js 공식 사이트 - About Node.js](https://nodejs.org/en/about)

<br/>

### 1-1. Observer Design Pattern

- 이러한 event-driven 시스템을 이용하는 것을 Observer Design Pattern이라고 부름
- Observer Design Pattern은 Node.js에 국한된 것이 아닌 개발 전역에서 이용되는 패턴임
- 이 패턴은 특정 `Subject를 관찰`하는 `Observer`가 있고, Observer는 Subject의 `변경사항에 대한 알림 받음`

<br/>

### 1-2. Event Emitter 클래스

- Node.js도 `Event 모듈`을 사용하여 시스템을 구축하는 옵션 제공
  - [Node.js 공식 사이트 - events](https://nodejs.org/api/events.html#events)
- 해당 모듈은 이벤트 처리하는데 사용할 `Event Emitter 클래스` 제공
  - [Event Emitter Class](https://nodejs.org/api/events.html#class-eventemitter)

```javascript
// Event 모듈의 Event Emitter Class 사용 예시

// events의 EventEmitter 클래스 가져오기
const EventEmitter = require('events');

// 이 객체는 on(등록) 및 emission(사용) 메서드를 사용할 수 있음
const celebrity = new EventEmitter();

// on은 이벤트 이름과 이벤트가 트리거 될 때, 실행할 콜백함수를 지정

// Observer1의 on 등록
celebrity.on('update post', () => {
  console.log('This post is so awesome!');
});

// Observer2의 on 등록
celebrity.on('update post', () => {
  console.log('I like this post!');
});

// emit으로 해당 이름의 이벤트 트리거(발생)
celebrity.emit('update post');
```

<br/>

- emit()에 추가 argument를 통해 해당 argument를 on에서 받아 실행할 수 있음

```javascript
// emit()으로 추가 argument 보내기

...
celebrity.on('update post', (type) => {
  console.log(`I like this ${type} post!`);
});

celebrity.emit('update post', 'image');
```

<br/>

### 1-3. Process 모듈

- process 모듈의 객체도 Event Emitter의 인스턴스임
- 여러가지 이벤트들 사용 가능함
- ex) `beforeExit`, `disconnect`, `exit`, `message` 등...
- [Node.js 공식 사이트 - Process events](https://nodejs.org/api/process.html#process-events)

```javascript
// process 모듈 사용 --> 여러 이벤트 중 beforeExit, exit 사용

const process = require('node:process');

process.on('beforeExit', (code) => {
  console.log('Process beforeExit event with code: ', code);
});

process.on('exit', (code) => {
  console.log('Process exit event with code: ', code);
});

console.log('This message is displayed first.');

// 출력
// This message is displayed first.
// Process beforeExit event with code: 0
// Process exit event with code: 0
```