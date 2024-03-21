# 디자인 패턴과 프로그래밍 패러다임

<br>
<br>

## 학습목표

라이브러리, 프레임워크의 기본이 되는 `디자인 패턴` 및 어떻게 로직을 구성해야하는지에 대한 시각이 담긴 `프로그래밍 패러다임` 배우기

> 라이브러리
>
> -   공통으로 사용되는 특정 기능들의 모듈화
> -   폴더명, 파일명 등에 대한 규칙이 없고 프레임워크에 비해 자유로움
> -   `도구`로서 내가 `직접` 컨트롤 할 수 있음
> -   ex) 자르기-가위

> 프레임워크
>
> -   공통으로 사용되는 특정 기능들의 모듈화
> -   폴더명, 파일명 등에 규칙이 있으며 라이브러리에 비해 엄격함
> -   `도구`를 이용하지만 도구가 컨트롤 하고 사용자는 따르면 됨
> -   ex) 운송-비행기(승객은 앉아있으면 됨)

<br>
<br>

## 1. 디자인 패턴

-   디자인 패턴이란 프로그램 설계 시, 문제를 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 `규약`으로 만든 것

<br>

### 1-1. 싱글톤 패턴(singleton pattern)

-   `하나의 클래스`에 `하나의 인스턴스`만 가지는 패턴
-   하나의 클래스 기반으로 여러 개별적인 인스턴스를 만들 수 있지만, 그렇게 하지않고 단, 하나의 인스턴스만 만들어 이를 기반으로 로직을 만듦
-   데이터베이스 연결 모듈에 많이 사용

![](png-clipart-singleton-pattern-software-design-pattern-system-object-instance-singleton-pattern-angle-text.png)

<싱글톤 패턴과 일반적인 관행 비교>

-   장점 : 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기에 인스턴스 생성 비용이 줄어듦
-   단점 : 의존성이 높아짐

<br>

### - 자바스크립트 싱글톤 패턴

-   자바스크립트에서는 리터럴{} 또는 new Object로 객체를 생성하는데 이럴 경우, 다른 어떤 객체와도 같지 않아 이 자체로 싱글톤 패턴을 구현할 수 있다.

```javascript
// ex)

// 자바스크립트 객체와 싱글톤 패턴
const obj = {
    a: 27,
};
const obj2 = {
    a: 27,
};
console.log(obj === obj2);

// 출력
// false;

// obj와 obj2는 다른 인스턴스를 가진다.
// 이 역시 싱글톤 패턴으로 볼 수 있지만, 실제 싱글톤 패턴은 다음과 같이 구성된다.

// -----------------------------------------------------

// 자바스크립트로 구현된 싱글톤 패턴
class Singleton {
    constructor() {
        if (!Singleton.instance) {
            Singleton.instance = this;
        }
        return Singleton.instance;
    }

    getInstance() {
        return this.instance;
    }
}

const a = new Singleton();
const b = new Singleton();
console.log(a === b);

// 출력
// true;

// 위의 코드는 Singleton.instance라는 하나의 인스턴스를 가지는 Singleton 클래스를 구현한 모습
// a와 b는 하나의 인스턴스를 가진다.
```

<br>

### - 데이터베이스 연결 모듈

-   싱글톤 패턴은 데이터베이스 연결 모듈에 많이 사용됨

```javascript
// ex)

const URL = "mongodb://localhost:27017/kundolapp";

const createConnection = (url) => ({ url: url });

class DB {
    constructor(url) {
        if (!DB.instance) {
            DB.instance = createConnection(url);
        }
        return DB.instance;
    }
    connect() {
        return this.instance;
    }
}

const a = new DB(URL);
const b = new DB(URL);
console.log(a === b);

// 출력
// true;

// DB.instance라는 하나의 인스턴스를 기반으로 a, b를 생성하였으며,
// 이를 통해 데이터베이스 연결에 관한 인스턴스 생성 비용을 절감할 수 있다.
```

<br>

### - MySQL의 싱글톤 패턴

-   Node.js에서 MySQL 데이터베이스를 연결할 때도 싱글톤 패턴이 사용된다.

```javascript
// ex)

// 메인 모듈
const mysql = require('mysql');
const pool = mysql.createPool({
	connectionLimit: 10,
	host: 'example.org',
	user: 'kundol',
	password: 'secret',
	database: '승철이디비'
});

pool.connect();

// 모듈 A
pool.query(query, function (error, results, fields)) {
	if (error) throw error;
	console.log('The solution is: ', results[0].solution);
};

// 모듈 B
pool.query(query, function (error, results, fields)) {
	if (error) throw error;
	console.log('The solution is: ', results[0].solution);
};

// 메인 모듈에서 데이터베이스 연결에 관한 인스턴스를 정의
// 다른 모듈인 A, B에서 해당 인스턴스를 기반으로 쿼리를 보내는 형식으로 쓰임
```

<br>

### 1-2. 싱글톤 패턴의 단점

-   싱글톤 패턴은 TDD(Test Driven Development) 할 때, 걸림돌이 됨
-   TDD할 때 단위 테스트를 진행하는데 단위 테스트는 테스트가 서로 독립적이어야 하며, 테스트를 어떤 순서로든 실행할 수 있어야 함
-   싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현되는 패턴이므로 각 테스트마다 독립적인 인스턴스를 만들기가 어렵다.

<br>

### - 의존성 주입

-   싱글톤 패턴은 모듈 간의 결합을 강하게 만들 수 있다는 단점이 있음
-   `의존성 주입(DI, Dependency Injection)`을 통해 모듈 간 `결합을 조금 더 느슨`하게 만들어 해결 가능

> 의존성(종속성) : A가 B에 의존성이 있다는 것은 B의 변경사항에 대해 A 또한 변경되어야 함을 의미

![](R1280x0.png)

-   메인모듈(main module)이 `직접` 하위모듈에 의존성을 주기보다 `중간에` 의존성 주입자(dependency injector)가 이 부분에 관여해 메인모듈이 `간접적`으로 의존성을 주입함
-   이를 `디커플링 된다`고도 함

<br>

### - 의존성 주입 장점

-   모듈 교체가 용이함
-   테스팅하기 쉬움
-   마이그레이션도 수월함
-   구현시, 추상화 레이어를 넣고 이를 기반으로 구현체를 넣기에 애플리케이션 의존성 방향이 일관되고 쉽게추론 가능
-   모듈 간의 관계들이 명확해짐

<br>

### - 의존성 주입 단점

-   모듈들이 더 분리가 되므로 클래스 수가 증가하여 복잡성이 커질 수 있음
-   런타임 패널티가 생길 수 있음

<br>

### - 의존성 주입 원칙

-   상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 함
-   상위 모듈, 하위 모듈 모두 추상화에 의존해야 함
-   추상화는 세부사항에 의존하지 않아야 함

<br>

### 1-3. 팩토리 패턴

-   팩토리 패턴(factory pattern)은 객체를 사용하는 코드에서 `객체 생성 부분`을 떼어내 `추상화`한 패턴이자 상속 관계에 있는 두 클래스에서 `상위 클래스`가 중요한 `뼈대`를 결정하고, `하위 클래스`에서 객체 생성에 관한 `구체적인 내용`을 결정하는 패턴

-   상위 클래스와 하위 클래스 분리로 `느슨한 결합`
-   `상위 클래스`는 인스턴스 생성 방식에 대해 알 필요가 없어 `유연성` 가지며 객체 생성 로직이 하위 클래스로 분리되어있어 코드를 리팩토링 시, 한 곳만 고칠 수 있어 `유지보수성`이 증가

ex)
라떼 레시피, 아메리카노 레시피, 우유 레시피 - 하위 클래스 : 구체적인 레시피 방법
컨베이어 벨트 - 레시피 전달
바리스타 공장 - 상위 클래스 : 레시피 토대로 생산

<br>

### - 자바스크립트 팩토리 패턴

: 자바스크립트에서 팩토리 패턴은 `new Object()`로 구현할 수 있다.

```javascript
// ex)

const num = new Object(42);
const str = new Object("abc");

console.log(num.constructor.name);
console.log(str.constructor.name);

(출력 >> Number) >> String;
```

-   숫자 또는 문자열 등 전달하는 요소에 따라 다른 타입의 객체를 생성함
-   즉, 전달받은 값에 따라 다른 객체를 생성, 다른 인스턴스 타입을 지정

```javascript
// ex)

// 커피 팩토리를 기반으로 라떼 등을 생산하는 코드

class Latte {
    constructor() {
        this.name = "latte";
    }
}

class Espresso {
    constructor() {
        this.name = "Espresso";
    }
}

class LatteFactory {
    static createCoffee() {
        return new Latte();
    }
}

class EspressoFactory {
    static createCoffee() {
        return new Espresso();
    }
}

const factoryList = { LatteFactory, EspressoFactory };

class CoffeeFactory {
    static createCoffee(type) {
        const factory = factoryList[type];
        return factory.createCoffee();
    }
}

const main = () => {
    // 라떼 커피를 주문한다.
    const coffee = CoffeeFactory.createCoffee("LatteFactory");
    // 커피 이름을 부른다.
    console.log(coffee.name);
};

main();

// 출력
// 라떼;
```
