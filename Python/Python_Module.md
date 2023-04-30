# 모듈

## - 목차
1. [모듈과 패키지](#1-모듈과-패키지)
   - [모듈(Module)](#1-모듈module)
   - [패키지(Package)](#2-패키지package)
2. [파이썬 표준 라이브러리](#2-파이썬-표준-라이브러리)
   - [라이브러리(Library)](#1-라이브러리library)
   - [random](#--random)
   - [datetime](#--datetime)
   - [os](#--os)
3. [파이썬 패키지](#3-파이썬-패키지)
   - [파이썬 패키지 관리자(pip)](#1-파이썬-패키지-관리자pip)
   - [모듈과 패키지 활용](#2-모듈과-패키지-활용)
   - [패키지 활용 명령어](#3-패키지-활용-명령어)
     - [패키지 설치](#--패키지-설치)
     - [패키지 삭제](#--패키지-삭제)
     - [패키지 목록 및 특정 패키지 정보](#--패키지-목록-및-특정-패키지-정보)
     - [패키지 freeze](#--패키지-freeze)
     - [패키지 관리하기](#--패키지-관리하기)

---

## (1) 모듈과 패키지

### **1) 모듈(Module)**


: `특정 기능`을 하는 코드를 파이썬 `파일(.py) 단위`로 작성한 것

### **2) 패키지(Package)**


- 특정 기능과 관련된 여러 `모듈의 집합`
- 패키지 안에는 또 다른 서브 패키지를 포함

---

## (2) 파이썬 표준 라이브러리

### **1) 라이브러리(Library)**

- 다양한 `패키지를 하나의 묶음`으로 만든 것
- 파이썬 표준 라이브러리(Python Standard Library, PSL)는 파이썬에 기본적으로 설치된 모듈과 내장함수들을 포함한다.

> 파이썬 표준 라이브러리 : https://docs.python.org/ko/3/library/index.html

### - random

- 숫자 / 수학 모듈 중 의사 난수를 생성(pseudo random number generator)
  - 대표적으로 임의의 숫자 생성, 무작위 요소의 선택, 무작위 비복원 추출(샘플링)을 위한 함수 제공
  

- random.randint(a, b)
  - a 이상 b 이하의 임의의 정수 N을 반환
  
    ```bash
    ex)
    
    
    ```
    
- random.choice(seq)
  - 비어있지 않은 시퀀스에서 임의의 요소를 반환
  - seq가 비어있으면 indexError를 발생 시킴
  
    ```bash
    ex)
    
    
    ```
    
- random.shuffle(seq)
  - 시퀀스를 제자리에서 섞는다.
  
    ```bash
    ex)
    
    
    ```
    
- random.sample(population, k)
  - 무작위 비복원 추출한 결과인 k 길이의 리스트를 반환
  
    ```bash
    ex)
    
    
    ```

---

### - datetime

- 날짜와 시간을 조작하는 객체를 제공

- 사용 가능한 데이터 타입
  - datetime.date, datetime.time, datetime.datetime, datetime.timedelta 등


- datetime.date(year, month, day)
  - 모든 인자가 필수이며, 인자는 특정 범위에 있는 정수여야 한다.
  - 이 범위를 벗어나는 인자가 주어지면 ValueError가 발생한다.
  
    ```bash
    ex)
    
    
    ```
    
- datetime.date.today()
  - 현재 지역 날짜를 반환한다.
  
    ```bash
    ex)
    
    
    ```
    
- datetime.datetime.today()
  - 현재 지역 datetime을 반환한다.
  - now()를 활용하면 타임존을 설정할 수 있다.
  
    ```bash
    ex)
    
    
    ```
    
---

### - os

- OS(Operation System, 운영체제)를 조작하기 위한 인터페이스 제공


- os.listdir(path='.')
  - path에 의해 주어진 디렉토리에 있는 항목들의 이름을 담고 있는 리스트를 반환
  - 리스트는 임의의 순서로 나열되며, 특수 항목은 포함하지 않는다.
  
    ```bash
    ex)
    
    
    ```
    
- os.mkdir(path)
  - path라는 디렉토리를 만든다.
  
    ```bash
    ex)
    
    
    ```
    
- os.chdir(path)
  - path를 변경한다.
  
    ```bash
    ex)
    
    
    ```
    
---

## (3) 파이썬 패키지

### **1) 파이썬 패키지 관리자(pip)**


: PyPi(Python Package Index)에 저장된 외부 패키지들을 설치하도록 도와주는 패키지 관리 시스템


### **2) 모듈과 패키지 활용**

```bash
ex)
  
import module

from module import var, function, Class

from module import *

from package import module

from package.module import var, function, Class
```


### **3) 패키지 활용 명령어**

### - 패키지 설치

- 최신 버전 / 특정 버전/ 최소 버전을 명시하여 설치 할 수 있다.
- 이미 설치되어 있는 경우, 이미 설치되어 있음을 알리고 아무것도 하지 않는다.

```bash
ex)

# 
$ pip install SomePackage

#
$ pip install SomePackage==1.0.5

#
$ pip install 'SomePackage>=1.0.4'

# 모두 bash, cmd 환경에서 사용되는 명령어
```


### - 패키지 삭제

- pip는 패키지를 업그레이드 하는 경우, 과거버전을 자동으로 지워줌

> $ pip uninstall SomePackage


### - 패키지 목록 및 특정 패키지 정보

> $ pip list

> $ pip show SomePackage


### - 패키지 freeze

- 설치된 패키지의 비슷한 목록을 만들지만, pip install에서 활용되는 형식으로 출력
- 해당하는 목록을 requirements.txt(관행)으로 만들어 관리함

> $ pip freeze


### - 패키지 관리하기

- 아래 명령어들을 통해 패키지 목록을 관리하고 설치할 수 있다.
- 일반적으로 패키지를 기록하는 파일의 이름은 requirements.txt로 정의함

> $ pip freeze > requirements.txt

> $ pip install -r requirements.txt