# 호출 시그니쳐

## 목차

1. [호출 시그니쳐(Call Signatures)](#1-호출-시그니쳐call-signatures)
    1. [호출 시그니쳐 생성](#1-1-호출-시그니쳐-생성)
    2. [호출 시그니쳐 사용](#1-2-호출-시그니쳐-사용)

<br/>
<br/>

## 1. 호출 시그니쳐(Call Signatures)

```tsx
const add = (a: number, b: number): number => a + b;
```

- 기존 함수에 위의 코드와 같이 타입을 지정하였음

<br/>

- 호출 시그니쳐는 `함수를 어떻게 호출해야하는 것`인지 알려줌
- 함수의 타입, parameter의 타입, 리턴 값의 타입 등을 알려줌

```
const add: (a: number, b: number) => number
```

<br/>

### 1-1. 호출 시그니쳐 생성

- `type` 키워드 사용
- 시그니쳐 이름을 짓고, parameter의 타입, 리턴 값의 타입 명시

```tsx
// 호출 시그니쳐 예시

type Add = (a: number, b: number) => number;

// ----------------------------------------

// 동일한 코드

type Add = {
    (a: number, b: number) : number
};
```

<br/>


### 1-2. 호출 시그니쳐 사용

- 앞서 선언한 호출 시그니쳐를 함수의 타입에 적용
- 이를 통해 타입 구조를 먼저 정의하고, 코드를 작성하도록 분리하여 관리 유도

```tsx
const add: Add = (a, b) => a + b;
```