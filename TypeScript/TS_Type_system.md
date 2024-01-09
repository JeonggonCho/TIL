# 타입 시스템

## - 목차
1. [타입 시스템](#1-타입-시스템)
    - [타입](#1-타입)
    - [타입 지정](#2-타입-지정)
        - [배열](#--배열)
        - [enum](#--enum)
        - [함수](#--함수)

---

## (1) 타입 시스템

### **1) 타입**

- Number -> `number`
- Boolean -> `boolean`
- String -> `string`
- Function -> `void`
  - JavaScript : `function(s, t) {};`
  - TypeScript : `functionName : (s: string, t: string) => void `
- Objecct -> `any`
- Array -> `any[]`
- ...

| 키워드          | 설명                                         |
|--------------|--------------------------------------------|
| any          | 모든 JS 값                                    |
| number       | 정수형(integer) 또는 실수형(floating point number) |
| boolean      | 참(true) 또는 거짓(false)                       |
| string       | 텍스트(문자열)                                   |
| null         | JS null 리터럴                                |
| undefined    | JS undefined 리터럴                           |
| object types | 모든 클래스, 모듈, 인터페이스 등                        |
| void         | 반환 값이 없는 함수                                |
| array        | 배열형, 컬렉션 타입                                |
| enum         | 열거형                                        |

- `강력하게 형식화`(strongly typed)
  - C# / Java와 같음
  - TypeScript (static typing)
  - JavaScript (dynamic typing)
- `개발 시에만 존재`하는 타입 시스템
  - 컴파일된 후에는 일반적인 JavaScript 타입 시스템

```typescript
// ex
// 타입스크립트
declare let document; document.title = "제목";

// 컴파일시 자바스크립트
document.title = "제목";
```

<br>

### **2) 타입 지정**

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
```

- `let` : declare it
- `num` : name it
- `콜론(:)` : annotate it
- `number` : type it
- `1234` : set it

<br>

### - 배열

- `let arr:any[]` : 배열형
- `arr.` : 모든 배열 관련 속성/메서드 제공
  - arr[0] 식으로 인덱스로 접근 가능

```typescript
// string형 배열
let stringArr: string[];

// 또는

Array<string>
```

<br>

### - enum
  - enum은 TypeScript에만 제공하는 특별한 타입
  - 여러 값을 묶어서 사용할 수 있게 해줌

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

- 각각의 인자들의 타입 지정
- 반환되는 값의 타입 지정

```typescript
function setStudent(
    name: string, passed: boolean, mark: number, courses: string[], info: any, state: State
) : void {
    ...
}
```