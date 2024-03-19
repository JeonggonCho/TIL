# Webpack 설정 - config 파일

## 목차

1. [Webpack 설정 파일 생성](#1-webpack-설정-파일-생성)
    1. [webpack.config.js](#1-1-webpackconfigjs)
        - [만약 여러 개의 Entry point를 원한다면?](#--만약-여러-개의-entry-point를-원한다면)

<br/>
<br/>

## 1. Webpack 설정 파일 생성

### 1-1. webpack.config.js

- `path` : 파일, 디렉토리의 경로를 다룰 때, Node.js의 내장모듈인 path 모듈 사용
- `resolve()` : 절대 경로 생성
    - ex) path.resolve('Users', 'jeonggon', 'index.html') => 'Users/jeonggon/index.html'

```js
// webpack.config.js


const path = require("path");

module.exports = {
  // 개발 모드
  mode: 'development',
  // 시작점
  entry: path.resolve(__dirname, 'src/index.js'),
  // 웹팩을 통해 생성된 결과물
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
  }
}
```

<br/>

### - 만약 여러 개의 Entry point를 원한다면?

- entry의 값을 객체로 만들기

```js
// webpack.config.js

const path = require("path");

module.exports = {
  // ...
  entry: {
    main: path.resolve(__dirname, 'src/index.js'),
    // ...
  },
}
```