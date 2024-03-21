# 형 변환(Typecasting)

## 목차

1. [자료형 변환(Typecasting)](#1-자료형-변환typecasting)
    1. [암시적 형 변환(Implicit Typecasting)](#1-1-암시적-형-변환implicit-typecasting)
    2. [명시적 형 변환(Explicit Typecasting)](#1-2-명시적-형-변환explicit-typecasting)

<br>
<br>

## 1. 자료형 변환(Typecasting)

-   파이썬에서 데이터 형태는 서로 변환할 수 있음

<br>

### 1-1. 암시적 형 변환(Implicit Typecasting)

-   사용자가 의도하지 않고, 파이썬 내부적으로 자료형을 변환하는 경우

-   bool
-   Numeric type (int, float, compl# ex)

<br>

```python
# ex) bool값이 True인 경우, 1로 False인 경우, 0으로 형 변환

True + 3

# 출력
# 4



# ex) 정수와 실수의 연산의 경우, 정수를 실수로 형 변환

3 + 5.0

# 출력
# 8.0



# ex) 문자가 포함된 연산에서 숫자끼리 연산을 하고 4j의 경우, 복소수(실수와 허수의 합)로 인식

3 + 4j + 5

# 출력
# (8 + 4j)
```

<br>

### 1-2. 명시적 형 변환(Explicit Typecasting)

-   사용자가 특정 함수를 활용하여 의도적으로 자료형을 변환하는 경우
-   int
-   float
-   str

<br>

```python
# ex) 문자열은 암시적 타입 변환이 되지 않음

'3' + 4

# 출력
# TypeError: can only concatenate str (not "int") to str



# ex) 명시적 타입 변환이 필요함

int('3') + 4

# 출력
# 7



# ex) 정수 형식이 아닌 경우, 타입 변환할 수 없음

int('3.5') + 5

# 출력
# ValueError: invalid literal for int() with base 10: '3.5'



# ex)

float('3.5') + 5

# 출력
# 8.5
```
