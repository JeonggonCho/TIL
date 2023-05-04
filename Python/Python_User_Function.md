# 사용자 정의 함수

## - 목차
1. [함수 기본 구조](#1-함수-기본-구조)
    - [선언 및 호출](#1-선언-및-호출)
2. [함수의 결과값(Output)](#2-함수의-결과값output)
3. [함수의 입력(Input)](#3-함수의-입력input)
    - [Positional Arguments](#1-positional-arguments)
    - [Keyword Arguments](#2-keyword-arguments)
    - [Default Arguments Values](#3-default-arguments-values)
    - [정해지지 않은 개수의 Arguments](#4-정해지지-않은-개수의-arguments)
    - [정해지지 않은 개수의 Keyword Arguments](#5-정해지지-않은-개수의-keyword-arguments)
4. [함수의 범위(Scope)](#4-함수의-범위scope)
    - [함수의 scope](#1-함수의-scope)
    - [객체 수명주기](#2-객체-수명주기)
        - [built-in scope](#--built-in-scope)
        - [global scope](#--global-scope)
        - [local scope](#--local-scope)
    - [이름 검색 규칙(Name Resolution)](#3-이름-검색-규칙name-resolution)

---

## (1) 함수 기본 구조

- 선언과 호출(define & call)
- 입력(input)
- 범위(scope)
- 결과값(output)

### **1) 선언 및 호출**

- 함수의 선언은 def 키워드를 활용함
- 들여쓰기를 통해 Function body(실행될 코드 블록)을 작성함
  - Docstring은 함수 body 앞에 선택적으로 작성 가능
  - 작성시에는 반드시 첫 번째 문장에 문자열''' '''
  > Docstring은 함수, 모듈, 클래스, 메서드와 같은 파이썬 코드 블록에 대한 문서화를 위한 문자열
  ```bash
  ex) # Docstring 예시
  
  def add_numbers(a, b):
    """
    두 숫자를 더하는 함수

    Parameters:
    - a (int): 더할 숫자
    - b (int): 더할 숫자

    Returns:
    int: 두 숫자의 합
    """
    return a + b

  ```
- 함수는 parameter를 넘겨줄 수 있음
- 함수는 동작 후에 return을 통해 결과값을 전달함
- 함수는 함수명()으로 호출
  - parameter가 있는 경우, 함수명(값1, 값2, ...)로 호출
    ```bash
    ex)
    def foo():
        return True
    
    # 호출
    foo()
    
    -----------------------------------
    
    def add(x, y):
        return x + y
    
    # 호출
    add(2, 3)
    
    -----------------------------------
    
    num1 = 0
    num2 = 1
    
    def func1(a, b):
        return a + b
    
    def func2(a, b):
        return a - b
    
    def func3(a, b):
        return func1(a, 5) + func2(5, b)
    
    result = func3(num1, num2)
    print(result)
    
    출력
    >> 9
    ```
    
---

## (2) 함수의 결과값(Output)

- 함수는 반드시 값을 하나만 return한다.
  - 명시적인 return이 없는 경우에도 None을 반환한다.
- 함수는 return과 동시에 실행이 종료된다.


Q) 아래 코드의 문제점은 무엇일까?

```bash
def minus_and_product(x, y):
    return x - y
    return x * y

minus_and_product(4, 5)

출력
>> -1

# 두 번째 return의 경우 절대 실행되지 않는다.
```

Q) 두 개 이상의 값을 return을 통해 반환하려면 어떻게 해야할까?

```bash
def minus_and_product(x, y):
    return x - y, x * y
    
minus_and_product(4, 5)

출력
>> (-1, 20)

# 튜플로 반환한다.
```
---

## (3) 함수의 입력(Input)

- Parameter: 함수를 실행할 때, 함수 내부에서 사용되는 식별자
- Argument: 함수를 호출할 때, 넣어주는 값
  - 필수 Argument: 반드시 전달되어야 하는 argument
  - 선택 Argument: 값을 전달하지 않아도 되는 경우는 기본 값이 전달

```bash
ex)
  
def function(x): # paramenter : x
    return x
    
function(1) # argument : 1
```

### **1) Positional Arguments**


: 기본적으로 함수 호출 시 Argument는 위치에 따라 함수 내에 전달됨

```bash
def add(x, y):
    return x + y
    
add(2, 3)
```


### **2) Keyword Arguments**

- 직접 변수의 이름으로 특정 Argument를 전달할 수 있음
- Keyword Argument 다음에 Positional Argument를 활용할 수 없음

```bash
def add(x, y):
    return x + y

add(x = 2, y = 5)
add(2, y = 5)
```


### **3) Default Arguments Values**

- 기본값을 지정하여 함수 호출 시, Argument 값을 설정하지 않도록 함
  - 정의된 것보다 더 적은 개수의 Argument들로 호출될 수 있음

```bash
def add(x, y = 0):
    return x + y
    
add(2)
```


### **4) 정해지지 않은 개수의 Arguments**

- 여러 개의 Positional Argument를 하나의 필수 parameter로 받아서 사용
  - 몇 개의 Positional Argument를 받을지 모르는 함수를 정의할 때 유용
- Argument들은 튜플로 묶여 처리되며, parameter에 *를 붙여 표현

```bash
def add(*args):
    for arg in args:
    print(arg)
    
add(2)
add(2, 3, 4, 5)
```


### **5) 정해지지 않은 개수의 Keyword Arguments**

- 함수가 임의의 개수 Argument를 Keyword Argument로 호출될 수 있도록 지정
- Argument들은 딕셔너리로 묶여 처리되며, parameter에 **를 붙여 표현

```bash
def family(**kwargs):
    for key, value in kwargs:
        print(key, ":", value)

family(father = 'John', mother = 'Jane', me = 'John Jr.')
```

---

## (4) 함수의 범위(Scope)

### **1) 함수의 scope**

- 함수는 코드 내부에 local scope를 생성하며, 그 외의 공간인 global scope로 구분된다.


- scope
  - global scope: 코드 어디에서든 참조할 수 있는 공간
  - local scope: 함수가 만든 scope로 함수 내부에서만 참조 가능


- variable
  - global variable(전역 변수): global scope에 정의된 변수
  - local variable(지역 변수): local scope에 정의된 변수


### **2) 객체 수명주기**


: 객체는 각자의 수명주기(Lifecycle)가 존재한다.


### - built-in scope


: 파이썬이 실행된 이후부터 `영원히` 유지


### - global scope


: 모듈이 `호출된 시점 이후` 혹은 `인터프리터가 끝날 때까지` 유지


### - local scope


: `함수가 호출`될 때 생성되고, 함수가 종료될 때까지 유지


```bash
ex) # 함수 내에서 지정된 변수를 함수 밖에서 사용하면?
  
def func():
    a = 20
    print('local', a)
    
func()
print('global', a)

출력
>> 'local' 20
>> NameError: name 'a' is not defined

# 변수 a는 함수 func 즉, Local Scope에서만 존재한다.
# 따라서 함수 밖에서 변수 a를 사용하게 되면, NameError가 발생한다.
```


### **3) 이름 검색 규칙(Name Resolution)**

- 파이썬에서 사용되는 이름(식별자)들은 `이름공간(namespace)에 저장`되어 있다.
- 아래와 같은 순서로 이름을 찾아나가며, `LEGB Rule`이라고 한다.

| 범위             | 설명                      |
|----------------|-------------------------|
| Local scope    | 함수                      |
| Enclosed scope | 특정 함수의 상위 함수            |
| Global scope   | 함수 밖의 변수, import 모듈     |
| Built-in scope | 파이썬 안에 내장되어 있는 함수 또는 속성 |

- 즉, `함수` 내에서는 `바깥 Scope의 변수`에 `접근은 가능`하지만, `수정은 불가능`하다.

![scope]()


