# 파이썬 응용 / 심화

## - 목차
1. [추가 문법](#1-추가-문법)
   - [조건표현식(Conditional Expression)](#1-조건표현식conditional-expression)
   - [enumerate 순회](#2-enumerate-순회)
   - [리스트 컴프리헨션(List Comprehension)](#3-리스트-컴프리헨션list-comprehension)
   - [딕셔너리 컴프리헨션(Dictionary Comprehension)](#4-딕셔너리-컴프리헨션dictionary-comprehension)
   - [lambda <parameter>: 표현식](#5-lambda-parameter--표현식)
   - [Type annotation](#6-type-annotation)
   - [Positional-only parameter](#7-positional-only-parameter)

---

## (1) 추가 문법

### **1) 조건표현식(Conditional Expression)**


: 조건 표현식을 일반적으로 조건에 따라 값을 할당할 때 사용

```bash
표현식 : <True인 경우 값> if <expression> else <False인 경우 값>

--------------------------------------------------------------

Q) num이 정수일 때, 아래 코드는 무엇을 위한 코드인가?
  
value  =  num   if num >= 0   else  -num
      <참일 경우>  <expression>     <거짓일 경우>


A) 절대값 저장
  
--------------------------------------------------------------

Q) 다음 코드와 동일한 조건 표현식을 작성하기
  
num = 2
if num % 2:
    result = '홀'
else:
    result = '짝'
print(result)

A)

num = 2
result = '홀' if num % 2 else '짝'
print(result)

--------------------------------------------------------------

Q) 다음 코드와 동일한 조건문 작성하기
  
num = -5
value = num if num >= 0 else 0
print(value)

A)
  
num = -5
if num >= 0:
    value = num
else:
    value = 0
print(value)
```


### **2) enumerate 순회**

- enumerate()
  - 인덱스와 객체를 쌍으로 담은 열거형(enumerate) 객체 반환
  - (index, value) 형태의 tuple로 구성된 열거 객체를 반환

```bash
ex)
  
members = ['민수', '영희', '철수']

# 일반적인 표현
for i in range(len(members)):
    print(f'{i} {members[i}')

# enumerate() 사용
for i, member in enumerate(members):
    print(i, member)
    
---------------------------------------------------------

members = ['민수', '영희', '철수']

print(enumerate(members))

출력
>> <enumerate at 0x105d3e100> # enumerate 주소값으로 나옴

print(list(enumerate(members)))

출력
>> [(0, '민수'), (1, '영희'), (2, '철수')] # 인덱스와 값이 튜플로 리스트에 담겨 출력

print(list(enumerate(members, start=1)))

출력
>> [(1, '민수'), (2, '영희'), (3, '철수')] # 기본값은 0이지만, start 값을 지정하면 해당 값부터 순차적으로 증가하여 출력
```


### **3) 리스트 컴프리헨션(List Comprehension)**


: 표현식과 제어문을 통해 특정한 값을 가진 리스트를 간결하게 생성하는 방법

```bash
표현식1 : [<expression> for <변수> in <iterable>]
표현식2 : [<expression> for <변수> in <iterable> if <조건식>]

------------------------------------------------------------------------

Q) 1 ~ 3의 세제곱 결과가 담긴 리스트 만들기
  
A)

# 일반적인 방법
li = []
for i in range(1, 4):
    li.append(i ** 3)
print(li)

출력
>> [1, 8, 27]

# 리스트 컴프리헨션 이용한 방법
li = [i ** 3 for i in range(1, 4)]
print(li)

출력
>> [1, 8, 27]
```


### **4) 딕셔너리 컴프리헨션(Dictionary Comprehension)**


: 표현식과 제어문을 통해 특정한 값을 가진 딕셔너리를 간결하게 생성하는 방법

```bash
표현식1 : {key: value for <변수> in <iterable>}
표현식2 : {key: value for <변수> in <iterable> if <조건식>}

------------------------------------------------------------------------

Q) 1 ~ 3의 세제곱 결과가 담긴 딕셔너리 만들기
  
# 일반적인 방법
dict = {}
for number in range(1, 4):
    dict[number] = number ** 3
print(dict)

출력
>> {1: 1, 2: 8, 3: 27}

# 딕셔너리 컴프리헨션 이용한 방법
{number: number ** 3 for number in range(1, 4)}
```


### **5) lambda [parameter] : 표현식**

- 람다함수
  - 표현식을 계산한 결과값을 반환하는 함수로, 이름이 없는 함수여서 익명함수라고도 불림

- 특징
  - return문을 가질 수 없음
  - 간편 조건문 외 조건문이나 반복문을 가질 수 없음

- 장점
  - 함수를 정의해서 사용하는 것보다 간결하게 사용 가능
  - def를 사용할 수 없는 곳에서도 사용 가능

```bash
ex)
  
# 람다 함수를 이용한 간단한 계산
calculate = lambda x, y, z: (x + y) * z
result = calculate(2, 3, 4)
print(result)

출력
>> 20

---------------------------------------------------

# 리스트의 각 요소에 2를 곱하는 람다 함수
numbers = [1, 2, 3, 4, 5]
doubled = list(map(lambda x: x * 2, numbers))
print(doubled)

출력
>> [2, 4, 6, 8, 10]

---------------------------------------------------

# 조건문을 사용한 람다 함수
is_even = lambda x: x % 2 == 0
print(is_even(4))
print(is_even(7))

출력
>> True
>> False

---------------------------------------------------

# 문자열의 길이 기준으로 정렬하는 람다 함수
words = ['apple', 'banana', 'kiwi', 'orange']
sorted_words = sorted(words, key=lanbda x: len(x))
print(sorted_words)

출력
>> ['kiwi', 'apple', 'banana', 'orange']
```


### **6) Type annotation**

- 동적 타입 언어인 파이썬에서 각 변수(버전3.6이상)/함수(버전3.5이상)마다 Type에 대한 설명을 붙일 수 있음
- 정적 타입으로 변경되는 것은 아니지만, IDE/텍스트 에디터를 통해 경고를 확인하고, 코드를 작성하는 과정에서 도움을 받을 수 있다.

```bash
ex)
  
hello: str = "hello world"

def add(x: int, y: int) -> int:
    return x + y

result: int = add(7, 4)
```

참고 : https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html

### **7) Positional-only parameter**

- 함수를 정의할 때, 어떻게 호출해야 하는지를 함께 지정
  - a, b는 위치만
  - c, d는 위치 및 키워드 모두
  - e, f는 키워드만

```bash
ex)

# '/' 이 기호는 Positional-only parameter의 끝을 나타낸다.
# 이 기호 전은 위치로만 인자 제공한다.
# 이 기호 이후는 위치 또는 키워드 방식으로 지정할 수 있다.
# '*' 이 기호는 Keyword-only patameter의 시작을 나타낸다.
# 이 기호 이후는 키워드 방식으로만 매개변수를 지정할 수 있다.
def func(a, b, /, c, d, *, e, f):
    print(a, b, c, d, e, f)

# 호출
func(1, 2, 3, d=4, e=5, f=6)
```

참고 : https://docs.python.org/3/whatsnew/3.8.html?highlight=function%20argument%20positional#positional-only-parameters