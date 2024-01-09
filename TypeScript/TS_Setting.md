# TypeScript 개발환경 구축

## - 목차
1. [개발환경 구축](#1-개발환경-구축)
    - [기본적인 TypeScript 설치](#1-기본적인-typescript-설치)
    - [컴파일 및 실행](#2-컴파일-및-실행)
    - [타입스크립트 설정 옵션 파일 생성하기 - tsconfig.json](#3-타입스크립트-설정-옵션-파일-생성하기---tsconfigjson)

---

## (1) 개발환경 구축

### **1) 기본적인 TypeScript 설치**

1) `node.js` 설치
2) 터미널에서 `'npm install -g typescript'` 입력하여 TypeScript를 전역으로 설치하기
    - 맥에서 전역 설치 시, 권한 문제로 에러가 발생할 수 있음
    - 이럴 경우, `'sudo npm install -g typescript'`를 통해 관리자 권한으로 실행함
3) 설치가 완료된 경우, `'tsc --vesion'`을 통해 설치된 TypeScript의 버전을 확인할 수 있음
4) 이후, 파일 생성 시, `(파일명).ts`로 생성하면 TypeScript 파일이 생성됨

<br>

### **2) 컴파일 및 실행**

1) `'tsc (파일명).ts'`를 터미널에 입력하면 컴파일 실행
    - "tsc(TypeScript Compiler)야 (파일명).ts 파일을 컴파일 해라" 라고 명령
2) 컴파일이 완료되면 동일한 위치에 동일한 이름의 JavaScript 파일이 생성된 것을 확인할 수 있음
    - `(파일명).js` 파일 생성
3) `'node (파일명).js'` 명령으로 JavaScript 파일 실행 가능
    - "node야 (파일명).js 파일을 실행해라" 라고 명령
4) 컴파일과 파일 실행 명령어 한 줄로 동시에 하기위해서는 `'tsc (파일명).ts & node (파일명).js'`를 이용
    - '&'는 '그리고'를 의미하며 'A & B'를 하게되면 'A를 하고 B를 해' 라고 명령

<br>

### - 타입스크립트 파일의 저장과 동시에 js 파일로 컴파일하기

- `'tsc -w (파일명).ts'`를 설정하면 계속해서 ts 파일 코드를 `실시간 관찰(watch-mode)`하고 저장하면 변경사항을 컴파일된 js 파일에 반영함
- `ctrl + c`로 관찰자 모드 종료 가능

<br>

### **3) 타입스크립트 설정 옵션 파일 생성하기 - tsconfig.json**

1) `'tsc --init'`을 터미널에 입력
2) `tsconfig.json` 파일 생성됨
  - 해당 파일에는 컴파일 옵션이 지정되어 있음
  - 아래와 같이 어떤 형식으로 컴파일 할 지, 모듈은 어떤 방식을 이용할 지 등이 작성되어있음

```json
{
   "compilerOptions": {
      "target": "es2016", 
      "module": "commonjs"
   }
}
```

3) tsconfig.json 파일이 생성되면 특정한 파일을 지정하지 않고 `'tsc'`만 입력하면 현재 폴더의 모든 TypeScript 파일이 컴파일 됨
4) 관찰자 모드도 `'tsc -w'`로 동일하게 실행 시킬 수 있음
