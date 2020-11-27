# React with TypeScript

**As of React v17.0, Had to downgrade the version `"react": "16.14.0"` and `"react-dom": "16.9.0"`, to avoid error that occurs in `tsconfig.json` file and JSX**

그냥 타입스크립트를 사용할때엔 파일 익스텐션을 `.ts` 로 하지만 리액트에서 타입 스크립트를 사용할땐 `.jsx` 대신 `.tsx`로 한다.

## Example to demonstrate how to use TS with React

---

### Using it inside component function & `useState` hook

##### [ App.tsx ]
```jsx
import { TextField } from "./TextField";

const App: React.FC = () => {
  return (
    <div>
      <TextField
        text='hello'
        ok
        person={{ firstName: "David", lastName: "Kim" }}
        i={30}
        // fn={someCallbackFunction}
      />
    </div>
  );
};

export default App;
```

##### [ TextField.tsx ]
```jsx
import { useState } from "react";

interface Person {
  firstName: string;
  lastName: string;
}

interface Props {
  text: string;
  ok: boolean;
  i?: number;
  fn?: (name: string) => string;
  //   obj: {
  //     f1: string;
  //   };
  person: Person;
}

interface TextNode {
  text: string | null;
}

export const TextField: React.FC<Props> = ({ person }) => {
  const [count, setCount] = useState<TextNode>({text: 'hello'});

  return (
    <div>
      <input />
    </div>
  );
};
```
- 위에 `interface Person{}` 안에 `firstName` 과 `lastName`에게 string 타입을 선언 한다. `interface Props{}`는 부모에게서 전달받을 props의 타입을 정해놓는건데, `interface Props{}` 안 `person` key에 그 `Person interface`를 선언 해준다. 약간 object를 선언 하는걸 따로 declare 하는 것과 같다.
- 또한 `interface Props{}` 안 key 에 물음표(?) 는 **optional** 이라는 뜻이다. 그 props는 들어올수도 안 들어올수도 있다는 뜻이다.
- 마지막으로 `interface TextNode{}` 에 text 는 `string` 이나 `number` 타입을 받을 것이다 라는 것을 표기한다. 이건 Union type 이라고 한다. useState에 선언 함과 같이 interface로 따로 선언하지 않으면 그냥 `const [count, setCount] = useState<string | null>({text: 'hello'});` 이렇게 적어도 괜찮다. 다만 나중에 유지보수가 조금 힘들 수 있을지도 모른다ㅎㅎ.

---

### Using it inside `useRef` hook

```jsx
interface Props {
  handleChange: (event: React.ChangeEvent<HTMLInputElement>) => void;
}

export const TextField: React.FC<Props> = ({ handleChange }) => {
  const [count, setCount] = useState<TextNode>({ text: "hello" });
  const inputRef = useRef<HTMLInputElement>(null);
  const divRef = useRef<HTMLDivElement>();

  return (
    <div ref={divRef}>
      <input ref={inputRef} onChange={handleChange} />
    </div>
  );
};

```
- `ref` 위에 마우스를 갖다대면 팝업처럼 메시지가 나오는데 그 메시지를 잘 읽어보면 힌트가 나온다. `useRef()` 안에 타입을 넣어주는데 무슨 타입인지 모를때엔 밑에 ref에 마우스를 갖다 대보자. 거기에 아주 좋은 힌트가 나온다. 또한 위에 예제를 보면 `useRef()` 파라미터에 아무것도 넣지 않을 경우에 `ref={divRef}`에서 컴플레인을 한다. 그 메시지를 읽어보면 메시지중 한 라인에 이렇게 적혀있다: `Type 'HTMLDivElement | undefined' is not assignable to type 'HTMLDivElement | null'.` This is pretty self explanatory.
- `onChange` 이벤트도 똑같다. 마우스를 갖다대면 위에 `interface`에 어떤 타입을 선언해 줘야 되는지 힌트를 알려준다.

---

### Using it inside `useReducer` hook

```jsx
import { useReducer } from "react";

type Actions =
  | { type: "ADD"; text: string }
  | { type: "REMOVE"; index: number };
// first object or second object

interface Todo {
  text: string;
  complete: boolean;
}

type State = Todo[]; // creates an array

const TodoReducer = (state: State, action: Actions) => {
  switch (action.type) {
    case "ADD":
      return [...state, { text: action.text, complete: false }];
    case "REMOVE":
      return state.filter((_, i) => action.index !== i);
    default:
      return state;
  }
};

export const ReducerExample: React.FC = () => {
  const [todos, dispatch] = useReducer(TodoReducer, []);

  return (
    <div>
      {JSON.stringify(todos)}
      <button
        onClick={() => {
          dispatch({ type: "ADD", text: "..." });
        }}
      >
        +
      </button>
    </div>
  );
};
```
- `type State = Todo[]` 은 array를 생성한다. Array 만드는 다른 방법으론 `Array<Todo>`도 있다. So, in the above example, I am storing an array of todos.

### Render props
