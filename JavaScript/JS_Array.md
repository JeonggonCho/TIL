# 배열

## - 목차
1. [Array](#1-array)
    - [배열의 구조](#1-배열의-구조)
    - [배열과 반복](#2-배열과-반복)
2. [배열과 메서드](#2-배열과-메서드)
    - [배열과 메서드](#1-배열과-메서드)
        - [pop](#--pop)
        - [push](#--push)
        - [shift](#--shift)
        - [unshift](#--unshift)
        - [forEach](#--foreach)
        - [콜백함수(Callback function)](#--콜백함수callback-function)
        - [map](#--map)
    - [배열 정리](#2-배열-정리)
3. [참고](#3-참고)
    - [배열 순회 종합](#1-배열-순회-종합)
    - [콜백함수 구조를 사용하는 이유](#2-콜백함수-구조를-사용하는-이유)

---

## (1) Array

- 순서가 있는 데이터 집합(data collection)을 저장하는 자료구조

<br>

### **1) 배열의 구조**

- 대괄호([ ])를 이용해 작성
- `length`를 사용해 배열에 담긴 요소가 몇 개인지 알 수 있음
- 배열 요소의 자료형에는 제약이 없음
- 배열의 마지막 요소는 객체와 마찬가지로 쉼표(tailing comma)로 끝날 수 있음

```javascript
// Array(배열) 예시

const fruits = ['apple', 'banana', 'coconut'];

console.log(fruits[0]);
console.log(fruits[1]);
console.log(fruits[2]);

console.log(fruits.length);

// 출력
// 'apple'
// 'banana'
// 'coconut'
// 3
```

<br>

### **2) 배열과 반복**

```javascript
// for - 인덱스로 접근하여 반복

for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

// 출력
// 'apple'
// 'banana'
// 'coconut'



// for ... of - 배열을 순회하여 반복

for (const fruit of fruits) {
    console.log(fruit);
}

// 출력
// 'apple'
// 'banana'
// 'coconut'
```


---

## (2) 배열과 메서드

### **1) 배열과 메서드**

|       메서드       |                       기능                        |     역할     |
|:---------------:|:-----------------------------------------------:|:----------:|
|   push / pop    |               배열의 끝 요소를 추가 또는 제거                | 요소 추가 / 제거 |
| unshift / shift |               배열의 앞 요소를 추가 또는 제거                | 요소 추가 / 제거 |
|     forEach     |       인자로 주어진 함수(콜백함수)를 배열 요소에 각각에 대해 실행        |     반복     |
|       map       | 배열 요소 전체를 대상으로 함수(콜백함수)를 호출하고, 함수 호출 결과를 배열로 반환 |     변형     |

<br>

### - pop

- 배열의 끝 요소를 제거하고, 제거한 요소를 반환

```javascript
console.log(fruits.pop()); // 'coconut'
console.log(fruits); // ['apple', 'banana']
```

<br>

### - push

- 배열의 끝에 요소를 추가

```javascript
fruits.push('orange');
console.log(fruits); // ['apple', 'banana', 'orange']
```

<br>

### - shift

- 배열의 앞 요소를 제거하고, 제거한 요소를 반환

```javascript
console.log(fruits.shift()); // 'apple'
console.log(fruits); // ['banana', 'orange']
```

<br>

### - unshift

- 배열의 앞에 요소를 추가

```javascript
fruits.unshift('melon');
console.log(fruits); // ['melon', 'banana', 'orange']
```

<br>

### - forEach

- 인자로 주어진 함수(콜백함수)를 배열 요소 각각에 대해 실행

```javascript
array.forEach(function (item, index, array) {
    statements;
})
```

- 콜백함수는 3가지의 매개변수로 구성
  - item : 배열의 요소
  - index : 배열 요소의 인덱스
  - array : 배열
- 반환 값 : undefined

```javascript
const fruits = ['apple', 'banana', 'coconut']

fruits.forEach(function (item, index, array) {
    console.log(`${item} / ${index} / ${array}`);
})

fruits.forEach((item, index, array) => {
    console.log(`${item} / ${index} / ${array}`);
})

// 출력
// 'apple' / 0 / 'apple', 'banana', 'coconut'
// 'banana' / 1 / 'apple', 'banana', 'coconut'
// 'coconut' / 2 / 'apple', 'banana', 'coconut'
```

<br>

### - 콜백함수(Callback function)

- 다른 함수에 `인자`로 전달되는 함수
- 외부 함수 내에서 호출되어 일종의 루틴이나 특정 작업을 진행

<br>

### - map

- 배열 요소 전체를 대상으로 함수(콜백함수)를 호출하고, 함수 호출의 결과를 모아 `새로운 배열`을 반환

```javascript
const result = array.map(function (item, index, array) {
    statements;
})
```

- 기본적으로 `forEach`와 동작원리가 같지만 forEach와 달리 `새로운 배열 반환`

```javascript
// map 예시1
const fruits = ['apple', 'banana', 'coconut'];

const result = fruits.map(function (fruit) {
    return fruit.length;
});

const result2 = fruits.map((fruit) => {
    return fruit.length;
});

console.log(result);

//출력
// [5, 6, 7]



// map 예시2
const numbers = [1, 2, 3];

const doubleNumber = numbers.map((number) => {
    return number * 2;
});

console.log(doubleNumber);

// 출력
// [2, 4, 6]
```

<br>

### **2) 배열 정리**

- 배열의 본질은 객체
- 배열의 요소를 대괄호 접근법을 사용해 접근하는 건 객체 문법과 동일
- 다만 배열의 키는 인덱스(숫자)임
- 숫자형 키를 사용함으로써 배열은 객체 기본 기능 이외에도 순서가 있는 컬렉션을 제어하게 해주는 특별한 메서드 제공


---

## (3) 참고

### **1) 배열 순회 종합**

| 방식         | 특징                                                                               | 비고    |
|------------|----------------------------------------------------------------------------------|-------|
| for loop   | - 배열의 인덱스를 이용하여 각 요소에 접근<br/>- break, continue 사용 가능                             |       |
| for ... of | - 배열 요소에 바로 접근 가능<br/>- break, continue 사용 가능                                    |       |
| forEach    | - 간결하고 가독성을 높임<br/>- callback함수를 이용하여 각 요소를 조작하기 쉬움<br/>- break, continue 사용 불가능 | 사용 권장 |

```javascript
// 배열 순회 예시


const chars = ['a', 'b', 'c', 'd'];

// for loop
for (let idx = 0; idx < chars.length; idx++) {
    console.log(idx, chars[idx]);
}

// for ... of
for (const char of chars) {
    console.log(char);
}

// forEach
chars.forEach((char, idx) => {
    console.log(idx, char);
})
```

<br>

### **2) 콜백함수 구조를 사용하는 이유**

- `함수의 재사용성 측면`
  - 함수를 호출하는 코드에서 콜백함수의 동작을 자유롭게 변경할 수 있음
  - 예를 들어, map함수는 콜백 함수를 인자로 받아 배열의 각 요소를 순회하며 콜백함수를 실행
  - 이때, 콜백함수는 각 요소를 변환하는 로직을 담당하므로, map함수를 호출하는 코드는 간결하고 가독성이 높아짐


- `비동기적 처리 측면`
  - `setTimeout` 함수는 콜백함수를 인자로 받아 일정 시간이 지난 후, 실행됨
  - 이때, setTimeout 함수는 비동기적으로 콜백함수를 실행하므로, 다른 코드의 실행을 방해하지 않음
  
```javascript
// 비동적으로 콜백함수를 처리하는 setTimeout 함수 예시
console.log('a');

setTimeout(() => {
    console.log('b');
}, 3000)

console.log('c');

// 출력
// 'a'
// 'c'
// 'b'
```