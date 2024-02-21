# 클래스

## 목차

1. [TypeScript 클래스](#1-typescript-클래스)
    1. [JavaScript 클래스와 TypeScript 클래스 비교](#1-1-javascript-클래스와-typescript-클래스-비교)
        - [TypeScript 접근 제한자(Access modifier)](#--typescript-접근-제한자access-modifier)
    2. [추상 클래스 (Abstract Class)](#1-2-추상-클래스-abstract-class)
    3. [추상 클래스 안의 메소드 & 추상 메소드 (Abstract Method)](#1-3-추상-클래스-안의-메소드--추상-메소드-abstract-method)
        - [추상 클래스 안의 메소드](#--추상-클래스-안의-메소드)
        - [추상 메소드 (Abstract Method)](#--추상-메소드-abstract-method)

<br/>
<br/>

## 1. TypeScript 클래스

- TypeScript를 이용하여 안전하고 더 나은 객체 지향 프로그래밍 구현할 수 있도록 도와줌

<br/>

### 1-1. JavaScript 클래스와 TypeScript 클래스 비교

- `JavaScript`에서는 `constructor 함수`를 만들고 그 안에 `this.firstName = firstName`과 같이 코드를 작성하였음
- `TypeScript`에서는 위와 같이 작성을 하지 않고, `파라미터들만 작성`하면 TypeScript가 알아서 constructor 함수를 만들어 줌

```javascript
// 기존의 JavaScript 코드에서 constructor 함수 작성
class Player {
  constructor(firstName, lastName, nickname) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.nickname = nickname;
  }
}

const jeonggon = new Player("jeonggon", "cho", "Gon");

jeonggon.firstName; // 문제없음(보호기능 없음)
```

```tsx
// TypeScript에서는 파라미터 정의만으로 constructor 함수가 자동으로 처리됨
class Player {
    constructor(
        private firstName: string,
        private lastName: string,
        public nickname: string
    ) {}
}

const jeonggon = new Player("jeonggon", "cho", "Gon");

jeonggon.firstName // 에러 발생 -> private이기 때문
```

<br/>

### - TypeScript 접근 제한자(Access modifier)

- public, protected, private 등을 지원하며 이를 통해 외부에서 특정 메서드나 프로퍼티에 `접근 범위를 지정 가능`

1. `public` : 어디에서나 접근 가능하며 생략가능한 default 값
2. `protected` : 해당 클래스 혹은 서브 클래스의 인스턴스에서만 접근 가능, 서브 클래스에서 protected 값을 public으로 오버라이딩하면 해당 값은 public으로 취급
3. `private` : 해당 클래스의 인스턴스에서만 접근 가능, 서브 클래스에서 오버라이딩 시, 에러 발생

<br/>

### 1-2. 추상 클래스 (Abstract Class)

- 추상 클래스는 다른 클래스가 `상속 받을 수만 있는` 클래스를 의미
- class 키워드 앞에 `abstract 키워드`를 붙여서 선언
- 상속받을 경우, 클래스 명 뒤에 `extends (추상 클래스 명)`을 붙여서 상속받음
- 하지만 추상 클래스는 직접 새로운 `인스턴스를 만드는 것은 불가함`

```tsx
// 추상 클래스 상속 예시

abstract class User {
    constructor(
        private firstName: string,
        private lastName: string,
        public nickname: string
    ) {}
}

class Player extends User {}

const jeonggon1 = new Player("jeonggon", "cho", "Gon");

const jeonggon2 = new User("jeonggon", "cho", "Gon"); // 에러 발생 -> 추상 클래스로 인스턴스 생성 불가함
```

<br/>

### 1-3. 추상 클래스 안의 메소드 & 추상 메소드 (Abstract Method)

### - 추상 클래스 안의 메소드

```tsx
// 추상 클래스 안의 메소드

abstract class User {
    constructor(
        private firstName: string,
        private lastName: string,
        public nickname: string
    ) {}
    
    // 메서드 생성
    getFullName() {
        return `${this.firstName} ${this.lastName}` 
    }
}

class Player extends User {}

const jeonggon = new Player("jeonggon", "cho", "Gon");

jeonggon.getFullName(); // 정상적으로 작동
```

<br/>

### - 추상 메소드 (Abstract Method)

- 추상 메소드를 만들려면, 메소드의 내용(implement)를 클래스 안에서 구현하지 않으면 됨
- 해당 클래스를 호출(call signature)할 때, 메소드의 내용 구현
- 추상 메소드는 추상 클래스를 `상속받는 모든 것들이 구현해야하는 메소드`를 의미함
- 메소드 앞에 `abstract` 키워드 붙여서 선언

```tsx
// 추상 메소드

abstract class User {
    constructor(
        protected firstName: string,
        protected lastName: string,
        protected nickname: string
    ) {}
    
    // 추상 메소드 생성
    abstract getNickName(): void
    
    getFullName() {
        return `${this.firstName} ${this.lastName}` 
    }
}

// 추상 메소드를 가진 클래스 호출
class Player extends User {
    // 추상 메소드를 구현해야 에러 안 발생함
    getNickName() {
        // 만약 클래스의 필드가 private(private nickname: string)이면
        // 클래스를 상속하였을지라도
        // 추상 메소드에서 해당 필드를 사용한 console.log 출력 불가함
        // 클래스를 상속받아 사용하려면 protected 사용(protected nickname: string)
        console.log(this.nickname);
    }
}

const jeonggon = new Player("jeonggon", "cho", "Gon");

jeonggon.firstName; // protected이므로 접근 불가능
```