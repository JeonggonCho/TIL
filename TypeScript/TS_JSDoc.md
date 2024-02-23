# JSDoc

## 목차

1. [TypeScript에서 JavaScript 사용](#1-typescript에서-javascript-사용)
    1. [allowJs](#1-1-allowjs)
2. [JSDoc](#2-jsdoc)
    1. [@ts-check](#2-1-ts-check)

<br/>
<br/>

## 1. TypeScript에서 JavaScript 사용

### 1-1. allowJs

- tsconfig.json 파일에서 allowJs 옵션을 추가한 뒤, true로 설정
- TypeScript가 JavaScript 파일 안에 들어와 함수 등의 코드를 불러오는 것이 가능해짐

```json
// tsconfig.json

...
"allowJs": true
...
```

```ts
// src/index.ts

// 기존 d.ts 정의 파일을 통한 모듈 가져오기가 아닌 ./myPackage.js 파일 가져오기 가능
// TypeScript가 모르기에 js파일을 토대로 타입을 유추함
import {init, exit} from "./myPackage";
```

<br/>
<br/>

## 2. JSDoc

- JSDoc은 간단하게 정의하면 `코멘트`로 이루어진 문법
- 코멘트를 TypeScript가 읽고 해당 부가 기능을 적용함 (코멘트이기에 브라우저에는 영향없음)

<br/>

### 2-1. @ts-check

- 이미 프로젝트를 JavaScript를 사용하여 많이 진행이 되어있을 경우, TypeScript로 전환하는 것이 어려울 수 있음
- 따라서 JavaScript 파일로 유지한 채, TypeScript의 보호를 적용하는 것이 필요할 수 있음
- 주석 처리된 `// @ts-check`를 해당 함수 상단에 작성하기
- TypeScript에게 JavaScript 파일을 확인하라고 알리는 것임
- 그 아래에 설명 및 파라미터, 리턴의 타입 작성

```javascript
// ts-check 기초 구성

// @ts-check
/**
 * introduction
 * @param {(type)} target
 * @returns (type)
 */
```

```javascript
// src/myPackage.js

// @ts-check
/**
 * Initializes the project
 * @param {object} config
 * @param {boolean} config.debug
 * @param {string} config.url
 * @returns boolean
 */
export function init(config) {
  return true;
}

/**
 * Exits the program
 * @param {number} code
 * @returns number
 */
export function exit(code) {
  return code + 1;
}
```

