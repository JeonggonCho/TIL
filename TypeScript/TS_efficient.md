# 효율적인 개발환경 구축

## 목차

1. [빌드 및 실행](#1-빌드-및-실행)
    1. [npm run start](#1-1-npm-run-start)
    2. [ts-node](#1-2-ts-node)
    3. [dev](#1-3-dev)
    4. [nodemon](#1-4-nodemon)
    5. [npm run dev](#1-5-npm-run-dev)

<br/>
<br/>

## 1. 빌드 및 실행

### 1-1. npm run start

- `build` : tsconfig.json에 작성된 include 폴더의 TypeScript 파일을 outDir 폴더로 `컴파일` 해주는 역할
- `start` : 컴파일된 build 폴더의 index.js를 `실행`하는 역할

```json
// package.json

...
"scripts": {
"build": "tsc",
"start": "node build/index.js"
},
...
```

- 이후, 아래와 같이 명령을 하게 되는데 빌드를 진행하고 실행시키는 것이 시간이 오래 소요될 수 있으며, 비효율적일 수 있음

```bash
# 빌드 및 실행 명령어

$ npm run build
$ npm run start

# 또는

$ npm run build && npm run start
```

<br/>

### 1-2. ts-node

- `빌드없이` TypeScript를 `실행`할 수 있도록 해주는 패키지
- 실제 프로덕션에서 사용하는 패키지는 아니며, 개발환경에서만 효율성을 위해 사용

```bash
# 패키지 설치

$ npm i -D ts-node
```

<br/>

### 1-3. dev

- dev 스크립트를 추가하기

```json
// package.json

...
"scripts": {
"build": "tsc",
"start": "node build/index.js",
"dev": "ts-node src/index.ts"
},
...
```

<br/>

### 1-4. nodemon

- 자동으로 커맨드를 재실행해줘서 일일히 커맨드를 다시 실행할 필요가 없어짐 (서버를 재시작할 필요없음)

```bash
# 패키지 설치

$ npm i nodemon
```

- nodemon을 활용한 dev 스크립트 수정

```json
// package.json

...
"scripts": {
"build": "tsc",
"start": "node build/index.js",
"dev": "nodemon --exec ts-node src/index.ts"
},
...
```

<br/>

### 1-5. npm run dev

```bash
$ npm run dev
```

- 해당 명령어 실행 시, 빌드없이 `바로 실행`할 수 있으며, index.ts 파일이 수정되면, 수정사항을 `바로 업데이트` 해 줌

