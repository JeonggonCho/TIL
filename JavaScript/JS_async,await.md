# async & await

## 목차

1. [async와 await](#1-async와-await)
    1. [async](#1-1-async)
        - [async 예시](#async-예시)
    2. [await](#1-2-await)
        - [await 예시](#await-예시)

<br>
<br>

## 1. async와 await

-   직관적으로 비동기를 처리할 수 있는 방법

<br>

### 1-1. async

-   `함수 앞`에 붙여서 사용하며, async를 사용한 함수는 `자동으로 Promise를 반환`하는 비동기 처리 함수가 된다.

<br>

### - async 예시

```javascript
// async

function hello() {
    return "hello";
}

async function helloAsync() {
    return "hello Async";
}

console.log(hello());
console.log(helloAsync());

// 출력
// 'hello'
// Promise { 'hello Async' }

// ------------------------------------------------

async function helloAsync() {
    return "hello Async"; // "hello Async"가 resolve의 값
}

helloAsync().then((res) => {
    console.log(res);
});

// 출력
// 'hello Async'
```

-   async를 사용한 함수 역시 Promise객체를 반환하므로 then / catch 메서드 사용가능함

<br>

### 1-2. await

-   await을 비동기 함수의 호출 앞에 붙이면, 비동기 함수를 `동기함수처럼 작동`시킴
-   즉, await이 붙은 함수가 끝나기 전까지 그 아래의 코드들을 수행하지 않음
-   await은 `async가 붙은 함수 내에서만` 사용 가능함
    <br>

### - await 예시

```javascript
// await

function delay(ms) {
    return new Promise((resolve) => {
        setTimeout(resolve, ms);
    });
}

async function helloAsync() {
    await delay(3000);
    return "hello async";
}

async function main() {
    const res = await helloAsync();
    console.log(res);
}

main();

// 출력
// 'hello Async'
```

-   async와 await을 활용하여 비동기 코드 Promise를 반환하는 함수를 동기적인 함수를 호출한 것처럼 사용할 수 있도록 만들어 줌
