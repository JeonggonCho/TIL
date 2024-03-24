# 데이터 생성, 삭제, 수정 - useMutation

## 목차

1. [useMutation](#1-usemutation)
    1. [데이터 생성하기 - create](#1-1-데이터-생성하기---create)
        - [데이터 생성 소스코드](#--데이터-생성-소스코드)
        - [생성한 데이터를 캐시 데이터에 업데이트](#--생성한-데이터를-캐시-데이터에-업데이트)
    2. [데이터 삭제하기 - remove](#1-2-데이터-삭제하기---remove)
        - [데이터 삭제 소스코드](#--데이터-삭제-소스코드)
        - [삭제한 데이터를 캐시 데이터에 업데이트](#--삭제한-데이터를-캐시-데이터에-업데이트)
    3. [데이터 수정하기 - update](#1-3-데이터-수정하기---update)
        - [checked 데이터 수정 소스코드](#--checked-데이터-수정-소스코드)
        - [text 데이터 수정 소스코드](#--text-데이터-수정-소스코드)
        - [focus 주기 - useRef](#--focus-주기---useref)
        - [수정 데이터를 ROOT_QUERY에 업데이트할 필요 없음](#--수정-데이터를-root_query에-업데이트할-필요-없음)

<br/>
<br/>

## 1. useMutation

- Hooks로 Apollo 애플리케이션에서 Mutation을 실행하기 위한 기본 API임
- useMutation()의 `인자로 생성한 mutation 넣기`
- 아래 예시에서 반환되는 `addTodo`는 `함수`이며, `error`의 경우, useQuery에서 반환하는 error와 이름을 구분하기위해 `addError 별칭`으로 짓기

```tsx
// 생성 useMutation 예시

const [addTodo, {error: addError}] = useMutation(ADD_TODO);
```

<br/>

- todos.ts에 생성한 ADD_TODO mutation을 보면, `addTodo`에서 `text`와 `checked`를 필요로 함

```ts
// src/apollo/todos.ts의 addTodo 예시

export const ADD_TODO = gql`
    mutation addTodo($text: String!, $checked: Boolean!) {
        createTodo(text: $text, checked: $checked) {
            text
            checked
            id
        }
    }
`;
```

<br/>

### 1-1. 데이터 생성하기 - create

### - 데이터 생성 소스코드

```tsx
// src/App.tsx

// useMutation Hooks가져오기
import {useMutation, useQuery} from "@apollo/client";
import {ADD_TODO, GET_TODOS} from "./apollo/todos";
import "./App.css";
import {IList} from "./types.ts";
import TodoItem from "./components/TodoItem.tsx";
import {useState} from "react";

function App() {
    const {loading, error, data} = useQuery(GET_TODOS);
    const [input, setInput] = useState("")

    // useMutation() 인자로 mutation인 ADD_TODO 넣기
    const [addTodo, {error: addError}] = useMutation(ADD_TODO);

    const counter = (): string => {
        if (data?.allTodos as IList[]) {
            const completed = (data.allTodos as IList[]).filter((todo: IList) => todo.checked);
            return `${completed.length}/${(data.allTodos as IList[]).length}`;
        }
        return "0/0"
    };

    // to do 작성 후, 제출 시, 실행할 handleSubmit 함수
    const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
        // 새로고침 막기
        e.preventDefault();

        // input에 작성된 to do가 없으면 return
        if (input.trim() === "") return;

        // 작성된 to do가 있으면
        // addTodo mutation 함수 호출
        addTodo({
            // 변수 넣기
            variables: {
                // 텍스트는 input, 완료여부는 false로 넣기
                text: input,
                checked: false
            }
        })

        // input 다시 비우기
        setInput("");
    };

    if (error) return <div>Network Error</div>;
    return (
        <div className="flex flex-col items-center">
            <div className="mt-5 text-3xl">Todo App{" "}<span className="text-sm">{counter()}</span></div>
            <div className="w-5/6 md:w-1/2 lg:w-3/5">

                {/*submit 발생 시, handelSubmit 함수 호출*/}
                <form onSubmit={handleSubmit}
                      className="flex justify-between p-5 my-5 text-4xl border-2 rounded-md shadow-md">
                    <input
                        placeholder="할 일을 작성해주세요."
                        type="text"
                        className="outline-none border-b-[1px] text-xl w-10/12 focus:border-b-[3px]"
                        value={input} onChange={(e) => setInput(e.target.value)}
                    />
                    <div>
                        <button type="submit" className="hover:scale-105">+</button>
                    </div>
                </form>
                {loading ? <div>loading...</div> :
                    <ul>
                        {data && data.allTodos.map((item: IList) => (
                            <TodoItem
                                key={item.id}
                                item={item}
                            />
                        ))}
                    </ul>
                }
            </div>
        </div>
    );
}

export default App;
```

- to do를 작성 후, submit 버튼을 누르면 아무 변화가 없으며 페이지 새로고침을 해야 화면에 TodoItem이 반영 됨
- 그 이유는 Root_Query에 새로 추가한 to do 리스트가 안 들어가있기 때문

<br/>

### - 생성한 데이터를 캐시 데이터에 업데이트

- `캐시`는 데이터에 대한 기존 쿼리에 새로 생성된 엔터티를 언제 추가해야하는지 모르기에 `업데이트 기능을 작성`해주어야 함
- useMutation의 두 번째 인자로 `update()` 추가
- `readQuery` : 캐시에서 직접 GraphQL 쿼리를 실행할 수 있는 메서드
- `writeQuery` : GraphQL 쿼리와 일치하는 형태로 캐시에 데이터를 쓸 수 있는 메서드

```tsx
// src/App.tsx

const [addTodo, {error: addError}] = useMutation(ADD_TODO, {
    // cache : 객체로서 readQuery, writeQuery 메서드 등을 포함
    // createTodo : addTodo 함수의 결과로 반환된 데이터 객체
    update(cache, {data: {createTodo}}) {
        // 캐시의 모든 to do 데이터 가져오기
        const previousTodos = cache.readQuery<AllTodosCache>({query: GET_TODOS})?.allTodos;
        // 캐시에 데이터 업데이트
        cache.writeQuery({
            query: GET_TODOS,
            data: {
                allTodos: [createTodo, ...previousTodos as IList[]],
            },
        });
    },
});
```

<br/>

### 1-2. 데이터 삭제하기 - remove

```tsx
// 삭제 useMutation 예시

const [removeTodo, {error: removeError}] = useMutation(REMOVE_TODO);
```

<br/>

### - 데이터 삭제 소스코드

```tsx
// src/App.tsx

const [removeTodo, {error: removeError}] = useMutation(REMOVE_TODO);

<TodoItem
    key={item.id}
    item={item}

    // TodoItem 컴포넌트에 removeTodo 함수 props로 전달
    handleRemove={removeTodo}
/>
```

<br/>

```tsx
// src/components/TodoItem.tsx

import {FC, useState} from "react";
import {IList} from "../types.ts";
import {FiEdit, FiMinusCircle} from "react-icons/fi";

interface TodoItemProps {
    item: IList;
    // handleRemove의 interface 타입 지정
    hadleRemove: (options: { variables: { id: number } }) => void;
}

// props에 handleRemove 추가
const TodoItem: FC<TodoItemProps> = ({item, handleRemove}) => {
    const [edit, setEdit] = useState(true);
    const [task, setTask] = useState(item.text);

    return (
        <li className="flex items-center justify-between p-5 my-3 text-2xl duration-300 hover:scale-105 border-2 rounded-md shadow-md">
            <div className="flex items-center w-10/12">
                <input checked={item.checked} type="checkbox" className="hover:scale-105 hover:cursor-pointer"/>
                <input
                    className={`outline-none h-[25px] text-xl w-full mx-5 px-3 disabled:bg-transparent focus:border-b-[1px] ${item.checked && 'line-through'} text-stone-500`}
                    type="text"
                    disabled={!edit}
                    value={task}
                    onChange={e => setTask(e.target.value)}/>
            </div>
            <div className="flex justify-between w-1/6">
                <FiEdit className="hover:scale-105 hover:cursor-pointer"/>

                {/*해당 아이콘 컴포넌트 클릭 시, handleRemove 함수 호출, handleRemove의 id인자로 item.id 전달*/}
                <FiMinusCircle onClick={() => handleRemove({variables: {id: item.id}})}
                               className="hover:scale-105 hover:cursor-pointer"/>
            </div>
        </li>
    )
}

export default TodoItem;
```

<br/>

### - 삭제한 데이터를 캐시 데이터에 업데이트

- Root_Query에서는 삭제 여부를 확인할 수 없으며, 새로고침을 해야 화면에 반영됨
- 캐시는 기존 쿼리에서 항목을 언제 제거해야하는지 알 수 없기에 update 기능에서 항목을 필터링하여 캐시 값을 수동으로 업데이트 해야 함
- `cache` 객체의 `modify` 메서드를 활용
- `modify` : GraphQL을 사용하지 않고 캐시 데이터를 조작할 수 있는 메서드

```tsx
// src/components/TodoItem.tsx

const [removeTodo, {error: removeError}] = useMutation(REMOVE_TODO, {
    update(cache, {data: {removeTodo}}) {
        cache.modify({
            fields: {
                allTodos(currentTodos: { __ref: string } [] = []) {
                    return currentTodos.filter((todo) => todo.__ref !== `Todo:${removeTodo.id}`);
                }
            }
        })
    }
});
```

<br/>

### 1-3. 데이터 수정하기 - update

- 수정하는 부분은 `text 수정`, `checked 수정`이 있음

```tsx
// 수정 useMutation 예시

const [updateTodo, {error: updateError}] = useMutation(UPDATE_TODO);
```

<br/>

### - checked 데이터 수정 소스코드

```tsx
// src/App.tsx

const [updateTodo, {error: updateError}] = useMutation(UPDATE_TODO);

<TodoItem
    key={item.id}
    item={item}
    handleRemove={removeTodo}

    // TodoItem 컴포넌트에 updateTodo 함수 props로 전달
    handleUpdate={updateTodo}
/>
```

<br/>

```tsx
// src/components/TodoItem.tsx

import {FC, useState} from "react";
import {IList} from "../types.ts";
import {FiEdit, FiMinusCircle} from "react-icons/fi";

interface TodoItemProps {
    item: IList;
    hadleRemove: (options: { variables: { id: number } }) => void;
    // handleUpdate의 interface 타입 지정
    hadleUpdate: (options: { variables: IList }) => void;
}

// props에 handleUpdate 추가
const TodoItem: FC<TodoItemProps> = ({item, handleRemove, handleUpdate}) => {
    const [edit, setEdit] = useState(true);
    const [task, setTask] = useState(item.text);

    return (
        <li className="flex items-center justify-between p-5 my-3 text-2xl duration-300 hover:scale-105 border-2 rounded-md shadow-md">
            <div className="flex items-center w-10/12">
                {/*해당 아이콘 컴포넌트 클릭 시, handleUpdate 함수 호출*/}
                <input
                    onChange={() => handleUpdate({variables: {id: item.id, text: task, checked: !item.checked}})}
                    checked={item.checked} type="checkbox"
                    className="hover:scale-105 hover:cursor-pointer"
                />
                <input
                    className={`outline-none h-[25px] text-xl w-full mx-5 px-3 disabled:bg-transparent focus:border-b-[1px] ${item.checked && 'line-through'} text-stone-500`}
                    type="text"
                    disabled={!edit}
                    value={task}
                    onChange={e => setTask(e.target.value)}
                />
            </div>
            <div className="flex justify-between w-1/6">
                <FiEdit className="hover:scale-105 hover:cursor-pointer"/>
                <FiMinusCircle onClick={() => handleRemove({variables: {id: item.id}})}
                               className="hover:scale-105 hover:cursor-pointer"/>
            </div>
        </li>
    )
}

export default TodoItem;
```

<br/>

### - text 데이터 수정 소스코드

```tsx
// src/components/TodoItem.tsx

import {FC, useState} from "react";
import {IList} from "../types.ts";
import {FiEdit, FiMinusCircle} from "react-icons/fi";

interface TodoItemProps {
    item: IList;
    hadleRemove: (options: { variables: { id: number } }) => void;
    hadleUpdate: (options: { variables: IList }) => void;
}

const TodoItem: FC<TodoItemProps> = ({item, handleRemove, handleUpdate}) => {
    const [edit, setEdit] = useState(true);
    const [task, setTask] = useState(item.text);

    // hadleTextChange 함수 생성
    const handleTextChange = (item: IList) => {
        // 호출 시, edit 토글하여 text input의 disabled 상태 바꾸기
        setEdit(!edit);
        // edit이 true의 상태이면 handleUpdate를 통해 데이터 수정 할 수 있음
        if (edit) {
            handleUpdate({
                variables: item
            })
        }
    };

    return (
        <li className="flex items-center justify-between p-5 my-3 text-2xl duration-300 hover:scale-105 border-2 rounded-md shadow-md">
            <div className="flex items-center w-10/12">
                <input
                    onChange={() => handleUpdate({variables: {id: item.id, text: task, checked: !item.checked}})}
                    checked={item.checked} type="checkbox"
                    className="hover:scale-105 hover:cursor-pointer"
                />
                <input
                    className={`outline-none h-[25px] text-xl w-full mx-5 px-3 disabled:bg-transparent focus:border-b-[1px] ${item.checked && 'line-through'} text-stone-500`}
                    type="text"
                    disabled={!edit}
                    value={task}
                    onChange={e => setTask(e.target.value)}
                />
            </div>
            <div className="flex justify-between w-1/6">
                <FiEdit
                    // 해당 아이콘 컴포넌트 클릭 시, handleTextChange 함수 호출
                    onClick={() => handleTextChange({id: item.id, text: task, checked: item.checked})}
                    className="hover:scale-105 hover:cursor-pointer"
                />
                <FiMinusCircle
                    onClick={() => handleRemove({variables: {id: item.id}})}
                    className="hover:scale-105 hover:cursor-pointer"
                />
            </div>
        </li>
    )
}

export default TodoItem;
```

<br/>

### - focus 주기 - useRef

- text 수정 버튼 클릭 시, text input에 자동으로 focus되지 않는 문제 useRef()를 통해 해결하기

```tsx
// src/components/TodoItem.tsx

// useRef, useEffect Hooks 가져오기
import {FC, useEffect, useRef, useState} from "react";
import {IList} from "../types.ts";
import {FiEdit, FiMinusCircle} from "react-icons/fi";

interface TodoItemProps {
    item: IList;
    hadleRemove: (options: { variables: { id: number } }) => void;
    hadleUpdate: (options: { variables: IList }) => void;
}

const TodoItem: FC<TodoItemProps> = ({item, handleRemove, handleUpdate}) => {
    const [edit, setEdit] = useState(true);
    const [task, setTask] = useState(item.text);

    // useRef로 inputRef 선언
    const inputRef = useRef<HTMLInputElement>(null);

    // useEffect로 edit이 수정 될 때마다 inputRef를 ref로 가진 요소에 focus 주기
    useEffect(() => {
        inputRef?.current?.focus();
    }, [edit])

    const handleTextChange = (item: IList) => {
        setEdit(!edit);
        if (edit) {
            handleUpdate({
                variables: item
            })
        }
    };

    return (
        <li className="flex items-center justify-between p-5 my-3 text-2xl duration-300 hover:scale-105 border-2 rounded-md shadow-md">
            <div className="flex items-center w-10/12">
                <input
                    onChange={() => handleUpdate({variables: {id: item.id, text: task, checked: !item.checked}})}
                    checked={item.checked} type="checkbox"
                    className="hover:scale-105 hover:cursor-pointer"
                />
                <input
                    // 해당 input의 ref 속성에 inputRef 할당
                    ref={inputRef}
                    className={`outline-none h-[25px] text-xl w-full mx-5 px-3 disabled:bg-transparent focus:border-b-[1px] ${item.checked && 'line-through'} text-stone-500`}
                    type="text"
                    disabled={!edit}
                    value={task}
                    onChange={e => setTask(e.target.value)}
                />
            </div>
            <div className="flex justify-between w-1/6">
                <FiEdit
                    onClick={() => handleTextChange({id: item.id, text: task, checked: item.checked})}
                    className="hover:scale-105 hover:cursor-pointer"
                />
                <FiMinusCircle
                    onClick={() => handleRemove({variables: {id: item.id}})}
                    className="hover:scale-105 hover:cursor-pointer"
                />
            </div>
        </li>
    )
}

export default TodoItem;
```

<br/>

### - 수정 데이터를 ROOT_QUERY에 업데이트할 필요 없음

- 데이터 생성 및 삭제와 달리, 수정은 Root_Query에 수동으로 업데이트 할 필요없음
- `생성 및 삭제` mutation의 경우, `ROOT_QUERY(캐시)`를 수정해주어야 했음
- 하지만, `수정` mutation의 경우, 고유식별자 `내부의 데이터 정보`가 수정되며 화면에 바로 렌더링 됨

```
생성 및 삭제에서 수정되어야하는 부분은 ROOT_QUERY임

ROOT_QUERY { --> 수동으로 목록 업데이트 필요
  allTodos: [
    0: {
      __ref: "Todo: 1"
    }
  ]
}
```

```
수정 시, 고유식별자 내부의 데이터 정보가 바뀜

Todo: 1 : { --> 해당 데이터의 내부 요소가 자체적으로 수정되어 컴포넌트 렌더링 됨
  __typename: "Todo"
  text: "개발공부하기"
  checked: true
  id: "0"
}
```