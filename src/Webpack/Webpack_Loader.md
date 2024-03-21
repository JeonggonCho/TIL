# Loader

## 목차

1. [Loader](#1-loader)
    1. [Loader 설정](#1-1-loader-설정)
        - [패키지 설치](#--패키지-설치)
        - [webpack.config.js 파일 작성](#--webpackconfigjs-파일-작성)
        - [styles 폴더 및 scss 파일 생성](#--styles-폴더-및-scss-파일-생성)
        - [생성한 scss 파일 index.js로 import](#--생성한-scss-파일-indexjs로-import)
        - [빌드하기](#--빌드하기)

<br/>
<br/>

## 1. Loader

- 로더는 웹팩이 웹 애플리케이션을 해석할 때, `JavaScript 파일이 아닌 웹 자원`(HTML, CSS, image, 폰트 등)들을 `변환`할 수 있도록 도와주는 속성

<br/>

### 1-1. Loader 설정

### - 패키지 설치

- 스타일링을 위해 sass 사용해보기
- 따라서 `css-loader`, `style-loader`, `sass`, `sass-loader` 설치

```bash
$ npm i -D css-loader style-loader sass sass-loader
```

- 스타일링에 주로 사용되는 모듈

    - `style-loader` : DOM에 스타일로 모듈 내보내기를 추가함
    - `css-loader` : resolve된 가져오기로 CSS 파일을 로드하고 CSS 코드를 반환함
    - `less-loader` : LESS 파일을 로드하고 컴파일함
    - `sass-loader` : SASS/SCSS 파일을 로드하고 컴파일함
    - `postcss-loader` : PostCSS를 사용해 CSS/SSS 파일을 로드하고 변환함
    - `stylus-loader` : Stylus 파일을 로드하고 컴파일함

<br/>

- css-loader와 style-loader는 항상 같이 사용됨

<br/>

- css-loader

    - `css-loader`는 `@import 및 url()`을 `import/require()`과 같이 해석하고 처리함
    - JSX 같은 JavaScript를 이용한 마크업 스타일링을 입힐 경우, import "style.css"와 같이 css 파일을 import 함
    - 즉, css 파일을 하나의 모듈로 취급함

<br/>

- style-loader

    - css-loader를 이용해 웹팩 의존성 트리에 추가한 후, css에 작성한 string 값들을 style-loader를 이용해서 DOM에 \<style>\</style>로 넣어줌
    - 즉, JavaScript 파일로 불러온 `css 파일`이 HTML 파일 안의 `style 태그로 존재할 수 있게 함`

<br/>

### - webpack.config.js 파일 작성

- `test` : 로더를 적용할 파일 유형 (일반적으로 정규 표현식 사용)
- `use` : 해당 파일에 적용할 로더의 이름으로서 배열 안에 있는 `역순으로 로더들이 작동함`
- css-loader를 통해 변환된 파일을 style-loader를 통해 DOM에 포함시키기에 배열에서 먼저 작동되어야하는 `css-loader를 style-loader보다 뒤에 작성하기`

```js
// webpack.config.js

const path = require("path");

module.exports = {
  mode: "development",
  entry: path.resolve(__dirname, "src/index.js"),
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "main.js",
  },

  // styling 로더들을 위한 설정
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        // 역순으로 로더가 작동함
        use: [
          // HTML DOM의 스타일 태그에 담기
          "style-loader",
          // CSS를 CommonJS로 변환
          "css-loader",
          // Sass를 CSS로 컴파일
          "sass-loader"],
      },
    ],
  },
};

```

<br/>

### - styles 폴더 및 scss 파일 생성

- src/styles/main.scss 파일 생성

```scss
//src/styles/main.scss

body {
  padding: 0;
}
```

<br/>

### - 생성한 scss 파일 index.js로 import

- src/index.js로 scss 파일 가져오기

```js
// src/index.js

// ...
import "./styles/main.scss";
```

<br/>

### - 빌드하기

- 적절한 로더 패키지를 설치하지 않고, webpack.config.js 파일 설정을 하지 않고 빌드 실행하면 에러 발생함

```bash
$ npm run build
```