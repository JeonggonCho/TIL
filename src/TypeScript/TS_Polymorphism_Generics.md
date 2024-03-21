# 다형성 - Generics

## 목차

1. [다형성(Polymorphism)](#1-다형성polymorphism)
    1. [Concrete Type](#1-1-concrete-type)
    2. [Generics](#1-2-generics)
        - [Generics 사용](#--generics-사용)
        - [any 타입과의 차이](#--any-타입과의-차이)
        - [여러 개의 Generics 사용하기](#--여러-개의-generics-사용하기)
        - [Generics 활용 사례](#--generics-활용-사례)

<br/>
<br/>

## 1. 다형성(Polymorphism)

- 여러가지 다른 구조, 모양을 의미

```tsx
type SuperPrint = {
    (arr: number[]): void
    (arr: boolean[]): void
};

const superPrint: SuperPrint = (arr) => {
    arr.forEach(i => console.log(i));
}

superPrint([1, 2, 3, 4]);
superPrint([true, false, true, true]);
// 위의 호출은 문제없음 SuperPrint 시그니처에서 arr가 숫자 배열 또는 boolean 배열일 수 있기 때문

// 하지만, 문자열 배열을 파라미터로 호출하면 에러 발생
superPrint(["a", "b", "c"]);

// 위의 에러 상황에 대해 SuperPrint 시그니쳐에
// (arr: string[]): void를 추가하는 것이 좋은 해결방법일까?

superPrint([1, 2, true, false]);
// 이 경우는?

superPrint([1, 2, true, "a"]);
// 이 경우는?
```

- 위의 에러 발생 시, 시그니쳐에 파라미터 타입을 추가하는 것이 아닌 다형성의 Generics를 이용할 수 있음

<br/>

### 1-1. Concrete Type

- number, string, boolean, unknown, void 등과 같이 확실히 지정된 타입

<br/>

### 1-2. Generics

- Generics는 타입의 `placeholder` 같은 것
- Generics는 call signatures를 작성 시, 들어올 `확실한 타입을 모를 경우`, 사용
- Generics를 사용하면, 타입스크립트가 들어온 `값을 통해 타입을 유추`하게 됨

<br/>

### - Generics 사용

1. 타입스크립트에 Generics를 사용하고 싶음을 알려주어야 함

```tsx
type SuperPrint = {
    // <T>(arr: number[]): void
    // <V>(arr: number[]): void
    <TypePlaceholder>(arr: number[]): void
};
```

- 호출 시그니쳐 파라미터 앞에 `<(Generics 이름)>`을 붙임
  - Generics 이름은 주로 `T`, `V` 등의 알파벳을 사용하나, 아무 명칭을 사용해도 됨

<br/>

2. 파라미터의 타입도 Generics의 이름으로 바꾸어야함

```tsx
type SuperPrint = {
    <TypePlaceholder>(arr: TypePlaceholder[]): void
};
```

<br/>

3. 함수가 파라미터를 리턴을 한다면 함수의 타입도 Generics의 이름으로 변경

```tsx
type SuperPrint = {
    <TypePlaceholder>(arr: TypePlaceholder[]): TypePlaceholder
};
```

<br/>

### - any 타입과의 차이

- 타입을 값에 따라서 유추하는 것을 보고 any와 차이가 무엇인가에 대한 의문이 생길 수 있음
- any의 경우, 최소한의 타입도 지정이 안 되기에 아래와 같은 상황에도 에러를 반환하지 않게 됨

```tsx
// Generics 대신 any 사용할 경우,

type SuperPrint = {
    (a: any[]) : any
};

const superPrint: SuperPrint = (a) => a[0];

const a = superPrint([1, 2, true, "hello"]);

// 배열의 첫번째 요소인 숫자타입 1에 toUpperCase() 메서드를 사용해도 에러 발생 안 함
a.toUpperCase();
```

- 즉, Generics를 사용하면 해당 값에 따라 타입을 `맞춰`주기 때문에 알맞은 메서드, 연산을 할 수 있음 (타입을 보장, 보호 받을 수 있음)

<br/>

### - 여러 개의 Generics 사용하기

```tsx
type SuperPrint = {
    <T, V>(a: T[], b: V) : T
}

const superPrint: SuperPrint = (a) => a[0];

const a = superPrint([1, 2, true, "hello"], "");
```

- 타입스크립트는 Generics를 처음 인식했을 경우, Generics의 순서를 기반으로 Generics의 타입을 인식함

<br/>

### - Generics 활용 사례

- 다양한 코드 확장이 가능함
- 타입을 생성하고 그 타입을 다른 타입에 넣어서 사용하는 등의 활용이 가능

```tsx
// 함수 타입으로 사용

function superPrint<T>(a: T[]) {
    return a[0]
}

// -----------------------------------------

type Player<E> = {
    name: string
    extraInfo: E
}

const jeonggon: Player<{favFood: string}> = {
    name: "jeonggon",
    extraInfo: {
        favFood: "rice"
    }
}

// -------------------------------------------

type Player<E> = {
    name: string
    extraInfo: E
}

type JeonggonPlayer = Player<{favFood: string}>

const jeonggon: JeonggonPlayer = {
    name: "jeonggon",
    extraInfo: {
        favFood: "rice"
    }
}

// ---------------------------------------------

type Player<E> = {
    name: string
    extraInfo: E
}

type JeonggonExtra = {
    favFood: string
}

type JeonggonPlayer = Player<JeonggonExtra>

const jeonggon: JeonggonPlayer = {
    name: "jeonggon",
    extraInfo: {
        favFood: "rice"
    }
}
```