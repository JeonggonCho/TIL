# 변수

## - 목록
1. [변수](#1-변수)
    - [namespace](#--namespace)

---

## (1) 변수

- 프로그램에서 사용할 데이터를 임시로 저장해 놓는 그릇
- 타입스크립트는 하나의 `프로젝트 단위`의 개념(전역 스코프)을 사용하기에 같은 프로젝트 안의 소스코드에서 `동일한 변수명`을 사용하여 재정의하는 경우, `에러 발생`
- 따라서 타입스크립트를 이용하면 이름 충돌을 방지 가능
- 만약, 이름을 특정 범위(scope) 내에서 사용하고 싶은 경우, `namespace(구 - 내부 모듈)`를 이용할 수 있음
  
```typescript
let num: number = 1234;
let name: string = "조정곤";
```

<br>

### - namespace

```typescript
//namespace 예시
let num: number = 1234;

// project1이라는 이름의 namespace 지정
namespace project1 {
    let num: number = 4567;
}
```

- 이전에 `내부 모듈`로 불림
- 동일한 소스코드 내에서 동일한 이름 사용으로 에러 발생하는 것을 피할 수 있음
- 하지만, 파일이 다르더라도 같은 이름의 namespace 안에서 동일한 이름의 변수를 사용하면 역시 에러가 발생
- 같은 네임스페이스에서는 같은 논리적 스코프를 가지기에 동시에 같은 변수명을 선언하면 중복이 되기 때문
- 반대로 변수명이 동일해도 namespace명이 다르다면 문제는 발생하지 않음
- 컴파일시, 반드시 `--outFile` 옵션을 사용하여 출력 js파일의 위치를 지정해주어야 함(그래야 namespace가 합쳐짐)

```shell
# --outFile 예시
> tsc ./namespace/app.ts --outFile dist/app.js
```

- 단점
  - 모듈 시스템과 큰 차이가 없음
  - 컴파일 옵션을 지정하지 않을 경우, `의존성(Dependency)들이 불러오지 않더라도` 오류메세지를 발생시키지 않고 정상적으로 컴파일하는 경우가 생김


---

참고출처 : [타입스크립트 모듈 & 네임스페이스 시스템 이해하기](https://inpa.tistory.com/entry/TS-%F0%9F%93%98-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%AA%A8%EB%93%88-%EB%84%A4%EC%9E%84%EC%8A%A4%ED%8E%98%EC%9D%B4%EC%8A%A4-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0#namespace_%EC%9D%98_%EB%8B%A8%EC%A0%90)

