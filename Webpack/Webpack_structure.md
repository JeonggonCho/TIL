# 폴더 및 파일 구조

## 목차

1. [폴더 및 파일 구조 만들기](#1-폴더-및-파일-구조-만들기)
    1. [dist, src 폴더 만들기](#1-1-dist-src-폴더-만들기)
        - [dist](#--dist)
        - [src](#--src)
    2. [index 파일 생성](#1-2-index-파일-생성)

<br/>
<br/>

## 1. 폴더 및 파일 구조 만들기

### 1-1. dist, src 폴더 만들기

### - dist

- src에 들어있는 코드들이 배포를 위해서 정적인 에셋들로 모이게 되는 공간
- React를 사용 시, `npm run build` 명령어를 사용해서 나오는 폴더와 같은 공간
- 즉, dist에 있는 파일을 이용해 화면에 UI나 기능들이 보이게 됨

<br/>

### - src

- 애플리케이션을 위해 작성해야하는 코드는 이 source 폴더 안으로 들어감

<br/>

### 1-2. index 파일 생성

- dist.index.html 파일 생성
- src.index.js 파일 생성
- 간단하게 index.html 파일에 index.js 파일 연결해주기

<br/>

```html
<!--dist/index.html-->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>Document</title>
</head>
<body>
<h1>Webpack</h1>
<script src="../src/index.js"></script>
</body>
</html>
```

```js
// src/index.js

console.log("Hello world");
```