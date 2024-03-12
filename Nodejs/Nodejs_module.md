# 모듈

## 목차

1. [모듈](#1-모듈)
    1. [Common JS 모듈 시스템 (CJS)](#1-1-common-js-모듈-시스템-cjs)
        - [CJS 모듈의 특징](#cjs-모듈의-특징)
    2. [ES 모듈 시스템 (ESM)](#1-2-es-모듈-시스템-esm)
        - [ESM 사용법](#esm-사용법)
        - [ESM 특징](#esm-특징)

<br/>
<br/>

## 1. 모듈

- 모듈은 코드를 구성하고 `재사용 가능한` 단위로 분할하는데 사용되는 개념
- 모듈은 일반적으로 `하나의 파일`에 `정의`되며, 해당 파일 안에서 필요한 변수, 함수, 클래스 등을 내보내고(export) 가져올(import) 수 있다.
- JavaScript에서 사용하는 모듈 시스템은 대표적으로 `Common JS 모듈 시스템`과 `ES 모듈 시스템`이 있다.

<br>

### 1-1. Common JS 모듈 시스템 (CJS)

```javascript
// calc.js

const add = (a, b) => a + b;
const sub = (a, b) => a - b;

module.exports = {
  moduleName: "calc module",
  add: add,
  sub: sub,
};
```

```javascript
// index.js

const calc = require("./calc");

console.log(calc);

// 출력
/*
{
    moduleName: "calc module",
    add: [Function: add],
    sub: [Function: sub],
};
*/
```

- Node.js에서는 `module.exports`를 사용하여 `객체 단위`로 모듈을 내보내기 할 수 있음
- 내보낸 모듈은 다른 파일에서 `require('경로');`를 통해 가져올 수 있음
- 하지만, 이는 Node.js의 내장함수를 활용한 방법으로 Vanila JavaScript에선 사용이 제한 될 수 있음

<br>

### - CJS 모듈의 특징

- Node.js 초기부터 지원
- require()은 즉시 스크립트를 실행하는 구조임
- top-level await이 불가능하므로 `동기적으로 작동`함
- 동기적으로 작동하므로 `Promise를 리턴하지 않고`, module.exports에 `설정된 값만을 리턴`함
- 가져오는 `순서에` 따라서 스크립트를 실행함

<br>

### 1-2. ES 모듈 시스템 (ESM)

- `import`로 모듈에 접근하고 `export`로 모듈을 내보냄
- `ECMA Script`에서 지원하는 방식임
- `Node 14`에서는 CJS, ESM이 공존하는데 동시에 사용하기 위해서는 별도의 처리가 필요함
- 모듈 시스템을 CJS(기본 값)에서 ESM으로 변경할 경우, JavaScript 일부 동작이 변경되어 `호환성에 문제`가 발생할 수 있음

<br>

### - ESM 사용법

1. `package.json`에 `"type": "module"`을 설정

```javascript
// package.json

{
  "type"
:
  "module",
}
```

<br>

2. 모듈에 접근할 때, import 사용

```javascript
// import 사용

import util from "utils";
import {add} from "utils";
import {add as add_func} from "utils";
```

<br>

3. 모듈 내보낼 때, export 사용

   <named exports (이름있는 내보내기) 사용>

    ```javascript
    // named exports 사용
    // calculator.js

    export const sum = (x, y) => x + y;
    export const multiply = (x, y) => x * y;
    ```

    - `named exports` : 이름을 지정하여 내보내기
    - named exports는 명명된 이름으로만 모듈을 불러올 수 있음
    - 여러 개의 함수, 변수, 또는 클래스 등을 동일한 모듈에서 내보내기 가능

    <br>

    ```javascript
    import { sum, multiply } from "./calculator";
    import { sum as sum_func } from "./calculator.js"; // 다른 별칭으로 수정

    console.log(sum(2, 4)); // 출력: 6
    console.log(sum_func(2, 4)); // 출력: 6
    ```

    - 사용할 때 `중괄호({})를 사용`하여 각각의 항목을 명시적으로 가져와야 함
    - `as`를 사용해 명명된 이름을 다른 별칭으로 수정할 수 있음

    <br>
    <br>

   <default exports (기본 내보내기) 사용>

    ```javascript
    // default exports 사용
    // module.js

    export default (x, y) => x + y;

    // ----------------------------------------------------

    // module.js
    const subtract = (a, b) => a - b;
    export default subtract;
    ```

    - `default exports` : 별도의 이름을 저장하지 않고 내보냄
    - 모듈 당 `하나`의 기본 내보내기만 할 수 있음

    <br>

    ```javascript
    // index.js

    import sub from "./module";
    ```

    - 사용할 때 `중괄호({}) 없이` 가져와야 함
    - 이름이 없는 export이므로, import할 때 `내가 지정한 이름`을 사용 가능

    <br>
    <br>

   <혼합 사용>

    ```javascript
    // module.js

    const subtract = (a, b) => a - b;
    export { subtract as default, add, multiply };
    ```

    ```javascript
    // index.js

    import subtract, { add, multiply } from "./module";
    ```

    - 기본적으로 `subtract`가 `기본 내보내기`로 사용
    - `add`와 `multiply`는 `이름이 있는 내보내기`로 사용
    - 이러한 모듈 시스템은 코드를 모듈화하고 재사용성을 높이는 데 도움이 됨

<br>

### - ESM 특징

- top-level await을 지원하므로 module loader가 `비동기 환경에서 실행`됨
- 스크립트를 바로 실행하는 CJS와 달리, import / export 구문을 찾아 `스크립트를 파싱함`
- 파싱단계에서 import / export 에러를 감지 할 수 있음
- 모듈을 병렬(비동기)로 다운로드하지만 실행은 순차적(동기)으로 함
- import / export를 지원하지 않는 브라우저가 있기에 ESM 사용을 위해 번들러가 필요
