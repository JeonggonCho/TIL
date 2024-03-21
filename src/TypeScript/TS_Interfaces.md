# Interfaces

## 목차

1. [interface vs type](#1-interface-vs-type)
    1. [type](#1-1-type)
        - [object 형태에 type 사용](#--object-형태에-type-사용)
        - [type 변수 자체에 타입 지정](#--type-변수-자체에-타입-지정)
        - [alias](#--alias)
        - [concrete 타입의 특정 값이 되도록하기](#--concrete-타입의-특정-값이-되도록하기)
    2. [interface](#1-2-interface)
        - [interface 상속](#--interface-상속)
        - [type 상속](#--type-상속)
        - [readonly](#--readonly)
        - [property 축적](#--property-축적)
    3. [추상 클래스를 interface로 변환](#1-3-추상-클래스를-interface로-변환)
        - [기존의 추상 클래스](#--기존의-추상-클래스)
        - [interface로 변환](#--interface로-변환)
        - [JavaScript로 컴파일](#--javascript로-컴파일)
        - [클래스에 interface 여러 개 상속](#--클래스에-interface-여러-개-상속)
    4. [타입으로 interface 사용하기](#1-4-타입으로-interface-사용하기)

<br/>
<br/>

## 1. interface vs type

- interface는 type과 비슷하지만 차이점이 있음

<br/>

### 1-1. type

### - object 형태에 type 사용

```tsx
// 타입스크립트에게 object의 모양을 얄려줌
// property의 이름은 무엇인지, 타입이 무엇인지에 대한 정보 제공

type Player = {
    nickname: string,
    healthBar: number
};

const jeonggon: Player = {
    nickname: "jeonggon",
    healthBar: 10
};
```

<br/>

### - type 변수 자체에 타입 지정

```tsx
type Food = string;

const kimchi: Food = "delicious"
```

<br/>

### - alias

```tsx
type Nickname = string;
type Health = number;

type Player = {
    nickname: Nickname,
    healthBar: Health
};
```

<br/>

### - concrete 타입의 특정 값이 되도록하기

```tsx
type Team = "red" | "blue" | "yellow";
type Health = 1 | 5 | 10;

type Player = {
    nickname: string,
    team: Team,
    healthBar: Health
};

const jeonggon: Player = {
    nickname: "jeonggon",
    team: "red",
    healthBar: 5
}
```

<br/>

### 1-2. interface

- `type`의 경우, `원하는 모든 것`이 될 수 있음 (string 배열, 문자열, 숫자, object 모양 등...)
- `interface`는 오직 `object의 모양`을 특정하는 기능만 제공

```tsx
// object의 모양을 알려주는 방법

// 1. type
type Player = {
    nickname: string,
    team: Team,
    healthBar: Health
};

// 2. interface
interface Player {
    nickname: string,
    team: Team,
    healthBar: Health
};
```

```tsx
interface Hello = string; // 에러 발생 -> 오직 오브젝트 모양만 설명
```

<br/>

### - interface 상속

- `interface`는 object 모양 형성에만 사용되어 제한적이지만 `class와 비슷함` (좀 더 객체지향 프로그래밍으로 보임)
- `extends` 키워드 사용

```tsx
interface User {
    name: string
}

interface Player extends User {}

const jeonggon: Player = {
    name: "jeonggon"
};
```

<br/>

### - type 상속

- `&` 연산자 사용 

```tsx
type User = {
    name: string
}

type Player = User & {}

const jeonggon: Player = {
    name: "jeonggon"
};
```

<br/>

### - readonly

- type과 마찬가지로 interface에서도 `수정이 불가능`하도록 readonly 키워드 사용 가능

```tsx
interface User {
    readonly name: string
}

interface Player extends User {}

const jeonggon: Player = {
    name: "jeonggon"
};

jeonggon.name = "chulsoo"; // 에러 발생 -> name 속성은 readonly로 수정 불가능
```

<br/>

### - property 축적

- 동일한 이름의 interface를 만들면 해당 property들이 축적됨
- 각각의 interface를 TypeScript가 알아서 `하나로 합쳐줌`
- type에서는 지원하지 않음

```tsx
interface User {
    name: string
}
interface User {
    lastName: string
}
interface User {
    health: number
}

const jeonggon: User = {
    name: "jeonggon",
    lastName: "cho",
    health: 10
}
```

<br/>

### 1-3. 추상 클래스를 interface로 변환

### - 기존의 추상 클래스

```tsx
// 추상 클래스

abstract class User {
    constructor(
        protected firstName: string,
        protected lastName: string
    ) {}
    abstract sayHi(name: string): string
    abstract fullName(): string
}

class Player extends User {
    fullName() {
        return `${this.firstName} ${this.lastName}`;
    }

    sayHi(name: string) {
        return `Hello ${name}. My name is ${this.fullName()}`;
    }
}
```

<br/>

### - interface로 변환

- interface는 constructor 함수도 가지지 않고, abstract도 사용하지 않음
- object나 클래스의 모양을 묘사하도록 해 줌
- extends 대신 JavaScript에는 사용하지 않는 `implements` 사용
- implements : 클래스가 `interface를 상속`해야 함을 알려줌 (상속을 `강제함`)
- implements를 쓰면 `코드가 가벼워짐`
- interface를 상속할 때에는 property를 `private, protected로 만들지 못함`
- 상속받는 클래스에 `constructor 함수`를 작성해주어야 함

```tsx
// interface로 변환

interface User {
    firstName: string,
    lastName: string,
    sayHi(name: string): string,
    fullName(): string
}

class Player implements User {
    constructor(
        public firstName: string,
        public lastName: string
    ) {}
    fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
    sayHi(name: string) {
        return `Hello ${name}. My name is ${this.fullName()}`;
    }
}
```

<br/>

### - JavaScript로 컴파일

- 위의 interface를 사용한 코드를 JavaScript 파일로 컴파일
- interface를 사용하면 JavaScript에서는 보이지 않기에 `가벼워짐`

```javascript
// 컴파일

class Player {
    constructor(firstName, lastName) {
      this.firstName = firstName
      this.lastName = lastName
    }
    fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
    sayHi(name: string) {
        return `Hello ${name}. My name is ${this.fullName()}`;
    }
}
```

<br/>

### - 클래스에 interface 여러 개 상속

```tsx
// interface 여러 개 상속

interface User {
    firstName: string,
    lastName: string,
    sayHi(name: string): string,
    fullName(): string
}

// 추가로 Human interface 생성 
interface Human {
    health: number
}

// 클래스에 Human interface 상속 후, health property 추가
class Player implements User, Human {
    constructor(
        public firstName: string,
        public lastName: string,
        public health: number
    ) {}
    fullName() {
        return `${this.firstName} ${this.lastName}`;
    }
    sayHi(name: string) {
        return `Hello ${name}. My name is ${this.fullName()}`;
    }
}
```

<br/>

### 1-4. 타입으로 interface 사용하기

- interface도 type, class와 마찬가지로 타입으로 사용할 수 있음
- 대신 `interface`는 함수의 리턴 타입으로 사용될 때, `형식만 맞추면 됨`
- 하지만 `class`를 함수의 리턴 타입으로 사용할 때는 `new User()`와 같이 리턴해 주어야 함

```tsx
// interface를 타입으로 사용

interface User {
    firstName: string,
    lastName: string,
    sayHi(name: string): string,
    fullName(): string
}

// 함수의 parameter 타입으로 User interface 적용
function makeUser(user: User):string {
    return "hi";
}

makeUser({
    firstName: "jeonggon",
    lastName: "cho",
    fullName: () => "hi",
    sayHi: (name) => "hello"
})
```