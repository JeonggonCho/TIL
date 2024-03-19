# Import 기능

## 목차

1. [Import](#1-import)
2. [Import 기능 만들기](#2-import-기능-만들기)
    1. [import 시도](#2-1-import-시도)
    2. [Webpack 설치](#2-2-webpack-설치)
        - [npm 초기화](#--npm-초기화)
        - [Webpack 패키지 설치](#--webpack-패키지-설치)
        - [scripts 추가](#--scripts-추가)
        - [build 스크립트 이용](#--build-스크립트-이용)
        - [빌드 완료](#--빌드-완료)
        - [index.html에 main.js 파일 연결](#--indexhtml에-mainjs-파일-연결)
        - [이미 만들어진 라이브러리 import 해보기](#--이미-만들어진-라이브러리-import-해보기)

<br/>
<br/>

## 1. Import

- `React`에서 App을 만들 경우, 모듈 및 함수, 패키지, 변수 값들을 간단하게 `import를 통해 가져와서 사용하였음`
- 이러한 Import 기능을 사용할 수 있는 이유는 `웹팩에 이미 설정이 다 되어있기에 가능함`

<br/>

## 2. Import 기능 만들기

### 2-1. import 시도

- src/randomAddress.js 파일 생성하여 export하고 index.js로 import하기

<br/>

```js
// src/randomAddress.js

function getRandomAddress() {
  return "서울";
}

export default getRandomAddress;
```

```js
// src/index.js

import getRandomAddress from "./randomAddress";

console.log(getRandomAddress());
```

- 아직 설정이 안 되어있기에 `SyntaxError: Cannot use import statement...` 발생함

<br/>

### 2-2. Webpack 설치

### - npm 초기화

- package.json 파일 생성

```bash
$ npm init -y
```

<br/>

### - Webpack 패키지 설치

- `webpack`과 `webpack-cli` 설치
- `-D` 플래그 : devDependencies로 설치하게 되어 개발환경에서만 작동함

```bash
$ npm i -D webpack webpack-cli
```

- package.json에서 의존성 패키지 설치 확인

```json
//package.json

{
  "devDependencies": {
    "webpack": "^5.90.3",
    "webpack-cli": "^5.1.4"
  }
}
```

<br/>

### - scripts 추가

- build를 위한 스크립트를 추가하기
- `"build": "webpack --mode production"` : src 폴더의 파일들을 빌드하여 dist로 넣어주는 스크립트

```json
//package.json

{
  "scripts": {
    "build": "webpack --mode production"
  }
}
```

<br/>

### - build 스크립트 이용

- `npm run build` 명령어 실행

```bash
$ npm run build
```

<br/>

### - 빌드 완료

- 자동으로 dist 폴더에 `main.js` 파일이 생성됨

```js
// dist/main.js

(() => {
  "use strict";
  console.log("서울")
})();
```

<br/>

### - index.html에 main.js 파일 연결

- 기존의 index.js 파일이 아닌 main.js로 교체
- 이제 에러 발생하지 않고 콘솔 잘 출력됨

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
<script src="./main.js"></script>
</body>
</html>
```

<br/>

### - 이미 만들어진 라이브러리 import 해보기

- nanoid라는 라이브러리 설치해서 사용해보기

```bash
$ npm install nanoid --save
```

```json
//package.json

{
  "dependencies": {
    "nanoid": "^5.0.6"
  }
}
```

- nanoid 라이브러리 index.js 파일로 import하기

```js
// index.js

// ...
import {nanoid} from "nanoid";

console.log(nanoid());
```

- 다시 npm run build로 빌드하기

```js
// dist/main.js

(() => {
  "use strict";
  console.log(((o = 21) => {
    let e = "", r = crypto.getRandomValues(new Uint8Array(o));
    for (; o--;) e += "useandom-26T198340PX75pxJACKVERYMINDBUSHWOLF_GQZbfghjklqvwyzrict"[63 & r[o]];
    return e
  })()), console.log("서울")
})();
```

- 콘솔에 nanoid가 생성한 유니크 id가 잘 출력됨을 확인할 수 있음