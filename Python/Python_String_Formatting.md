# String Formatting

## - 목차
1. [String Interpolation](#1-string-interpolation)
    - [%-formatting](#1--formatting)
    - [f-string](#1-f-string) 
   
---

## (1) String Interpolation

### **1) %-formatting**


: 문자열로 변수를 활용하여 만드는 법_1

> %를 사용한 자리 표시자를 통해 자리를 표시하고, 문자열 끝에 자리 표시자에 대입할 변수를 순서에 맞게 작성

| % formatting | 의미                         |
|--------------|----------------------------|
| %c           | 단일문자                       |
| %s           | 문자열                        |
| %d           | 10진수, 정수                   |
| %o           | 8진수(octal)                 |
| %x           | 16진수(hexadecimal)          |
| %f           | 부동소수점으로 소수점 이하 6자리까지 표시    |
| %e 또는 %E     | 지수표기법으로 e는 소문자로 E는 대문자로 출력 |


```bash
ex)

name = 'Kim'
score = 4.5

print('Hello, %s' % name) # %s는 문자열을 나타내는 자리 표시자
print('내 성적은, %d' % score) # %d는 정수(10진수 숫자)를 나타내는 자리 표시자
print('내 성적은, %f' % score) # %f는 부동 소수점 숫자를 나타내는 자리 표시자로 소수점 이하 6자리까지 표시

출력
>> 'Hello, Kim'
>> 내 성적은 4
>> 내 성적은 4.500000
```

### **1) f-string**


: 문자열로 변수를 활용하여 만드는 법_2

> 중괄호{ } 를 사용하며 { }안에 변수를 넣어 문자열로 출력, 괄호 안에서 연산이 가능하다.

```bash
ex)

name = 'Kim'
score = 4.5
print(f'Hello, {name}! 성적은 {score}')

출력
>> 'Hello, Kim! 성적은 4.5'


ex)
  
pi = 3.141592
print(f'원주율은 {pi:.3}. 반지름이 2일때 원의 넓이는 {pi*2*2}')
# {pi:.3}은 인덱스 슬라이싱과 유사하게 소수점 몇번째 자리까지 표시할지 지정한 것

출력
>> '원주율은 3.14. 반지름이 2일때 원의 넓이는 12.566268'
```