# 파일 전송하기 - res.sendFile()

## 목차

1. [파일 전송](#1-파일-전송)
    1. [폴더 및 파일 생성](#1-1-폴더-및-파일-생성)
    2. [sendFile 메서드](#1-2-sendfile-메서드)

<br/>
<br/>

## 1. 파일 전송

### 1-1. 폴더 및 파일 생성

- 정적인 파일을 보관할 폴더 (public) 생성한 후, 원하는 파일 넣기
- ex) public/images/이미지.jpeg

<br/>

### 1-2. sendFile 메서드

- 파일을 보내는데 사용되는 메서드
- `path.join()` : path 모듈이 필요하며 여러 세그먼트를 하나의 경로로 결합시켜 줌
- `__dirname` : 현재 실행하는 파일의 절대 경로를 의미 (아래 코드에서는 controllers/posts.controller.js를 의미함)
- `'..'` : 디텍토리를 한 레벨 밖으로 이동

```js
// controllers/posts.controller.js

// path 모듈 가져오기
const path = require('path');

function getPost(req, res) {
  res.sendFile(path.join(__dirname, '..', 'public', 'images', '이미지.jpeg'));
}

module.exports = {
  getPost
};
```

- 이미지를 응답받고 response의 헤더를 보면 'Content-Type'이 'image/jpeg'로 설정되어 있는 것을 볼 수 있음
- 이는 express가 알아서 타입을 컨트롤 해준 결과임