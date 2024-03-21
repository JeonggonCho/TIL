# Lib & Declaration Files

## 목차

1. [Lib Configuration](#1-lib-configuration)
    1. [lib](#1-1-lib)
    2. [Declaration Files](#1-2-declaration-files)
        - [esModuleInterop](#--esmoduleinterop)
        - [strict](#--strict)
        - [.d.ts 파일](#--dts-파일)

<br/>
<br/>

## 1. Lib Configuration

- lib : 합쳐진 라이브러리의 `정의 파일`을 특정하기

<br>

### 1-1. lib

- TypeScript에 코드가 어디를 기반으로 동작할 것인지를 알려줌
- 서버용 JavaScript인 `es6`와 브라우저용 JavaScript인 `DOM` 등을 lib의 배열에 넣기
- 해당 lib을 작성함으로써 TypeScript는 어떤 `메서드, API, 타입`들을 사용할지 등을 IDE에서 추천해줌

```json
// tsconfig.json

{
  "include": [
    "src"
  ],
  "compilerOptions": {
    "outDir": "build",
    "target": "ES6",
    "lib": [
      "es6",
      "DOM"
    ]
  }
}
```

<br/>

### 1-2. Declaration Files

- TypeScript가 lib에 의해서 사용할 메서드, API, 타입을 추천해주는데 이를 가능하게 해주는 파일

<br/>

### - esModuleInterop

- `CommonJS 모듈`과 `ES 모듈 방식`을 `혼용`하여 사용할 수 있게 해주는 옵션

```json
// tsconfig.json

"esModuleInterop": true,
```

<br/>

### - strict

- TypeScript의 규칙을 강제함

```json
// tsconfig.json

"strict": true
```

<br/>

### - .d.ts 파일

- d.ts 파일은 `정의 파일`로서 JavaScript 코드의 구조, argument, 리턴, 타입을 TypeScript에게 설명해주는 파일

```javascript
// src/myPackage.js

// 코드 작성 후, 내보내기
export function init(config) {
  return true;
}

export function exit(code) {
  return code + 1;
}
```

```ts
// src/index.ts

import {exit, init} from "myPackage"; // 에러발생 -> TypeScript는 myPackage를 모름
```

- TypeScript에게 모듈을 인식할 수 있도록 `myPackage.d.ts` 파일 생성
- 해당 파일에서 `declare` 키워드를 사용하여 `myPackage` 모듈 인식시키고 해당 모듈의 함수구조, 리턴, 리턴 타입 정의하기

```ts
// src/myPackage.d.ts

interface Config {
    url: string;
}

declare module "myPackage" {
    function init(config: Config): boolean;

    function exit(code: number): number;
}
```

- d.ts 파일을 통해 TypeScript는 해당 모듈을 인식하고 에러를 발생시키지 않음