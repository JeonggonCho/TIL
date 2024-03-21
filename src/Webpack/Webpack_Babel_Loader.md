# Babel Loader

## 목차

1. [Babel Loader](#1-babel-loader)
    1. [Babel Loader 사용](#1-1-babel-loader-사용)
        - [패키지 설치](#--패키지-설치)
        - [babel-loader 설정](#--babel-loader-설정)

<br/>
<br/>

## 1. Babel Loader

- ES6 이상 JavaScript 코드는 구버전 브라우저에서 지원이 안 되는 경우가 있었음
- 구버전 브라우저에서도 최신 JavaScript 문법을 이용할 수 있도록 `ES5 이하의 코드로 트랜스파일링`하는 기능이 `Babel`임
- 웹팩으로 번들링할 때도 Babel을 적용시킬 수 있게 하는 것이 babel-loader임

<br/>

### 1-1. Babel Loader 사용

### - 패키지 설치

- `babel-loader`, `@babel/core`, `@babel/preset-env` 3가지 패키지 설치

```bash
$ npm install -D babel-loader @babel/core @babel/preset-env
```

<br/>

### - babel-loader 설정

- webpack.config.js 파일에서 module의 rules 안에 작성

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
    clean: true,
  },
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: ["style-loader", "css-loader", "sass-loader"],
      },

      // 추가 작성
      {
        // 타겟은 ".js"로 끝나는 파일
        test: /\.js$/,
        // node_modules 폴더 제외
        exclude: /node_modules/,
        // babel-loader 적용
        use: {
          loader: "babel-loader",
          options: {
            presets: ["@babel/preset-env"],
          },
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "src/index.html",
    }),
  ],
  devServer: {
    static: {
      directory: path.join(__dirname, "dist"),
    },
    compress: true,
    port: 3000,
    open: true,
  },
  devtool: "source-map",
};

```