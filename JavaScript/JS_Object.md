# 객체

## - 목차
1. [Object](#1-object)
    - [객체의 구조](#1-객체의-구조)
2. [객체의 속성](#2-객체의-속성)
    - [Property 활용](#1-property-활용)
    - [Property 존재 여부 확인 (in)](#2-property-존재-여부-확인-in)
    - [단축 Property](#3-단축-property)
    - [계산된 Property](#4-계산된-property)
    - [비구조화 할당](#5-비구조화-할당)
3. [객체와 함수](#3-객체와-함수)
    - [Method](#1-method)
        - [Method 예시](#--method-예시)
        - [Method + this 예시](#--method--this-예시)
        - [this 키워드](#2-this-키워드)
            - [JavaScript에서 this는 함수를 '호출하는 방법'에 따라 가리키는 대상이 다름](#--javascript에서-this는-함수를-호출하는-방법에-따라-가리키는-대상이-다름)
            - [Nested 함수에서의 문제점과 해결책](#--nested-함수에서의-문제점과-해결책)
4. [참고](#4-참고)
    - [유용한 객체 메서드](#1-유용한-객체-메서드)
    - [JavaScript의 'this' 특징](#2-javascript의-this-특징)
    - [JSON(JavaScript Object Notation)](#3-jsonjavascript-object-notation)
        - [JSON <-> Object 변환하기](#--json---object-변환하기)

---

## (1) Object

- `키-값의 쌍`으로 구분된 데이터 집합(data collection)을 저장하는 자료형
- `키`는 따옴표가 없으나 `문자열`임
- 키가 중복 가능하나 추천하지 않음
  - 중복일 경우, 뒤의 요소를 우선시 함
- `값`은 자료형이 서로 달라도 상관없음
- 객체는 `생성자 방식`, `객체 리터럴 방식`으로 생성가능
  - 생성자 방식(new 키워드 사용) : let person = new Object();
  - 객체 리터럴 방식(중괄호 사용) : {};

<br>

### **1) 객체의 구조**

- `중괄호`를 이용해 생성
- 중괄호 안에는 `key:value`쌍으로 구성된 속성(property)를 여러 개 넣을 수 있음
- `key`는 `문자형`, `value`는 `모든 자료형`이 허용

```javascript
const user = {
    name: 'Jeonggon',
    age: 27,
    'key with space': true,
};
```

- 마지막 속성에 콤마(,) 붙이는 것을 `trailing comma`라고 하며 속성을 추가, 삭제, 이동하기에 용아하여 추가함


---

## (2) 객체의 속성

### **1) Property 활용**

```javascript
// 조회 - 점 표기법, 괄호 표기법
console.log(user.age); // 27
console.log(user['key with space']); // true


// 추가
user.address = 'korea';
console.log(user); // {name: 'Jeonggon', age: 27, key with space: true, address: 'korea'}


// 수정
user.age = 30;
console.log(user.age) // 30


// 삭제 - delete 사용, null 사용
// delete 단점 : 삭제하더라도 프로퍼티와 객체의 연결을 끊을 뿐, 메모리에 프로퍼티가 남아있음
delete user.address;
console.log(user); // {name: 'Jeonggon', age: 27, key with space: true}


// null 사용 : 삭제한 효과를 내며, 메모리의 내역도 지워짐
user.age = null;
console.log(user); // {name: 'Jeonggon', key with space: true}
```

<br>

### **2) Property 존재 여부 확인 (in)**

```javascript
console.log('age in user'); // true
console.log('country' in user); // false
```

<br>

### **3) 단축 Property**

- 키 이름과 값으로 쓰이는 변수의 이름이 같은 경우, 단축 구문을 사용할 수 있음

```javascript
const age = 30;
const address = 'korea';

const oldUser = {
    age: age,
    address, address,
};

// 위의 객체를 함축하여 표현
const newUser = {
    age,
    address,
};
```

<br>

### **4) 계산된 Property**

- 키가 대괄호([])로 둘러싸여 있는 프로퍼티
- 고정된 값이 아닌 변수 값을 사용할 수 있음

```javascript
const product = prompt('물건 이름을 입력해주세요');
const prefix = 'my';
const suffix = 'property';

const bag = {
    [product]: 5,
    [prefix + suffix]: 'value',
}

console.log(bag); // {연필: 5, myproperty: 'value'}
```

<br>

### **5) 비구조화 할당**

```javascript
// ex)
let object = {one: "one", two: "two", three: "three"};

let one = object.one;
let two = object.two;
let three = object.three;
// 반복적인 코드를 사용하며 비효율적임


// 1) 비구조화 할당으로 간단하게 표현
let {one, two, three} = object;
// 키값을 기준으로 one이라는 키를 가지는 값을 one 변수에 저장하고
// two라는 키를 가지는 값을 two에 저장하고
// three라는 프로퍼티 값을 three에 저장
// 배열에서는 인덱스(순서)를 이용했다면, 객체는 키를 이용함
// 기본적으로 변수명이 키 이름으로 할당됨


// 2) 원하는 변수명으로 지정하는 방법
let {one:myOne, two:myTwo, three:myThree} = object;
// 콜론(:) 뒤에 원하는 변수명 지정


// 3) 존재하지 않는 키를 작성할 경우
let {one, two, three, four} = object;
// 존재하지 않는 키 이름의 four 변수에 undefined 할당됨


// 4) 기본값 설정
let {one, two, three, four="four"} = object;
// 기본 값을 설정하면 undefined가 할당되지 않도록 할 수 있음
```


---

## (3) 객체와 함수

### **1) Method**

- `객체 속성`에 정의된 `함수`

<br>

### - Method 예시

- object.method() : 메서드는 객체를 `행동`할 수 있게 함

```javascript
const person = {
    name: 'Sophia',
    greeting: function () {
        return 'Hello';
    },
};

// greeting 메서드 호출
console.log(person.greeting());

// 출력
// 'Hello'
```

- `this` 키워드를 사용해 객체에 대한 특정한 작업을 수행할 수 있음

<br>

### - Method + this 예시

- 메서드가 멤버를 참조할 경우, 자기자신이 속한 객체를 참조함

```javascript
const person = {
    name: 'Sophia',
    greeting: function () {
        return `Hello my name is ${this.name}`;
    }
};

console.log(person.greeting());

// 출력
// 'Hello my name is Sophia'
```

<br>

### **2) this 키워드**

- 함수나 메서드를 `호출한 객체`를 가리키는 키워드
- 함수 내에서 객체의 속성 및 메서드에 접근하기 위해 사용

<br>

### - JavaScript에서 this는 함수를 '호출하는 방법'에 따라 가리키는 대상이 다름

- 단순 호출 시 -> 전역 객체
- 메서드 호출 시 -> 메서드를 호출한 객체

```javascript
// 단순 호출 시 this
const myFunc = function () {
    return this;
};

console.log(myFunc()) // window


// 메서드 호출시 this
const myObj = {
    data: 1,
    myFunc: function () {
        return this;
    }
}

console.log(myObj.myFunc()) // myObj
```

<br>

### - Nested 함수에서의 문제점과 해결책

```javascript
// 문제점

const myObj2 = {
    numbers: [1, 2, 3],
    myFunc: function () {
        this.numbers.forEach(function (number) {
            console.log(number); // 1 2 3
            console.log(this); // window
        })
    }
}
```

- forEach의 인자로 들어간 함수는 `일반 함수 호출`이기 때문에 `this`가 `전역 객체`를 가리킴

```javascript
// 해결책 - 화살표 함수

const myObj3 = {
    numbers: [1, 2, 3],
    myFunc: function () {
        this.numbers.forEach((number) => {
            console.log(number); // 1 2 3
            console.log(this); // myObj3
        })
    }
}
```

- `화살표 함수`는 자신만의 this를 가지지 않기 때문에 외부 함수에서 this를 가져옴


---

## (4) 참고

### **1) 유용한 객체 메서드**

- `Object.keys('객체명')` : 객체의 키들을 모아서 배열로 반환
- `Object.values('객체명'` : 객체의 값들을 모아서 배열로 반환

```javascript
const profile = {
    name: 'Sophia',
    age: 30,
}

console.log(Object.keys(profile)); 

// 출력
// ['name', 'age']

console.log(Object.values(profile));

// 출력
// ['Sophia', 30]
```

<br>

### **2) JavaScript의 'this' 특징**

- JavaScript에서 this는 함수가 `호출되는 방식`에 따라 결정되는 현재 객체를 나타냄
- `Python의 self`와 `Java의 this`는 선언 시, `값이 이미 정해지는 것`에 비해 `JavaScript의 this`는 함수가 호출되기 전까지 값이 할당되지 않고 `호출 시 결정`됨(동적)

<br>

### **3) JSON(JavaScript Object Notation)**

- `Key-Value` 형태로 이루어진 자료 표기법
- JavaScript의 Object와 유사한 구조를 가지고 있지만 JSON은 형식이 있는 `문자열`
- JavaScript에서 JSON을 사용하기 위해서는 Object 자료형으로 변환해야 함

<br>

### - JSON <-> Object 변환하기

```javascript
const jsObject = {
    coffee: 'Americano',
    iceCream: 'Cookie and cream',
};


// Object -> JSON
const objToJson = JSON.stringify(jsObject);

console.log(objToJson); // {"coffee":"Americano","iceCream":"Cookie and cream"}
console.log(typeof objToJson); // string


// JSON -> Object
const jsonToObj = JSON.parse(objToJson);

console.log(jsonToObj); // { coffee: 'Americano', iceCream: 'Cookie and cream' }
console.log(typeof jsonToObj); // object
```