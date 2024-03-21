# development server

## 목차

1. [development server](#1-development-server)
    1. [development server 설정하기](#1-1-development-server-설정하기)
    2. [devServer 이용할 Script 생성](#1-2-devserver-이용할-script-생성)
    3. [npm run dev 명령어 실행](#1-3-npm-run-dev-명령어-실행)

<br/>
<br/>

## 1. development server

- Visual Studio Code의 확장 프로그램인 `Live server`를 통해 HTML UI를 확인할 수 있는데 웹팩을 이용해서 실행할 수 있음
- React에서도 npm run start 명령어 실행 시, Live server가 아닌 자동으로 템플릿을 실행하는 것을 볼 수 있음

<br/>

### 1-1. development server 설정하기

- wepack.config.js 파일에 devServer 옵션을 추가해주기

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
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: "index.html",
      template: "src/index.html",
    }),
  ],

  // devServer 옵션 추가
  devServer: {
    static: {
      // 경로 넣기
      directory: path.join(__dirname, "dist"),
    },
    compress: true, // compress: true는 제공되는 모든 항목에 대해 gzip 압축을 활성화함
    port: 3000, // 실행할 포트
    open: true, // 서버가 시작된 후, 기본 브라우저를 열도록 지시
  },
};
```

<br/>

### 1-2. devServer 이용할 Script 생성

- devServer를 이용하기 위해서는 package.json에 script 추가

```json
// package.json

{
  "scripts": {
    "dev": "webpack serve"
  }
}
```

<br/>

### 1-3. npm run dev 명령어 실행

- scripts의 serve를 사용하기 위해서는 `webpack-dev-server` 패키지를 설치해야한다고 알림
- 설치 진행 질문에 y 입력하여 패키지 설치하기

```bash
[webpack-cli] For using 'serve' command you need to install: 'webpack-dev-server' package.
[webpack-cli] Would you like to install 'webpack-dev-server' package? (That will run 'npm install -D webpack-dev-server') (Y/n) 
```

- 설치가 완료되면 설정한 포트에서 서버를 실행하고 브라우저가 자동으로 켜지는 것을 확인할 수 있음