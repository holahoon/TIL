# React useReducer hook with TypeScript

아주 간단한 예제를 보자.
```tsx
import { useReducer } from "react";

type Action = { type: "INCREASE" } | { type: "DECREASE" };

// 리듀서
function reducerFunction(state: number, action: Action): number {
  switch (action.type) {
    case "INCREASE":
      return state + 1;
    case "DECREASE":
      return state - 1;
    default:
      throw new Error("something went wrong");
  }
}

export default function Counter() {
  const [count, dispatch] = useReducer(reducerFunction, 0);

  const onIncrease = () => dispatch({ type: "INCREASE" });
  const onDecrease = () => dispatch({ type: "DECREASE" });

  return (
    <div>
      <h2>{count}</h2>
      <div>
        <button onClick={onIncrease}>+</button>
        <button onClick={onDecrease}>-</button>
      </div>
    </div>
  );
}
```
위의 예제는 액션들이 `type`값만 있어어 아주 간단하다. 그러나 우리는 프로쀄셔널 개발자 아닌가?

이번엔 좀더 로직이 들어간 예제를 보잣.
```tsx
import { useReducer } from "react";

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
      throw new Error("Unhandled action");
  }
}

export default function ReducerExample() {
  const someState: State = {
    count: 0,
    text: "Hello",
    color: "blue",
    isGood: true,
  };

  const [state, dispatch] = useReducer(reducerFunction, someState);

  const setCount = () => dispatch({ type: "SET_COUNT", count: 5 });
  const setText = () => dispatch({ type: "SET_TEXT", text: "I'm sexy" });
  const setColor = () => dispatch({ type: "SET_COLOR", color: "red" });
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