# Provider

## 목차

1. [Provider란?](#1-provider란)
    1. [react-redux 라이브러리 설치](#1-1-react-redux-라이브러리-설치)
    2. [Provider로 앱 감싸기](#1-2-provider로-앱-감싸기)
    3. [Todo UI 생성](#1-3-todo-ui-생성)

<br/>
<br/>

## 1. Provider란?

- \<Provider> 구성 요소(컴포넌트)는 `Redux Store 저장소에 액세스`해야하는 `모든 중첩 구성 요(컴포넌트)`소에서 Redux Store를 사용할 수 있도록 함

<br/>

### 1-1. react-redux 라이브러리 설치

- Provider를 사용하기 위해서는 `react-redux` 라이브러리 필요함

```bash
$ npm install react-redux --save
```

<br/>

### 1-2. Provider로 앱 감싸기

- 앱의 모든 컴포넌트는 Redux Store에 연결할 수 있으므로 대부분의 응용 프로그램은 `최상위단에서 <Provider>를 렌더링`함
- 그런 다음 Hooks 및 연결 API는 React의 context 메커니즘을 통해 제공된 저장소 인스턴스에 액세스 할 수 있음

```tsx
// Provider 예시

// src/index.tsx

// ...
// react-redux 라이브러리에서 Provider 컴포넌트 가져오기
import {Provider} from "react-redux";

const store = createStore(rootReducer);

ReactDOM.render(
    <React.StrictMode>
        {/*App 컴포넌트 감싸고 store 전달하기*/}
        <Provider store={store}>
            <App
                onIncrement={() => store.dispatch({type: 'INCREMENT'})}
                onDecrement={() => store.dispatch({type: 'DECREMENT'})}
            />
        </Provider>
    </React.StrictMode>
document.getElementById('root');
)
```

<br/>

### 1-3. Todo UI 생성

- App.tsx의 카운터 UI와 함께 Todo UI 만들기

```tsx
// src/App.tsx

import React, {useState} from "react";
import "./App.css";

type Props = {
    value: number;
    onIncrement: () => void;
    onDecrement: () => void;
};

function App({value, onIncrement, onDecrement}: Props) {
    // To do 입력 상태관리 useState
    const [todoValue, setTodoValue] = useState("");
    // input에 텍스트가 작성될 때마다 To do 값을 최신화하는 handleChange 함수
    const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        setTodoValue(e.target.value);
    };
    // form에서 submit이 발생할 때, 호출할 addTodo 함수
    // e.preventDefault()로 새로고침 막고, input에 작성된 텍스트 초기화하기
    const addTodo = (e: React.FormEvent<HTMLFormElement>) => {
        e.preventDefault();
        setTodoValue("");
    };
    return (
        <div className="App">
            Clicked: {value} times
            <button onClick={onIncrement}>+</button>
            <button onClick={onDecrement}>-</button>

            {/*form 작성*/}
            <form onSubmit={addTodo}>
                <input type="text" value={todoValue} onChange={handleChange} placeholder="할 일을 입력해주세요"/>
                <input type="submit"/>
            </form>
        </div>
    );
}

export default App;

```