# 리스트 정렬

## 목차

1. [리스트 정렬](#1-리스트-정렬)
    1. [정렬 코드](#1-1-정렬-코드)
    2. [소스코드 수정](#1-2-소스코드-수정)

<br/>
<br/>

## 1. 리스트 정렬

- to do 리스트에서 checked가 true인 항목들 아래에 모아서 정렬하기

<br/>

### 1-1. 정렬 코드

```tsx
// 정렬

// to do 배열을 list로 받기
const sort = (list: IList[]): IList[] => {
    // 기존 배열을 유지하기 위해 새로운 배열 생성
    const newList = [...list];
    // checked가 true/false의 boolean 값인데 앞에 "+"를 붙이면 true는 1, false는 0으로 숫자로 바뀜
    return newList.sort((a, b) => +a.checked - +b.checked);
};
```

<br/>

### 1-2. 소스코드 수정

```tsx
// src/App.tsx

// 정렬 함수 생성
const sort = (list: IList[]): IList[] => {
    const newList = [...list];
    return newList.sort((a, b) => +a.checked - +b.checked);
};

// data.allTodos에 sort() 메서드로 감싸기
<ul>
    {data && sort(data.allTodos).map((item: IList) => (
        <TodoItem
            key={item.id}
            item={item}
            handleRemove={removeTodo}
            handleUpdate={updateTodo}
        />
    ))}
</ul>
```