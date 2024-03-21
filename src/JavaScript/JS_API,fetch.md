# API & fetch

## 목차

1. [API](#1-api)
    1. [사전적 정의](#1-1-사전적-정의)
    2. [비유](#1-2-비유)
2. [fetch](#2-fetch)
    1. [fetch 예시](#2-1-fetch-예시)

<br>
<br>

## 1. API

### 1-1. 사전적 정의

-   API(Application Programming Interface) : 응용 프로그램 인터페이스
-   응용 프로그램에서 사용할 수 있도록 운영체제나 프로그래밍 언어가 제공하는 `기능을 제어`할 수 있게 만든 `인터페이스`를 의미
-   주로 파일제어, 창제어, 화상처리, 문자제어 등을 위한 인터페이스 제공

<br>

### 1-2. 비유

![API 비유](../assets/img/JS_API.png)

-   음식을 주문하고 받는 것과 웹에서 정보를 요청하고 응답받는 것과 유사함

<br>
<br>

## 2. fetch

-   JavaScript에서 `API를 호출`할 수 있도록 도와주는 `내장함수`
-   fetch는 비동기 Promise를 리턴하게 되고 이 결과 값을 json() 메서드를 통해 확인할 수 있음

<br>

### 2-1. fetch 예시

```javascript
// fetch

async function getData() {
    let rawResponse = await fetch("https://jsonplaceholder.typicode.com/posts");
    let jsonResponse = await rawResponse.json();
    console.log(jsonResponse);
}

getData();

// 출력
/*
[
  {
    userId: 1,
    id: 1,
    title: 'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
    body: 'quia et suscipit\n' +
      'suscipit recusandae consequuntur expedita et cum\n' +
      'reprehenderit molestiae ut ut quas totam\n' +
      'nostrum rerum est autem sunt rem eveniet architecto'
  },
  {
    userId: 1,
    id: 2,
    title: 'qui est esse',
    body: 'est rerum tempore vitae\n' +
      'sequi sint nihil reprehenderit dolor beatae ea dolores neque\n' +
      'fugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\n' +
      'qui aperiam non debitis possimus qui neque nisi nulla'
  }, ...
]
*/
```

-   `json() 메서드` : 인자를 받지 않으며, resolve된 Promise 객체를 반환
