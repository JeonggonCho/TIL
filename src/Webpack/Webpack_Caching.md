# Caching

## 목차

1. [Caching](#1-caching)
    1. [파일 이름에 해쉬 값 부여 방법](#1-1-파일-이름에-해쉬-값-부여-방법)
        - [소스코드 변경 없이 빌드](#--소스코드-변경-없이-빌드)
        - [소스코드 변경 후 빌드](#--소스코드-변경-후-빌드)
    2. [이전 해쉬 파일을 자동으로 삭제하기](#1-2-이전-해쉬-파일을-자동으로-삭제하기)

<br/>
<br/>

## 1. Caching

- 웹팩 컴파일로 생성된 파일에서 `변경된 내용이 없다면` 브라우저는 `캐시 상태를 유지`하고 그대로 사용하게 됨
- 브라우저가 `변경 사항을 확인`하는 방법 중 하나는 `파일의 이름`임
- 따라서 파일 생성 시, `파일 이름에 해쉬(Hash) 값을 부여`할 수 있음
    - ex) main4363sdsf4535.js

<br/>

### 1-1. 파일 이름에 해쉬 값 부여 방법

- webpack.config.js 설정 파일에서 결과물을 설정하는 `output`의 하드코딩 된 `filename`을 바꿔주어야 함

```js
// webpack.config.js

const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  mode: "development",
  entry: path.resolve(__dirname, "src/index.js"),
  output: {
    path: path.resolve(__dirname, "dist"),

    // output의 filename을 [name][contenthash]로 수정
    filename: "[name][contenthash].js",
  },
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "src/index.html",
    }),
  ],
};
```

<br/>

<p align="center">
    <img src="../assets/img/Webpack_caching_file.png" width="400" alt="Webpack_caching_file"><br/>
    <span>dist 폴더에 해쉬 값이 부여된 파일 생성</span>
</p>

<br/>

<p align="center">
    <img src="../assets/img/Webpack_caching_import.png" width="700" alt="Webpack_caching_import"><br/>
    <span>dist의 index.html에 해쉬값의 JS파일이 자동으로 import 됨</span>
</p>

<br/>

### - 소스코드 변경 없이 빌드

- 아무 파일도 생성되지 않음

<br/>

### - 소스코드 변경 후 빌드

- 새로운 빌드 파일 생성됨
- dist/index.html 파일에도 새로운 JS 빌드 파일이 import됨

<p align="center">
    <img src="../assets/img/Webpack_caching_edit_build.png" width="400" alt="Webpack_caching_edit_build"><br/>
    <span>src/index.js 소스코드 수정 후 다시 빌드 시, 새로운 파일 생성</span>
</p>

<br/>

### 1-2. 이전 해쉬 파일을 자동으로 삭제하기

- `output` 옵션에 `clean: true` 설정 넣기

```js
// webpack.config.js

const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  mode: "development",
  entry: path.resolve(__dirname, "src/index.js"),
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "[name][contenthash].js",

    // 해당 설정을 추가해주면 build시, 새로운 빌드 파일만 남고 이전 파일들을 자동으로 삭제해줌
    clean: true,
  },
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "src/index.html",
    }),
  ],
};
```