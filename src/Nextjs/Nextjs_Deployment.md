# Deployment

## 목차

1. [배포하기](#1-배포하기)
    1. [package.json 명령어 추가](#1-1-packagejson-명령어-추가)
        - [push 전에 프로젝트가 build 가능한지 확인하기](#--push-전에-프로젝트가-build-가능한지-확인하기)
    2. [push 하기](#1-2-push-하기)
    3. [Vercel에 접속](#1-3-vercel에-접속)

<br/>
<br/>

## 1. 배포하기

### 1-1. package.json 명령어 추가

```json
// package.json

{
  ...
  "main": "index.js",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "keywords": [],
  ...
}
```

1. `"build": "next build"` : 어플리케이션을 실제로 production 용으로 만들기
2. `"start": "next start"` : 어플리케이션을 production 모드로 시작해 줌

- 해당 명령어를 직접 호출하지는 않으며, `Vercel(클라우드 플랫폼 회사, Next.js 개발 및 프로젝트 배포 운영)`이 호출함

<br/>

### - push 전에 프로젝트가 build 가능한지 확인하기

- `npm run build` 실행
    - 에러 발생 요소를 미리 확인 할 수 있음

<br/>

### 1-2. push 하기

- 추가된 명령어와 함께 소스코드 GitHub에 push 하기

<br/>

### 1-3. Vercel에 접속

1. `Add New` -> `Project` 클릭

![add new](../assets/img/Nextjs_vercel_add_project.png)

2. 배포할 프로젝트의 레포지토리 선택

![select repo](../assets/img/Nextjs_vercel_import_repo.png)

3. 프로젝트 이름 작성, 루트 디렉토리('./')는 수정하면 안 됨

![setting](../assets/img/Nextjs_vercel_project_setting.png)

4. `Build and Output Settings`
    - package.json의 스크립트에서 작성한 `npm run build`와 `npm run start` 자동으로 실행 될 것임
5. API 환경변수 키 값이 있을 경우, 작성
6. `Deploy` 클릭
    - 에러가 발생하여 배포가 안 될 수 있음
    - 해당 문제를 해결하고 재배포 진행