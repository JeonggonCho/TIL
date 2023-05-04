# 사용자 정의 클래스

---

## (1) 객체 지향 프로그래밍


: 파이썬은 모두 객체(object)로 이뤄져 있다. 객체는 특정 타입의 인스턴스(instance)이다.

- 123, 900, 5 는 모두 int의 인스턴스
- 'hello', 'world' 는 모두 string의 인스턴스
- [123, 45, 6], [] 은 모두 list의 인스턴스


### **1) 객체**

- 객체(object)의 특징
  - 타입(type): 어떤 연산자(operator)와 조작(method)이 가능한가
  - 속성(attribute): 어떤 상태(데이터)를 가지는가
  - 조작법(method): 어떤 행위(함수)를 할 수 있는가


### **2) 절차 지향 프로그래밍(POP, Procedure Oriented Programming)**

- 개념
  - 프로그램을 실행하는데 필요한 절차나 단계를 중심으로 설계됨
  - 프로그램은 주로 함수 또는 서브루틴의 모음으로 이루어짐
  - 데이터와 함수(절차)가 별도로 존재하며, 데이터를 처리하기 위한 절차를 차례대로 실행하는 방식

- 구조
  - 프로그램의 구조가 단계별로 나뉘어져 있음
  - 데이터와 함수가 분리되어 있어 데이터가 변경되면 해당 데이터를 처리하는 함수도 변경이 필요

- 재사용성과 확장성
  - 함수를 재사용하는데 중점
  - 코드의 재사용성이 낮을 수 있고, 큰 프로젝트에서 유지보수가 어려움

- 대표적인 언어
  - C언어

```bash
ex) # 사각형 넓이 및 둘레 구하기(절차 지향 프로그래밍)
  
a = 10
b = 30

square1_area = a * b
square1_circumference = 2 * (a + b)

c = 300
d = 20

square2_area = c * d
square2_circumference = 2 * (c + d)
```


### **3) 객체 지향 프로그래밍(OOP, Object Oriented Programming)**

- 개념
  - 프로그램을 여러 개의 독립된 객체들과 그 객체들 간의 상호작용으로 파악하는 프로그래밍 방법
  - 객체는 데이터와 그 데이터를 처리하는 메서드(함수)를 함께 포함하는 캡슐화된 개념
  - 상속, 다형성, 캡슐화와 같은 개념을 통해 유연하고 모듈화된 코드 작성이 가능

- 구조
  - 프로그램을 객체 단위로 설계하며, 객체 간의 상호작용이 중요
  - 데이터와 해당 데이터를 처리하는 메서드가 객체 내부에 캡슐화되어 있어 데이터 변경에 따른 영향이 해당 객체 내부로 제한

- 재사용성과 확장성
  - 객체의 재사용성이 높음
  - 캡슐화를 통해 모듈화되어 있어 객체를 쉽게 재사용하고, 상속 등을 통해 쉽게 확장 가능

- 대표적인 언어
  - Java, C++, Python 등

```bash
ex) # 사각형 넓이 및 둘레 구하기(객체 지향 프로그래밍)

def area(x, y):
    return x * y  

def circumference(x, y):
    return 2 * (x + y)
    
a = 10
b = 30
c = 300
d = 20

square1_area = area(a, b)
square1_circumference = circumference(a, b)
square2_area = area(c, d)
square2_circumference = circumference(c, d)

-----------------------------------------------------------------

# 클래스 적용

class Rectangle:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        
    def area(self):
        return self.x * self.y
        
    def circumference(self):
        return 2 * (self.x + self.y)
        
r1 = Rectangle(10, 30)
r1.area()
r1.circumference()

r2 = Rectangle(300, 20)
r2.area()
r2.circumference()
```

- 사각형: 클래스(class)
- 각각의 사각형(r1, r2): 인스턴스(instance)
- 사각형의 정보: 속성(attribute)
  - 가로 길이, 세로 길이
- 사각형의 행동/기능: 메서드(method)
  - 넓이 구하기, 높이 구하기

 
### - 정리

<OOP와 POP 비교>

![객체_절차](../img/python_객체_절차.png)

- 절차 지향 프로그래밍(POP)
  : 데이터와 함수로 인한 변화

- 객체 지향 프로그래밍(OOP)
  : 데이터와 기능(메소드) 분리, 추상화된 구조로 프로그램을 유연하고 변경에 용이하게 하므로 대규모 소프트웨어 개발에 많이 사용된다. 또한 개발과 보수를 간편하게 하고, 직관적으로 코드를 분석할 수 있게 하는 장점이 있다.

---

## (2) 클래스와 인스턴스

### **1) 기본 문법**

```bash
# 클래스 정의
class MyClass:
    pass

# 인스턴스 생성
my_instance = MyClass()

# 메서드 호출
my_instance.my_method()

# 속성
my_instance.my_attribute
```

### **2) 정의**

### - 클래스


: 객체들의 분류(class)


---

### - 인스턴스


: 하나 하나의 실체/예(instance)

- 파이썬은 모든 것이 객체, 모든 객체는 특정 타입의 인스턴스!


[인스턴스 변수]

- 인스턴스가 개인적으로 가지고 있는 속성(attribute)
- 각 인스턴스 들의 고유한 변수
- 생성자 메소드에서 self.<name>으로 정의
- 인스턴스가 생성된 이후, <instance>.<name>으로 접근 및 할당

```bash
ex)
  
class Person:
    def __init__(self, name):
        self.name = name # 인스턴스 변수 정의

tom = Person('tom')
print(tom.name) # 인스턴스 변수 접근

출력
>> 'tom'

tom.name = 'james' # 인스턴스 변수 할당
print(tom.name)

출력
>> 'james'
```

[인스턴스 메소드]

- 인스턴스 변수를 사용하거나, 인스턴스 변수에 값을 설정하는 메소드
- 클래스 내부에 정의되는 메소드의 기본
- 호출 시, 첫번째 인자로 인스턴스 자기자신(self)이 전달됨

```bash
ex)
  
class MyClass:
    
    def instance_method(self, arg1, arg2, ...)

my_instance = MyClass()
my_instance.instance_method(...)
```

[self]

- 인스턴스 자기자신
- 파이썬에서 인스턴스 메소드는 호출 시, 첫번째 인자로 인스턴스 자신이 전달되도록 설계
  - 매개변수 이름으로 self를 첫번때 인자로 정의
  - 다른 단어로 써도 작동하지만, 파이썬의 암묵적인 규칙


---

### - 속성


: 특정 데이터 타입/클래스의 객체들이 가지게 될 상태/데이터를 의미

```bash
ex)
  
class Person:
    def __init__(self, name):
        self.name = name
        
person1 = Person('철수')
person1.name

출력
>> '철수'

# 클래스 Person은 name 속성으로 '철수'를 받음
```


---

### - 메소드


: 특정 데이터 타입/클래스의 객체에 공통적으로 적용 가능한 행위(함수)

```bash
ex)
  
class Person:
    def talk(self):
        print('안녕')
        
    def eat(self, food):
        print(f'{food}를 먹다')
        
person1 = Person()

person1.talk()

출력
>> '안녕'

person1.eat('피자')

출력
>> '피자를 먹다'

person1.eat('치킨')

출력
>> '치킨을 먹다'
```


[생성자(constructor) 메소드]

- 인스턴스 객체가 생성될 때, 자동으로 호출되는 메소드
- 인스턴스 변수들의 초기값을 설정
  - 인스턴스 생성
  - \__init\__ 메소드 자동 호출

```bash
ex)
  
class Person:
    def __init__(self):
        print('인스턴스가 생성되었습니다.')

person1 = Person()

출력
>> '인스턴스가 생성되었습니다.'

--------------------------------------------------

class Person:
    def __init__(self, name):
        print(f'인스턴스가 생성되었습니다. {name}')

person1 = Person('철수')

출력
>> '인스턴스가 생성되었습니다. 철수'
```


[소멸자(destructor) 메소드]

- 인스턴스 객체가 소멸(파괴)되기 직전에 호출되는 메소드

```bash
ex)
  
class Person:
    def __del__(self):
        print('인스턴스가 사라졌습니다.')
        
person1 = Person()
del person1

출력
>> '인스턴스가 사라졌습니다.'
```


[매직 메소드]

- Double underscore(__)가 있는 메소드는 특수한 동작을 위해 만들어진 메소드로 스페셜 메소드 혹은 매직 메소드라고 불림
- 특정 상황에 자동으로 불리는 메소드
- 종류 및 예시

```bash
__str__(self),
__len__(self),
__repr__(self),
__lt__(self, other),
__le__(self, other),
__eq__(self, other),
__gt__(self, other),
__ge__(self, other),
__ne__(self, other)

--------------------------------------------------

class Circle:
    
    def __init__(self, r):
        self.r = r
        
    def area(self):
        return 3.14 * self.r * self.r
    
    # __str__    
    # 해당 객체 출력 시 자동으로 호출되며, 출력 형태를 지정한다.
    # 어떤 인스턴스를 출력하면 __str__의 return 값이 출력
    def __str__(self):
        return f'[원] radius: {self.r}'
        
    # __gt__
    # 부등호 연산자(>, greater than)
    def __gt__(self, other):
        return self.r > other.r
        
c1 = Circle(10)
c2 = Circle(1)

print(c1)

출력
>> '[원] radius: 10'

print(c2)

출력
>> '[원] radius: 1'

c1 > c2

출력
>> True

c1 < c2

출력
>> False
```


---

### - 객체 비교하기

- ==
  - 동등한(equal)을 의미
  - 변수가 참조하는 객체가 동등한(= 내용이 같은) 경우, True
  - 두 객체가 같아 보이지만 실제로 동일한 대상을 가리키고 있다고 확인해 준 것은 아님

- is
  - 동일한(identical)을 의미
  - 두 변수가 동일한 객체를 가리키는 경우, True

```bash
ex)
  
a = [1, 2, 3]
b = [1, 2, 3]

print(a == b, a is b)

출력
>> True, False

-------------------------------------

a = [1, 2, 3]
b = a

print(a == b, a is b)

출력
>> True, True
```