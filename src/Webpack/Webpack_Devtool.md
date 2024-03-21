# Devtool

## 목차

1. [Devtool](#1-devtool)
    1. [Source Map](#1-1-source-map)
    2. [Devtool 설정](#1-2-devtool-설정)
    3. [npm run dev, npm run build](#1-3-npm-run-dev-npm-run-build)

<br/>
<br/>

## 1. Devtool

- `소스 맵(source map)`의 생성 여부와 생성 방법을 제어함
- devtool에서 사용할 수 있는 많은 옵션이 존재함
- 각 옵션마다 퍼포먼스와 퀄리티, 특징, 장단점이 다르기에 상황에 적절한 옵션을 사용하는 것을 추천함
- [Webpack 공식 사이트 - devtool](https://webpack.kr/configuration/devtool/)

<br/>

### 1-1. Source Map

- 웹 사이트 성능을 향상시키기 위해 JavaScript, CSS 파일을 `결합하고 압축` 과정을 수행할 수 있음
- `압축 파일 내에서 코드를 디버깅`하기 위해서 `소스 맵`을 이용해 디버깅
- 소스 맵은 `압축 파일(빌드)`의 코드를 `소스 파일(원본)`의 원래 코드와 `매핑(연결)`하는 방법을 제공함
- 개발자 도구의 Sources 탭에서 확인할 수 있음

<br/>

### 1-2. Devtool 설정

- 여러 옵션 중 `source-map`이라는 devtool 사용해보기
- webpack.config.js 파일에서 devtool 설정 작성

```js
// webpack.config.js

const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  // ...
  // devtool 설정 옵션 작성
  devtool: "source-map",
};
```

<br/>

### 1-3. npm run dev, npm run build

- 현재 `source-map` 옵션이 적용된 상태에 `npm run dev` 명령어를 실행하여 서버를 열고 브라우저 개발자 도구의 Source 탭을 확인하면 빌드된 코드가 아닌 원본 코드를 확인해 볼 수 있음
- 또한 `npm run build` 명령어를 통해 다시 빌드를 해보면 `소스 맵의 파일이 같이 생성`되는 것을 확인할 수 있음
    - ex) main1234asdf.js(빌드 파일)와 main1234asdf.js.map(소스 맵)