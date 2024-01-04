# JavaScript 기초 문법

## - 목차
1. [변수](#1-변수)
    - [식별자(변수명) 작성 규칙](#1-식별자변수명-작성-규칙)
    - [변수 선언 키워드](#2-변수-선언-키워드)
        - [let](#--let)
        - [const](#--const)
        - [블록 스코프(block scope)](#--블록-스코프block-scope)
        - [변수 선언 키워드 정리](#--변수-선언-키워드-정리)
2. [데이터 타입](#2-데이터-타입)
    - [원시 자료형(Primitive type)](#1-원시-자료형primitive-type)
        - [Number](#--number)
        - [String](#--string)
        - [null](#--null)
        - [undefined](#--undefined)
        - [null과 undefined](#--null과-undefined)
        - [Boolean](#--boolean)
    - [참조 자료형(Reference type)](#2-참조-자료형reference-type)
3. [연산자](#3-연산자)
    - [할당 연산자](#1-할당-연산자)
    - [비교 연산자](#2-비교-연산자)
    - [동등 연산자(==)](#3-동등-연산자)
    - [일치 연산자(===)](#4-일치-연산자)
    - [논리 연산자](#5-논리-연산자)
4. [조건문](#4-조건문)
    - [if](#1-if)
5. [반복문](#5-반복문)
    - [반복문 종류](#1-반복문-종류)
        - [while](#--while)
        - [for](#--for)
        - [for ... in](#--for--in)
        - [for ... of](#--for--of)
        - ['for ... in'과 'for ... of'](#--for--in과-for--of)
        - [반복문 정리](#--반복문-정리)
6. [참고](#6-참고)
    - [세미콜론(semicolon)](#1-세미콜론semicolon)
    - [var 키워드](#2-var-키워드)
    - [Template literals (템플릿 리터럴)](#3-template-literals-템플릿-리터럴)
    - [반복문 사용 시 const 및 let](#4-반복문-사용-시-const-및-let)
    - [NaN을 반환하는 경우](#5-nan을-반환하는-경우)

---

## (1) 변수

### **1) 식별자(변수명) 작성 규칙**

- 반드시 문자, 달러($), 언더바(_)로 시작
- 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작
- 예약어(키워드) 사용 불가
- 카멜 케이스(camelCase) 사용
  - 변수, 객체, 함수에 사용
- 파스칼 케이스(PascalCase) 사용
  - 클래스, 생성자에 사용
- 대문자 스네이크 케이스(SNAKE_CASE) 사용
  - 상수(constants)에 사용
  - 상수 : 개발자의 의도와 상관없이 변경될 가능성이 없는 값

<br>

### **2) 변수 선언 키워드**

1) `let`
2) `const`
3) `var`

- let, const는 ES6에서 추가됨

<br>

### - let

- 블록 스코프를 갖는 지역 변수를 선언
- 재할당 가능 & 재선언 불가능

```javascript
// 재할당 가능
let number = 10
number = 20

// 재선언 불가능
let number = 10
let number = 20
```

<br>

### - const

- 블록 스코프를 갖는 지역 변수를 선언
- 재할당 불가능 & 재선언 불가능

```javascript
// 재할당 불가능
const number = 10
number = 10

// 재선언 불가능
const number = 10
const number = 20

// 반드시 선언시 초기 값 설정 필요
const number
```

<br>

### - 블록 스코프(block scope)

- if, for, 함수 등의 `중괄호({}) 내부`를 가리킴
- 블록 스코프를 가지는 변수는 블록 바깥에서 접근 불가능

<br>

### - 변수 선언 키워드 정리

- 기본적으로 const 사용을 권장
- 재할당이 필요한 경우에만 let을 사용


---

## (2) 데이터 타입

### **1) 원시 자료형(Primitive type)**

- 변수에 값이 `직접` 저장되는 자료형
- `불변`이며 `값`이 복사됨
- ex) Number, String, Boolean, undefined, null

```javascript
const char = 'abc';
let a = 10;
let b = a;
b = 20;

console.log(char);
console.log(a);
console.log(b);

// 출력
// 'abc'
// 10
// 20
```

<br>

### - Number

- 정수 또는 실수형 숫자를 표현하는 자료형

```javascript
const a = 13;
const b = -5;
const c = 3.14; // float
const d = 2.998e8; // 2.998 * 10^8 = 299,800,000
const e = Infinity;
const f = -Infinity;
const g = NaN; // Not a Number를 나타내는 값
```

<br>

### - String

- 텍스트 데이터를 표현하는 자료형
- 곱셈, 나눗셈, 뺄셈은 안 되지만, `덧셈`을 이용하여 문자열끼리 `결합` 가능
- `Template Literal`을 사용하여 문자열 사이에 `변수 삽입` 가능 (backtick 사용)

```javascript
// ex1)
const firstName = 'Tony';
const lastName = 'Stark';
const fullName = firstName + lastName;

// ex2)
const age = 10;
const message = `홍길동은 ${age}세 입니다.`;
console.log(message);

// 출력
// '홍길동은 10세 입니다.'
```

<br>

### - null

- 변수의 `값이 없음`을 `의도적`으로 표현할 때, 사용

```javascript
const lastName = null;
console.log(lastName);

// 출력
// null
```

<br>

### - undefined

- 변수 선언 후, 직접 값을 할당하지 않으면 자동으로 할당됨

```javascript
let firstName;
console.log(firstName);

// 출력
// undefined
```

<br>

### - null과 undefined

- 동일한 역할을 하는 두 개의 키워드가 존재하는 이유는 단순한 `JavaScript 설계 실수`
- `null`이 `원시 자료형`임에도 불구하고 `object로 출력`되는 이유는 `JavaScript 설계 당시 버그를 해결 못한 것`
  - 이미 null타입에 의존하는 많은 프로그램들이 존재하여 `하위 호환을 유지`하기 위해 그대로 둠

```javascript
typeof null;
typeof undefined;

// 출력
// 'object'
// 'undefined'
```

<br>

### - Boolean

- true와 false
- 조건문 또는 반복문에서 boolean이 아닌 데이터 타입은 `자동 형변환 규칙`에 따라 true 또는 false로 변환됨

|  데이터 타입   |   false    |   true    |
|:---------:|:----------:|:---------:|
| undefined |  항상 false  |     X     |
|   null    |  항상 false  |     X     |
|  Number   | 0, -0, NaN | 나머지 모든 경우 |
|  String   |   빈 문자열    | 나머지 모든 경우 |

<ToBoolean Conversions(자동 형변환)>

```javascript
// 자동 형변환 예시
console.log(8 * null); // 0, null은 0
console.log('5' - 1); // 4 (빼기는 문자열을 숫자로 형변환 후 연산)
console.log('5' + 1); // '51' (더하기는 숫자를 문자열로 현변환 후 연산)
console.log('five' * 2); // NaN
```

<br>

### **2) 참조 자료형(Reference type)**

- 객체의 `주소`가 저장되는 자료형
- `가변`이며 `주소`가 복사됨
- ex) Objects(Object, Array, Function)

```javascript
const obj1 = {name: 'Jeonggon', age: 27};
const obj2 = obj1;
obj2.age = 40;

console.log(obj1.age);
console.log(obj2.age);

// 출력
// 40
// 40
```


---

## (3) 연산자

### **1) 할당 연산자**

- 오른쪽에 있는 피연산자의 평가 결과를 왼쪽의 피연산자에 `할당`하는 연산자
- 다양한 연산에 대한 `단축 연산자` 지원
- Increment(`++`)
  - 피연산자의 값을 1 증가시키는 연산자
- Decrement(`--`)
  - 피연산자의 값을 1 감소시키는 연산자
- `+=` 또는 `-=`와 같이 더 분명한 표현으로 적을 것을 권장

```javascript
let c = 0;

c += 10;
console.log(c); // 10

c -= 3;
console.log(c); // 7

c *= 10;
console.log(c); // 70

c++;
console.log(c); // 71

c--;
console.log(c); // 70
```

<br>

### **2) 비교 연산자**

- 피연산자들(숫자, 문자, Boolean 등)을 비교하고 결과 값을 Boolean으로 반환하는 연산자

```javascript
console.log(3 > 2); // true
console.log(3 < 2); // false

console.log('A' < 'B'); // true
console.log('Z' < 'a'); // true
console.log('가' < '나'); // true
```

<br>

### **3) 동등 연산자(==)**

- 두 피연산자가 같은 `값`으로 평가되는지 비교한 후, Boolean 값을 반환
- 비교할 때, `암묵적인 타입 변환`을 통해 타입을 일치시킨 후, 같은 값인지 비교
- 두 피연산자가 모두 객체일 경우, 메모리는 같은 객체를 바라보는지 판별
- 예상치 못한 결과가 발생할 수 있으므로 `특별한 경우를 제외하고 사용하지 않음`

```javascript
const a = 1;
const b = '1';
console.log(a == b); // true
console.log(a == true); // true
```

<br>

### **4) 일치 연산자(===)**

- 두 피연산자의 `값`과 `타입`이 모두 같은 경우, true를 반환
- 같은 객체를 가리키거나, 같은 타입이면서 같은 값인지를 비교
- 엄격한 비교가 이루어지며 암묵적 타입 변환이 발생하지 않음

```javascript
const a = 1;
const b = '1';

console.log(a === b); // false
console.log(a === Number(b)); // true
```

<br>

### **5) 논리 연산자**

- and 연산은 `&&` 연산자
- or 연산은 `||` 연산자
- not 연산은 `!` 연산자
- 단축 평가 지원
  - false && true => false
  - true || false => true

```javascript
console.log(true && true); // true
console.log(true && false); // false

console.log(false || true); // true
console.log(false || false); // false

console.log(!true); // false

console.log(1 && 0); // 0
console.log(0 && 1); // 0
console.log(4 && 7); // 7

console.log(1 || 0); // 1
console.log(0 || 1); // 1
console.log(4 || 7); // 4
```


---

## (4) 조건문

### **1) if**

- 조건 표현식의 결과 값을 Boolean 타입으로 변환 후, 참/거짓 판단

```javascript
// 기본 구조
if (조건문) {
    명령문;
} else if (조건문) {
    명령문;
} else {
    명령문;
}

// 예시
const name = 'manager';

if (name === 'admin') {
    console.log('관리자님 환영합니다.');
} else if (name === 'manager') {
    console.log('매니저님 환영합니다.');
} else {
    console.log(`${name}님 환영합니다.`);
}
```


---

## (5) 반복문

### **1) 반복문 종류**

- while
- for
- for ... in
- for ... of

<br>

### - while

- 조건문이 참이면 명령문을 계속 수행함

```javascript
while (조건문) {
    명령문;
}

// 예시
let i = 0;

while (i < 3) {
    console.log(i);
    i += 1;
}

// 출력
// 0
// 1
// 2
```

<br>

### - for

- 특정한 조건이 거짓으로 판별될 때까지 반복
- 반복문에 진입하여 초기문의 내용을 기반으로 변수 선언
- 조건문을 평가한 후, 코드 블럭의 명령문 수행
- 코드 블럭 수행 이후, 증감문 실행

```javascript
for ([초기문]; [조건문]; [증감문]) {
    명령문;
}

// 예시
for (let i = 0; i < 3; i++) {
    console.log(i);
}

// 출력
// 0
// 1
// 2
```

<br>

### - for ... in

- 객체(object)의 속성을 순회할 때 사용

```javascript
for (variable in object) {
    명령문;
}

// 예시
const fruits = {a: 'apple', b: 'banana'};

for (const key in fruits) {
    console.log(key);
    console.log(fruits[key]);
}

// 출력
// 'a'
// 'apple'
// 'b'
// 'banana'
```

<br>

### - for ... of

- 반복 가능한 객체(배열, 문자열)를 순회할 때 사용

```javascript
for (variable of iterable) {
    명령문;
}

// 예시
const numbers = [0, 1, 2, 3];

for (const number of numbers) {
    console.log(number);
}

// 출력
// 0
// 1
// 2
// 3
```

<br>

### - 'for ... in'과 'for ... of'

- for ... in은 객체의 속성에 대해 반복
- for ... of는 객체의 요소에 대해 반복

```javascript
const arr = [3, 5, 7];

// for ... in
for (const i in arr) {
    console.log(i);
}

// 출력
// 0
// 1
// 2


// for ... of
for (const i of arr) {
    console.log(i);
}

// 출력
// 3
// 5
// 7
```

- for ... in은 순서에 따라 `인덱스를 반환`하는 것을 `보장할 수 없기`에 인덱스 순서가 중요한 배열에서는 사용하지 않음

```javascript
// for ... in

// Array
const numbers = [10, 20, 30];

for (const number in numbers) {
    console.log(number); // 0 1 2
}

// Object
const capitals = {
    korea: '서울',
    france: '파리',
    japan: '도쿄'
}

for (const capital in capitals) {
    console.log(capital); // korea france japan
}
```

```javascript
// for ... of

// Array
const numbers = [10, 20, 30];

for (const number of numbers) {
    console.log(number); // 10 20 30
}

// Object
const capitals = {
    korea: '서울',
    france: '파리',
    japan: '도쿄'
}

for (const capital of capitals) {
    console.log(capital); // TypeError: capitals is not iterable
}
```

<br>

### - 반복문 정리

|    키워드     |     연관 키워드      |  스코프   |
|:----------:|:---------------:|:------:|
|   while    | break, continue | 블록 스코프 |
|    for     | break, continue | 블록 스코프 |
| for ... in |    object 순회    | 블록 스코프 |
| for ... of |   Iterable 순회   | 블록 스코프 |


---

## (6) 참고

### **1) 세미콜론(semicolon)**

- JavaScript에서 세미콜론(;)을 선택적으로 사용 가능
- 세미콜론이 없으면 ASI에 의해 자동으로 세미콜론 삽입됨
  - ASI (Automatic Semicolon Insertion, 자동 세미콜론 삽입 규칙)

<br>

### **2) var 키워드**

- 재할당 가능 & 재선언 가능
- ES6 이전에 변수 선언 시, 사용되던 키워드
- `호이스팅`되는 특성으로 예상치 못한 문제가 발생할 수 있음

  ```javascript
  // 호이스팅
  console.log(name); // undefined => 선언 이전에 참조했지만 에러 발생하지 않음
  
  var name = '홍길동';
  
  // 위 코드를 암묵적으로 아래와 같이 이해함
  var name; // undefined로 초기화
  console.log(name);
  
  var name = '홍길동';
  
  // -------------------------------------------
  
  // let과 const
  console.log(email); // Uncaught ReferenceError
  let email = 'abc123@gmail.com';
  
  console.log(age); // Uncaught ReferenceError
  const age = 50;
  ```
  - JavaScript에서 변수들은 실제 실행시, 코드 최상단으로 끌어올려지며(hoisted) 이러한 과정에서 var로 선언된 변수는 undefined로 초기화됨
  - 반면, let과 const는 호이스팅이 일어나면 에러를 발생시킴
  - 따라서 ES6 이후부터, `const`와 `let`을 사용하는 것을 권장
- 함수 스코프(function scope)를 가짐
  - 함수의 중괄호 내부를 가리킴
  - 함수 스코프를 가지는 변수는 함수 바깥에서 접근 불가능
  
  ```javascript
  // 함수 스코프 예시
  function foo() {
      var x = 5;
      console.log(x); // 5
  }
  
  console.log(x); // ReferenceError: x is not defined
  ```
- 변수 선언 시, var, let, const 키워드 중 하나를 쓰지 않으면 자동으로 var로 선언됨

<br>

### **3) Template literals (템플릿 리터럴)**

- 내장된 표현식을 허용하는 문자열 작성 방식
- ES6+ 부터 지원
- Backtick(``)을 이용하며, 여러 줄에 걸쳐 문자열을 정의할 수도 있고, JavaScript의 변수를 문자열 안에 바로 연결할 수 있는 장점이 생김
- 표현식은 $와 중괄호(${expression})로 표기

<br>

### **4) 반복문 사용 시 const 및 let**

- for문
  - for (let i = 0; i < arr.length; i++) {...}의 경우, 최초 정의한 i를 `재할당`하면서 사용하기 때문에 `const`사용시, `에러`발생


- for ... in, for ... of
  - 재할당이 아니라, 매 반복마다 다른 속성이름이 변수에 지정되는 것이므로 `const` 사용해도 `에러 발생 안 함`

<br>

### **5) NaN을 반환하는 경우**

- 숫자로서 읽을 수 없음
  - ex) Number(undefined)


- 결과가 허수인 수학 계산식
  - ex) Math.sqrt(-1)


- 피연산자가 NaN
  - ex) 7 ** NaN


- 정의할 수 없는 계산식
  - ex) 0 * Infinity


- 문자열을 포함하면서 덧셈이 아닌 계산식
  - ex) '가' / 3