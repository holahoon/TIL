# React Context API with TypeScript

##### [ SampleContext.tsx ]
```tsx
import { useReducer, createContext, useContext, Dispatch } from "react";

type Color = "red" | "green" | "blue";

export type State = {
  count: number;
  text: string;
  color: Color;
  isGood: boolean;
};

type Action =
  | { type: "SET_COUNT"; count: number }
  | { type: "SET_TEXT"; text: string }
  | { type: "SET_COLOR"; color: Color }
  | { type: "TOGGLE_GOOD" };

// 디스패치를 위한 타입 (Dispatch 를 리액트에서 불러올 수 있음), 액션들의 타입을 Dispatch 의 Generics로 설정
type SampleDispatch = Dispatch<Action>;

// Global state 만들기
export const globalState: State = {
count: 10,
text: "DK",
color: "green",
isGood: true,
};

// Context 만들기
const SampleStateContext = createContext<State | null>(null);
const SampleDispatchContext = createContext<SampleDispatch | null>(null);

// Reducer 만들기
function reducerFunction(state: State, action: Action): State {
  switch (action.type) {
    case "SET_COUNT":
      return {
        ...state,
        count: action.count,
      };
    case "SET_TEXT":
      return {
        ...state,
        text: action.text,
      };
    case "SET_COLOR":
      return {
        ...state,
        color: action.color,
      };
    case "TOGGLE_GOOD":
      return {
        ...state,
        isGood: !state.isGood,
      };
    default:
      throw new Error("Unhandled action!");
  }
}

export default function SampleProvider({children}: {children: React.ReactNode;}) {
  const [state, dispatch] = useReducer(reducerFunction, globalState);

  return (
    <SampleStateContext.Provider value={state}>
      <SampleDispatchContext.Provider value={dispatch}>
        {children}
      </SampleDispatchContext.Provider>
    </SampleStateContext.Provider>
  );
}

// Custom hook for using state
export function useSampleState() {
  const state = useContext(SampleStateContext);
  if (!state) throw new Error("Cannot find SampleStateProvider"); // 유효하지 않을때 에러발생
  return state;
}

// Custom hook for using dispatch
export function useSampleDispatch() {
  const dispatch = useContext(SampleDispatchContext);
  if (!dispatch) throw new Error("Cannot find SampleDispatchProvider"); // 유효하지 않을때 에러발생
  return dispatch;
}
```

##### [ ReducerSample.tsx ]
```tsx
import { useSampleState, useSampleDispatch } from "./SampleContext";

export default function ReducerSample() {
  const state = useSampleState();
  const dispatch = useSampleDispatch();

  const setCount = () => dispatch({ type: "SET_COUNT", count: 5 });
  const setText = () => dispatch({ type: "SET_TEXT", text: "I'm sexy" });
  const setColor = () => dispatch({ type: "SET_COLOR", color: "blue" });
  const toggleGood = () => dispatch({ type: "TOGGLE_GOOD" });

  return (
    <div>
      <p>
        <code>Count: </code>
        {state.count}
      </p>
      <p>
        <code>Text: </code>
        {state.text}
      </p>
      <p>
        <code>Color: </code>
        {state.color}
      </p>
      <p>
        <code>Good: </code>
        {state.isGood ? "TRUE!" : "FALSE.."}
      </p>

      <div>
        <button onClick={setCount}>Set Count</button>
        <button onClick={setText}>Set Text</button>
        <button onClick={setColor}>Set Color</button>
        <button onClick={toggleGood}>Set Good</button>
      </div>
    </div>
  );
}
```

##### [ App.tsx ]
```tsx
import SampleProvider from "./SampleContext";
import ReducerSample from "./ReducerSample";

const App: React.FC = () => {
  return (
    <>
      <SampleProvider>
        <ReducerSample />
      </SampleProvider>
    </>
  );
};

export default App;
```