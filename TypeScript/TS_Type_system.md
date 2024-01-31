# 타입 시스템

## 목차

1. [타입 시스템](#1-타입-시스템)
    1. [타입](#1-1-타입)
        - [unknown 타입](#--unknown-타입)
        - [void 타입](#--void-타입)
        - [never 타입](#--never-타입)
    2. [타입 지정](#1-2-타입-지정)
        - [배열](#--배열)
        - [객체](#--객체)
        - [alias(별칭)](#--alias별칭)
        - [튜플](#--튜플)
        - [enum](#--enum)
        - [함수](#--함수)
        - [선택적 타입(Optional type)](#--선택적-타입optional-type)
        - [readonly](#--readonly)
    3. [타입 추론](#1-3-타입-추론)

<br>
<br>

## 1. 타입 시스템

### 1-1. 타입

| 키워드          | 설명                                         |
|--------------|--------------------------------------------|
| any          | 모든 JS 값 (TypeScript에서 사용을 지양)              |
| number       | 정수형(integer) 또는 실수형(floating point number) |
| boolean      | 참(true) 또는 거짓(false)                       |
| string       | 텍스트(문자열)                                   |
| null         | JS null 리터럴                                |
| undefined    | JS undefined 리터럴                           |
| object types | 모든 클래스, 모듈, 인터페이스 등                        |
| unknown      | 받은 값의 타입이 무엇인지 모를 경우                       |
| void         | 반환 값이 없는 함수                                |
| never        | 함수가 절대 return 하지 않을 때                      |
| array        | 배열형, 컬렉션 타입                                |
| enum         | 열거형                                        |

-   `강력하게 형식화`(strongly typed)
    -   C# / Java와 같음
    -   TypeScript (static typing)
    -   JavaScript (dynamic typing)
-   `개발 시에만 존재`하는 타입 시스템
    -   컴파일된 후에는 일반적인 JavaScript 타입 시스템

<br>

### - unknown 타입

- 받은 값의 `타입이 무엇인지 모를 경우`, 사용
- TypeScript로부터 일종의 보호를 받음
- 어떤 작업을 수행하기위해서는 이 변수의 타입을 먼저 확인해야 함 (TypeScript가 확인작업을 강제함)

```typescript
let a: unknown;


// 에러 메시지 : a의 타입이 무엇인지 모르기에 실행할 수 없음
let b = a + 1;

// 타입 확인을 거친 후, 숫자이면 더하기 연산을 수행하기에 에러 안남
// if 블록(scope) 안에서는 a의 타입은 number
if (typeof a === 'number') {
    let b = a + 1;
}


// 에러 메시지 : a의 타입이 무엇인지 모르기에 실행할 수 없음
let c = a.toUpperCase();

// 타입 확인을 거친 후, 문자열이면 대문자 치환을 수행하기에 에러 안남
// if 블록(scope) 안에서는 a의 타입은 string
if (typeof a === 'string') {
    let c = a.toUpperCase();
}
```

<br>

### - void 타입

- 아무 것도 리턴하지 않는 함수의 타입
- void는 따로 지정해주지 않아도 됨

```typescript
function hello() {
    console.log('x');
}
```

<br>

### - never 타입

- 함수가 절대 return 하지 않을 때 발생
- ex) 함수에서 예외 발생 시

```typescript
// 에러 메시지 : 문자열 "x"를 리턴함
function hello(): never {
    return "x";
}


// 리턴없이 에러를 출력하기에 오류 발생하지 않음
function hello(): never {
    throw new Error('xxx');
}


function hello(name:string|number): never {
    if (typeof name === 'string') {
        name // 이 if 블록에서 name의 타입은 string
    } else if (typeof name === 'number') {
        name // 이 if 블록에서 name의 타입은 number
    } else {
        name // 이 if 블록에서 name의 타입은 never
    }
}
```

<br>

### 1-2. 타입 지정

- 기본적으로 변수 명 뒤에 `콜론(':')과 타입`을 명시하여 사용

```typescript
// Type Annotations

// 기존 JavaScript는 any type으로 변수에 아무 형식의 값 할당가능
let num = "string";

// 반면 TypeScript는 형식을 지정해야하며 지정한 형식의 값만 할당가능
let num: number = 1234;

// any 타입의 경우, 모든 형식의 값 저장 가능
let a: any = 1234;

// any로 설정되는 경우,
let anyData;

// |(or) 사용 가능
let b: number | string = 1234;
```

-   `let` : declare it
-   `num` : name it
-   `콜론(':')` : annotate it
-   `number` : type it
-   `1234` : set it
-   `|` : 타입을 '또는'을 사용하여 여러 개 지정 가능

<br>

### - 배열

-   `let arr:any[]` : 배열형
-   (...) 사용시, 배열형이 와야함
-   `arr.` : 모든 배열 관련 속성/메서드 제공
    -   arr[0] 식으로 인덱스로 접근 가능

```typescript
// string형 배열
let stringArr: string[];

// 또는

Array<string>;


// (...) params(가변 길이 매개변수) 사용
let ...email: string[];
```

<br>

### - 객체

- `let obj:{}` : 객체형

```typescript
// 객체 타입 지정

const player : {
    name: string,
    age?: number,
} = {
    name: "조정곤",
}

// 오류 메시지로 player.age가 undefined일 수도 있음을 알려줌
if (player.age < 10) {}

// 따라서 player.age가 존재하는지 명시해야함
if (player.age && player.age < 10) {}
```

- 객체의 각 키에 대한 타입을 지정
- 선택적 변수 사용하여 nullable로 받을 수도 있음

<br>

### - alias(별칭)

- `동일한 구조`의 객체가 많이 존재하는 경우, 비효율적이며 코드가 반복되어 복잡해질 수 있음

```typescript
// 여러 개의 객체가 반복
const playerJeonggon : {
    name: string,
    age?: number,
} = {
    name: "조정곤",
}

const playerChulsoo : {
    name: string,
    age?: number,
} = {
    name: "김철수",
    age: 20,
}

...

// -----------------------------------------------------

// 별칭 사용
type Name = string;
type Age = number;
type Player = {
    name: Name,
    age?: Age,
};

const jeonggon: Player = {
    name: "조정곤",
}

const chulsoo: Player = {
    name: "김철수",
    age: 20,
}
```

- `type + 별칭`의 구조
- 미리 공통된 타입을 지정하여 `재사용 가능`하게 함
- 별칭은 대문자로 시작
- 객체뿐만 아니라 모든 타입의 변수에 사용가능함

<br>

### - 튜플

- 배열로 구현
- 최소한의 길이를 가짐
- 특정 위치에 특정 타입이 있어야 함

```typescript
// 선수의 정보
// [이름(string), 나이(number), 챔피언유무(boolean)]
// 이 배열은 최소 3개의 인자를 가지며 첫번째는 문자열, 두번째는 숫자형, 세번째는 참/거짓 타입임

const player1: [string, number, boolean] = ["조정곤", 27, true]

// 에러 메시지 : 첫번째 요소는 문자열 타입이기에 숫자로 변경불가
player[0] = 1

// ------------------------------------------------------------------

// readonly 사용

const player2: readonly [string, number, boolean] = ["조정곤", 27, true]
// 모든 값 변경 불가능
```

<br>

### - enum

-   enum은 TypeScript에만 제공하는 특별한 타입
-   여러 값을 묶어서 사용할 수 있게 해줌

```typescript
// enum
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}

let dir: Direction = Direction.Up;
```

<br>

### - 함수

-   각각의 `인자들`의 타입 지정
-   `반환되는 값`의 타입 지정 (인자들 소괄호 뒤에 콜론(:) + 타입)

```typescript
// 함수 선언식

function setStudent(
    name: string,
    passed: boolean,
    mark: number,
    courses: string[],
    info: any,
    state: State): void {
    ...
}

// ----------------------------------------------------

// 화살표 함수

let setStudent = (name:string, ...): string => name;
```

<br>

### - 선택적 타입(Optional type)

-   함수 선언 시, `필수적으로 받지 않아도 되는` 파라미터가 있을 경우, `타입지정(:) 앞`에 `?`를 사용하여 옵션으로 지정 가능
-   이 경우에, 옵션으로 설정된 파라미터에 인자를 전달하지 않아도 함수가 정상 호출됨, 없을 경우 undefined

```typescript
// Optional type 예시

// age 파라미터는 있어도 되고 없어도됨 (null 가능(= nullable) 타입)
function user(
    name: stirng,
    age?: number): void {
    console.log(name);

    if (age) {
        console.log(age);
    }
}

user("Jeonggon");

// 출력
// 'Jeonggon'
```

<br>

### - readonly

- 해당 변수를 `읽기 전용`으로 고정하여 변수의 값을 변경하는 것이 불가능해짐 (불변성 immutable)
- 보호장치의 역할을 함

```typescript
type Name = string;
type Age = number;
type Player = {
    readonly name: Name,
    age?: Age,
};

const playerMaker = (name:string): Player => name;
const jeonggon = playMaker("jeonggon");

// 에러 메세지 : 이미 jeonggon 객체의 name은 jeonggon이며, read-only 값이므로 변경 불가
jeonggon.name = "chulsoo";

// -----------------------------------------------------

const numbers: readonly number[] = [1, 2, 3, 4];

// 에러 메세지 : 해당 배열은 readonly로 값을 추가할 수 없음
numbers.push(1);

// .filter(), .map()과 같은 기존 배열에 영향을 주지않는 메서드는 사용 가능
```

<br>

### 1-3. 타입 추론

- TypeScript는 타입을 지정하지 않더라도 타입을 추론해줌

```typescript
// 타입 추론 예시

let a = "hello"; // 타입을 명시하지 않았지만, 문자열을 할당함

a = "bye"; // 문제없음 : 같은 string 타입

a = 1; // 오류 메시지로 string이 아닌 것을 알려줌

// ------------------------------------------------------

let b = [1, 2, 3]; // 타입을 명시하지 않았지만 자동으로 숫자형 배열이란 것을 인식

b.push("1"); // 오류 메시지로 number[]에 string을 넣으려 한다는 것을 알려줌
```