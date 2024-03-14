# response - res.json() vs res.send() vs res.end()

## 목차

1. [res.json() vs res.send()](#1-resjson-vs-ressend)
    1. [send로 JavaScript 객체 보내기](#1-1-send로-javascript-객체-보내기)
    2. [res.json 소스코드와 res.send 소스코드 비교](#1-2-resjson-소스코드와-ressend-소스코드-비교)
        - [res.json 소스코드](#--resjson-소스코드)
        - [res.send 소스코드](#--ressend-소스코드)
    3. [res.json() vs res.send() 결론](#1-3-resjson-vs-ressend-결론)
2. [res.send() vs res.end()](#2-ressend-vs-resend)
    1. [res.end()](#2-1-resend)
        - [ETag](#--etag)
        - [꼭 res.end()를 이용해 세션을 종료해야하는가?](#--꼭-resend를-이용해-세션을-종료해야하는가)

<br/>
<br/>

## 1. res.json() vs res.send()

- res.send와 res.json을 사용하는 것은 기능상 `거의 동일함`

<br/>

### 1-1. send로 JavaScript 객체 보내기

```js
app.get('/', (req, res) => {
  res.send({a: "a"});
})
```

```
Response Headers

Content-Type: application/json;
```

- res.send를 사용해도 헤더를 확인해보면 Express가 자동으로 `Content-Type`을 올바르게 설정해줌

<br/>

### 1-2. res.json 소스코드와 res.send 소스코드 비교

### - res.json 소스코드

- res.json()은 argument로 객체를 받음

```js
res.json = function json(obj) {
  var val = obj;

  //...

  // settings
  var app = this.app;
  var escape = app.get('json escape');
  var replacer = app.get('json replacer');
  var spaces = app.get('json spaces');
  var body = stringify(val, replacer, spaces, escape);

  // Content-Type
  if (!this.get('Content-Type')) {
    this.set('Content-Type', 'application/json');
  }

  return this.send(body);
}
```

1. obj를 JSON 문자열로 변환 (stringify)
2. Content-Type 헤더가 세팅되지 않았을 경우, this(res 객체)에 Content-Type으로 application/json을 세팅
3. res.send(body)를 실행

```
호출

res.json(obj) --> res.send(string)
```

<br/>

### - res.send 소스코드

- res.send()는 argument로 여러 타입을 받을 수 있음

```js
res.send = function send(body) {
  var chunk = body;

  // ...

  switch (typeof chunk) {
    // string defaulting to html
    case 'string':
      if (!this.get('Content-Type')) {
        this.type('html');
      }
      break;
    case 'boolean':
    // ...
    case 'number':
    // ...
    case 'object':
      if (chunk === null) {
        chunk = '';
      } else if (Buffer.isBuffer(chunk)) {
        if (!this.get('Content-Type')) {
          this.type('bin');
        } else {
          return this.json(chunk);
        }
      }
      break;
  }
}
```

1. body의 타입을 체크
2. object일 경우 res.json 호출
3. res.json에서 문자열로 바꾼 후, res.send를 다시 호출하여 처리

```
호출

res.send(obj) --> res.json(obj) --> res.send(string)
```

<br/>

### 1-3. res.json() vs res.send() 결론

- res.send와 res.json 사이에는 외부에서 보기에는 차이가 없지만, res.send가 내부에서 호출이 한 번 더 발생하는 것을 볼 수 있음
- 따라서 `object(객체)를 전달`하는 경우에는 호출도 적게하고 직관적인 `res.json`을 사용하는 것을 지향함

<br/>
<br/>

## 2. res.send() vs res.end()

### 2-1. res.end()

- res.end() 메서드는 response가 있고 일단 데이터를 수집하거나 호출자에게 제공하고 싶은 다른 작업을 수행한 뒤 `마지막 단계로 세션을 종료해야 함`
- res.end()는 res.send()와 마찬가지로 괄호 안에 `컨텐츠를 넣어 보낼 수도 있음`
    - ex) res.end('\<p>some html\</p>')
- 하지만 res.end()를 이용하면 헤더에 `Content-Type`과 `ETag`가 없음

<br/>

### - ETag

- ETag HTTP 응답 헤더는 `리소스의 특정 버전에 대한 식별자`로서 컨텐츠가 변경되지 않은 경우, 웹 서버에서 전체 응답을 보내도 되지 않아 캐시를 보다 효율적으로 사용하고 대역폭을 절약할 수 있음
- 변경 유무를 체크하여 `성능을 높여줌`
- 따라서 res.end()보다는 res.send()를 사용하는게 성능적인 측면에서 좋음

<br/>

### - 꼭 res.end()를 이용해 세션을 종료해야하는가?

1. res.end()로 종료해야하는 때
    - 데이터를 제공하지 않고 응답을 종료하려면 res.end()를 사용할 수 있음
    - 404 페이지에 유용할 수 있음
    - ex) res.status(404).end();

2. res.end()로 종료하지 않아도 되는 때
    - 데이터를 res.json()이나 res.send()로 보내면 자동으로 세션을 종료함