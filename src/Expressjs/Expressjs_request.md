# request - req.params()

## 목차

1. [req.params](#1-reqparams)
    1. [user 데이터 생성](#1-1-user-데이터-생성)
    2. [라우터 핸들러 생성](#1-2-라우터-핸들러-생성)

<br/>
<br/>

## 1. req.params

- 파라미터를 요청에 전달하여 파라미터에 해당하는 데이터를 출력하기

```
request : GET http://localhost:3000/users

[
    {"id":0, "name":"minsoo"},
    {"id":1, "name":"jeonggon"}
]
```

```
request : GET http://localhost:3000/users/1

{"id":1, "name":"jeonggon"}
```

<br/>

### 1-1. user 데이터 생성

```js
// 유저 데이터

const users = [
  {
    id: 0,
    name: 'minsoo'
  },
  {
    id: 1,
    name: 'jeonggon'
  }
]
```

<br/>

### 1-2. 라우터 핸들러 생성

- 경로 매개변수는 URL의 해당 위치에 지정된 값을 캡쳐하는데 사용되는 URL 세그먼트로 해당 값은 req.params 객체에 채워짐
- `:파라미터 이름` : 콜론과 파라미터 이름으로 해당 파라미터 값 가져오기
- `req.params 객체`로 파라미터 이름에 접근 가능
- [Express 공식 사이트 - Routing](https://expressjs.com/en/guide/routing.html)

```js
// 라우터 핸들러

// "/users"로 접근 시, 응답으로 users 모든 정보 전달
app.get('/users', (req, res) => {
  res.json(users);
  // 또는
  // res.send(users);
});

app.get('/users/:userId', (req, res) => {
  const userId = Number(req.params.userId);
  const user = users[userId];
  if (user) {
    res.json(user);
  } else {
    res.sendStatus(404);
  }
})
```