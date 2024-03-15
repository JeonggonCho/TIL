# 정적 파일 제공 - express.static()

## 목차

1. [프론트엔드를 위한 Node.js](#1-프론트엔드를-위한-nodejs)
    1. [폴더 및 파일 생성](#1-1-폴더-및-파일-생성)
    2. [파일 코드 작성](#1-2-파일-코드-작성)
    3. [정적 파일을 화면에서 보기 - express.static()](#1-3-정적-파일을-화면에서-보기---expressstatic)
    4. [express.static() 사용법](#1-4-expressstatic-사용법)
        - [가상 경로 지정](#--가상-경로-지정)
        - [절대 경로 사용](#--절대-경로-사용)

<br/>
<br/>

## 1. 프론트엔드를 위한 Node.js

- 프론트엔드를 위해 `HTML 또는 CSS` 파일을 작성하여 클라이언트의 `요청에 맞게 제공`할 수 있음
- 이러한 정적인 파일들을 제공해주기 위해서는 Express에서 제공하는 express.static() 메서드를 사용하면 됨

<br/>

### 1-1. 폴더 및 파일 생성

- public/css/style.css
- public/index.html

<br/>

### 1-2. 파일 코드 작성

- index.html 파일 작성

```html
<!--index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
<h1>It is a image</h1>
<img src="images/이미지.jpeg" alt="이미지"/>
</body>
</html>
```

<br/>

- style.css 파일 작성

```css
/*style.css*/

body {
    font: 14px Arial, Sans-Serif;
}

img {
    width: 300px;
}
```

<br/>

### 1-3. 정적 파일을 화면에서 보기 - express.static()

- Node.js가 express.static() 메서드를 사용하여 정적 파일을 브라우저의 요청에 맞게 제공함
- `express.static()` : 이미지, CSS, JavaScript 파일과 같은 정적 파일을 제공하려면 Express의 express.static() 내장 미들웨어 기능을 사용할 수 있음

<br/>

### 1-4. express.static() 사용법

- express.static() 메서드를 사용하여 public 디렉토리 안에 있는 모든 파일을 제공하기

```js
// server.js

// public 디렉토리를 미들웨어에 등록
app.use(express.static('public'));
```

- `http://localhost:4000` 으로 접근하면 index.html 페이지를 볼 수 있음

```
http://localhost:4000/images/이미지.jpeg
http://localhost:4000/css/style.css
```

<br/>

### - 가상 경로 지정

- express.static()의 첫 번째 인자에 경로 넣기
- 해당 메서드가 제공하는 파일에 대한 가상 경로 접두사(경로가 실제로 파일 시스템에 존재하지 않음)를 생성하기 위해 마운트 경로를 지정

```js
// server.js

app.use('/static', express.static('public'));
```

- `http://localhost:4000/static` 으로 접근하면 index.html 페이지를 볼 수 있음

```
http://localhost:4000/static/images/이미지.jpeg
http://localhost:4000/static/css/style.css
```

<br/>

### - 절대 경로 사용

- express.static() 함수에 제공하는 경로는 Node 프로세서를 시작하는 디렉토리에 `상대적임`
- 만약, `다른 디렉토리`에서 express 앱을 실행하는 경우, `에러가 발생함`
- 따라서 절대 경로를 사용하는 것이 안전하기에 사용을 지향함
- 절대 경로는 express.static()에 바로 public을 넣는 것이 아닌 절대 경로에서 public 넣기

```js
// server.js

const path = require('path');
app.use('/static', express.static(path.join(__dirname, 'public')));
```