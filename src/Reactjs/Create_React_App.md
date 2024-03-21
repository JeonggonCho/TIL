# React 앱 만들기

## 목차

1. [리액트 구성](#1-리액트-구성)
2. [리액트 시작하기](#2-리액트-시작하기)
    1. [src 폴더](#2-1-src-폴더)
    2. [HTML 파일 없이 어떻게 자바스크립트 파일만으로 화면을 출력할까?](#2-2-html-파일-없이-어떻게-자바스크립트-파일만으로-화면을-출력할까)
    3. [root는 실제로 어디 파일에 있는가?](#2-3-root는-실제로-어디-파일에-있는가)
    4. [node_modules 폴더](#2-4-node_modules-폴더)
    5. [public 폴더](#2-5-public-폴더)

<br>
<br>

## 1. 리액트 구성

-   리액트는 `Webpack`과 `Babel`로 구성되어있음

> -   Webpack : 다수의 자바스크립트 파일을 하나의 파일로 합쳐주는 모듈 번들 라이브러리
> -   Babel : JSX(자파스크립트와 HTML의 혼용 문법체계) 문법을 사용할 수 있도록 해주는 JSX 컴파일러 라이브러리

-   즉, 리액트는 이미 세팅이 되어있는 `패키지`

<br>
<br>

## 2. 리액트 시작하기

1. 프로젝트 폴더 만들기
2. `npx` 이용
    - npx는 npm을 편리하게 이용하기 위한 도구(처음 한 번)
    - `npx -v`로 버전이 출력되면 됨
3. npx가 설치되어 있지 않다면 `npm install -g npx` 터미널에 입력하여 설치
4. `npx create-react-app <프로젝트명>` 입력을 통해 리액트앱 설치 (3~5분 정도 소요)
    - create-react-app은 `노드(node)`기반의 패키지
    - 프로젝트명에 대문자 사용 불가능

<br>

![리액트_시작폴더](../assets/img/React_start1.jpeg)

<리액트 시작폴더>

<br>

-   노드 기반의 패키지이므로 다른 SPA 프레임워크(next.js, vue.js)와 마찬가지로 `package.json`과 `package-lock.json`을 가짐
-   성공적으로 설치 시, 터미널에서 "Happy hacking!" 문구를 볼 수 있음
-   node.js의 버전 문제로 설치가 실패할 수 있음
-   그 외에도 다양한 원인으로 설치에 실패할 수 있음

<br>

![리액트_package.json](../assets/img/React_package_json.png)

<리액트 package.json>

<br>

5. `'npm start'` 터미널 입력
    - "scripts"의 "start" : 리액트 어플리케이션 실행 스크립트

<br>

![리액트_시작페이지](../assets/img/React_start_page.jpeg)

<리액트 시작페이지>

<br>

-   브라우저 `요청` : localhost:3000 주소
    -   자신의 컴퓨터를 가리키는 주소
    -   본인 컴퓨터에서만 유효
-   화면 `응답`
    -   리액트 앱은 node.js 기반의 웹서버 위에서 동작

6. `'ctrl + c'` 로 웹서버 종료 가능

<br>

### 2-1. src 폴더

![리액트_App.js](../assets/img/React_app_js.png)

<리액트 App.js>

-   `App.js` 파일의 return 요소가 `브라우저 화면에 출력`됨

<br>

### 2-2. HTML 파일 없이 어떻게 자바스크립트 파일만으로 화면을 출력할까?

![리액트_앱리턴](../assets/img/React_app_js_return.png)

<브라우저 개발자 도구 코드와 App.js 리턴 코드 비교>

-   App() 함수 리턴 값이 `<div id='root'>` 태그 안에 `자식 요소`로 들어감

<br>

![리액트_index.js](../assets/img/React_index_js.png)

<리액트 index.js>

-   `index.js` 파일 역할 : 최상위 컴포넌트를 지정
-   기본적으로 최상위 컴포넌트는 App
-   코드의 흐름 : `index.js`에 App.js의 `App()`을 가져와서 `리액트 DOM`에 넣어 `root에 렌더링`

<br>

### 2-3. root는 실제로 어디 파일에 있는가?

![리액트_index.html](../assets/img/React_index_html.png)

<리액트 index.html>

-   root 태그는 `index.html`에 존재

<br>

### 2-4. node_modules 폴더

-   `외부 모듈`들을 담고 있는 폴더
-   모든 소스코드를 담고 있기에 용량이 매우 큼
-   지워져도 문제 없음
-   `'package.json'`과 `'package-lock.json'`에 정보가 명시되어 있어 `'npm i(= install)'`를 터미널에 입력하면 node_modules가 다시 설치됨

<br>

### 2-5. public 폴더

-   favicon.ico : 아이콘 파일
-   logo192.png, logo512.png, manifest.json : 모바일 관련
-   robots.txt : 구글, 네이버가 웹사이트를 수집하여 검색 시 띄워줄 때, 수집 허용 및 불가를 경로 기준으로 알려줌
