# TypeScript 개발환경 구축

## - 목차
1. [개발환경 구축](#1-개발환경-구축)
    - [기본적인 TypeScript 설치](#1-기본적인-typescript-설치)
    - [컴파일 및 실행](#2-컴파일-및-실행)

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