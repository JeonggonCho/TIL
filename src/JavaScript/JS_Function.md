# 함수

## 목차

1. [Function](#1-function)
    1. [함수의 구조](#1-1-함수의-구조)
2. [함수의 정의](#2-함수의-정의)
    1. [선언식(function declaration)](#2-1-선언식function-declaration)
    2. [표현식(function expression)](#2-2-표현식function-expression)
        - [함수 선언식과 표현식의 호이스팅](#함수-선언식과-표현식의-호이스팅)
        - [기본 함수 매개변수(Default function parameter)](#기본-함수-매개변수default-function-parameter)
        - [매개변수와 인자의 개수 불일치](#매개변수와-인자의-개수-불일치)
        - [나머지 매개변수(Rest parameters)](#나머지-매개변수rest-parameters)
    3. [화살표 함수 표현식(Arrow function expressions)](#2-3-화살표-함수-표현식arrow-function-expressions)
        - [화살표 함수 표현식의 작성순서](#화살표-함수-표현식의-작성순서)
    4. [콜백함수](#2-4-콜백함수)
3. [참고](#3-참고)
    1. [화살표 함수 표현식 응용](#3-1-화살표-함수-표현식-응용)

<br>
<br>

## 1. Function

-   참조 자료형(Reference type)에 속하며 모든 함수는 Function object

<br>

### 1-1. 함수의 구조

-   함수의 이름
-   함수의 매개변수
-   함수의 body를 구성하는 statement

```javascript
function name ([param[, param, [..., param]]]) {
    statements;
    return value;
}
```

-   return이 없다면 undefined를 반환

<br>
<br>

## 2. 함수의 정의

### 2-1. 선언식(function declaration)

```javascript
// 함수 선언식 구조
function funcName() {
    statements;
}

// 함수 선언식 예시
function add(num1, num2) {
    return num1 + num2;
}

console.log(add(2, 7));

// 출력
// 9
```

<br>

### 2-2. 표현식(function expression)

-   함수 이름이 없는 `익명 함수`를 사용할 수 있음
-   선언식과 달리 `표현식`으로 정의한 함수는 `호이스팅이 되지 않으므로` 코드에서 나타나기 전에 먼저 사용할 수 없음

```javascript
// 함수 표현식 구조
const funcName = function () {
    statements;
};

// 함수 표현식 예시
const sub = function (num1, num2) {
    return num1 - num2;
};

console.log(sub(7, 2));

// 출력
// 5
```

<br>

### - 함수 선언식과 표현식의 호이스팅

```javascript
// 호이스팅 비교

// 선언식
console.log(add(2, 7)); // 9

function add(num1, num2) {
    return num1 + num2;
}

// 표현식
console.log(sub(7, 2)); // ReferenceError: Cannot access 'sub' before initialization

const sub = function (num1, num2) {
    return num1 - num2;
};
```

|      | 선언식                                 | 표현식                                    |
| ---- | -------------------------------------- | ----------------------------------------- |
| 특징 | - 익명 함수 불가능<br/>- 호이스팅 있음 | - 익명 함수 사용 가능<br/>- 호이스팅 없음 |
| 비고 |                                        | - 사용 권장                               |

<br>

### - 기본 함수 매개변수(Default function parameter)

-   값이 없거나 undefined가 전달될 경우, 이름 붙은 `매개변수`를 `기본 값`으로 초기화

```javascript
const greeting = function (name = "Anonymous") {
    return `Hi ${name}`;
};

console.log(greeting());

// 출력
// 'Hi Anonymous'
```

<br>

### - 매개변수와 인자의 개수 불일치

-   `매개변수의 개수 < 인자의 개수`인 경우

```javascript
const noArgs = function () {
    return 0;
};

console.log(noArgs(1, 2, 3));

// 출력
// 0

const twoArgs = function (param1, param2) {
    return [param1, param2];
};

console.log(twoArgs(1, 2, 3));

// 출력
// [1, 2]
```

-   `매개변수의 개수 > 인자의 개수`인 경우

```javascript
const threeArgs = function (param1, param2, param3) {
    return [param1, param2, param3];
};

console.log(threeArgs()); // [undefined, undefined, undefined]
console.log(threeArgs(1)); // [1, undefined, undefined]
console.log(threeArgs(2, 3)); // [2, 3, undefined]
```

<br>

### - 나머지 매개변수(Rest parameters)

-   무한한 수의 인자를 배열로 허용하여 가변 함수를 나타내는 방법

```javascript
const myFunc = function (param1, param2, ...restPrams) {
    return [param1, param2, restPrams];
};

console.log(myFunc(1, 2, 3, 4, 5)); // [1, 2, [3, 4, 5]]
console.log(myFunc(1, 2)); // [1, 2, []]
```

-   함수 정의에는 `하나`의 나머지 매개변수만 있을 수 있음
-   나머지 매개변수는 함수 정의에서 `마지막 매개변수`여야 함

<br>

### 2-3. 화살표 함수 표현식(Arrow function expressions)

-   함수 표현식의 간결한 표현법

<br>

### - 화살표 함수 표현식의 작성순서

1. `function 키워드 제거` 후 매개변수와 중괄호 사이에 `화살표(=>)` 작성
2. 함수의 매개변수가 하나뿐이면 매개변수의 '()' 제거 가능
3. 함수 본문의 표현식이 한 줄이라면 '{}'와 'return' 제거 가능

```javascript
// 화살표 함수 표현식 예시

const arrow1 = function (name) {
    return `hello, ${name}`;
};

// 1. function 키워드 삭제 후, 화살표 작성
const arrow2 = (name) => {
    return `hello, ${name}`;
};

// 2. 인자가 1개일 경우에만, () 생략 가능
const arrow3 = (name) => {
    return `hello, ${name}`;
};

// 3. 함수 바디가 return을 포함한 표현식 1개일 경우에 {} & return 삭제 가능
const arrow4 = (name) => `hello, ${name}`;
```

<br>

### 2-4. 콜백함수

-   어떤 다른 함수의 `매개변수`로 `함수`를 넘겨주는 것을 의미함

```javascript
// 일반 함수 사용

function checkMood(mood) {
    if (mood === "good") {
        sing();
    } else {
        cry();
    }
}

function cry() {
    console.log("Action: CRY");
}

function sing() {
    console.log("Action: SING");
}

checkMood("good"); // 출력 : "Action: SING"

// 콜백함수 사용 예시
// checkMood 함수의 2번째, 3번째 매개변수(값)로 sing, cry 콜백함수를 전달 - 함수 표현식

function checkMood(mood, goodCallback, badCallback) {
    if (mood === "good") {
        goodCallback();
    } else {
        badCallback();
    }
}

function cry() {
    console.log("Action: CRY");
}

function sing() {
    console.log("Action: SING");
}

checkMood("sad", sing, cry); // 출력 : "Action: CRY"
```

<br>
<br>

## 3. 참고

### 3-1. 화살표 함수 표현식 응용

```javascript
// 1. 인자가 없다면 () or _로 표시 가능
const noArg1 = () => "No args";
const noArg2 = (_) => "No args";

// 2-1. object를 return 한다면 return을 명시적으로 작성해야 함
const returnObject1 = () => {
    return { key: "value" };
};

// 2-2. return을 적지 않으려면, 소괄호롤 감싸야 함
const returnObject2 = () => {
    ({ key: "value" });
};
```
