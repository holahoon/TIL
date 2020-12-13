# Typesafe-actions

#### [Github documentation](https://github.com/piotrwitek/typesafe-actions)

리덕스를 사용하는 프로젝트에서 action creators 와 reducer 를 좀더 쉽고 깔끔하게 작성 할 수 있게 해주는 라이브러리 이다.

##### Shout out to [Velopert](https://react.vlpt.us/using-typescript/05-ts-redux.html)

##### Installation
```sh
$ yarn add typesafe-actions
```


## Example 1).
#### pre-refactor
```typescript
// * [ Action Types ] *
/**
 * 뒤에 as const 를 붙여줌으로써 나중에 액션 객체를 만들때 action.type 의 값을 추론하는 과정에서
 * action.type 이 string 으로 추론되지 않고 'counter/INCREASE' 같이 실제 문자열 값으로 추론 되도록 해준다.
 */
const INCREASE = "counter/INCREASE" as const;
const DECREASE = "counter/DECREASE" as const;
const INCREASE_BY = "INCREASE_BY" as const;

// * [ Action Creators ] *
export const increase = () => ({
  type: INCREASE,
});

export const decrease = () => ({
  type: DECREASE,
});

export const increaseBy = (diff: number) => ({
  type: INCREASE_BY,
  payload: diff,
});

// * [ Action Creator Types ] *
/**
 * ReturnType<typeof ______> 는 특정 함수의 반환값을 추론해준다.
 * 위에서 액션타입을 선언 할 때 as const 를 하지 않으면 이 부분이 제대로 작동을 안한다.
 */
type CounterAction =
  | ReturnType<typeof increase>
  | ReturnType<typeof decrease>
  | ReturnType<typeof increaseBy>;

// * [ Initial State Type ] *
type InitialCounterState = {
  count: number;
};

// * [ Initial State ] *
const initialState: InitialCounterState = {
  count: 0,
};

// * [ Reducers ] *
export default function counterReducer(
  state: InitialCounterState = initialState,
  action: CounterAction
): InitialCounterState {
  switch (action.type) {
    case INCREASE:
      return {
        count: state.count + 1,
      };
    case DECREASE:
      return {
        count: state.count - 1,
      };
    case INCREASE_BY:
      return {
        count: state.count + action.payload,
      };
    default:
      return state;
  }
}
```

#### post-refactor - 1
> 아래 `reducer` 에는 `createReducer`를 사용해서 Object map style 형태로 작성해 주었다. 
> 
```typescript
import { createAction, ActionType, createReducer } from "typesafe-actions";

// * [ Action Types ] *
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";
const INCREASE_BY = "counter/INCREASE_BY";

// * [ Action Creators ] *
export const increase = createAction(INCREASE)();
export const decrease = createAction(DECREASE)();
export const increaseBy = createAction(INCREASE_BY)<number>(); // payload 타입을 Generics로 설정한다.

// * [ Action Object Types ] *
const actions = { increase, decrease, increaseBy }; // 모든 액션 생성함수들을 actions 객체 안에 넣는다.
type CounterAction = ActionType<typeof actions>; // ActionType 를 사용하여 모든 액션 객체들의 타입을 준비해줄 수 있다.

// * [ Initial State Type ] *
type InitialCounterState = {
  count: number;
};

// * [ Initial State ] *
const initialState: InitialCounterState = {
  count: 0,
};

// * [ Reducers ] *
/**
 * Object map style - https://github.com/piotrwitek/typesafe-actions#createreducer
 * createReducer 는 리듀서를 쉽게 만들어 주는 함수이다.
 * Generics로 리듀서에서 관리할 상태, 그리고 리듀서에서 처리 할 모든 액션 타입을 넣어야한다.
 */
const counterReducer = createReducer<InitialCounterState, CounterAction>(
  initialState,
  {
    [INCREASE]: (state) => ({ count: state.count + 1 }), // 액션을 참조할 필요가 없으면 parameter에 state 만 받아도 된다.
    [DECREASE]: (state) => ({ count: state.count - 1 }),
    [INCREASE_BY]: (state, action) => ({ count: state.count + action.payload }), // 액션의 타입을 유추 할 수 있다.
  }
);
export default counterReducer;
```

#### post-refactor - 2
> 아래 `reducer` 에는 `createReducer`를 사용해서 Chain API style 형식으로 작성해 주었다. 

```typescript
...
/**
 * Chain API style - https://github.com/piotrwitek/typesafe-actions#createreducer
 * createReducer 는 리듀서를 쉽게 만들어 주는 함수이다.
 * Generics로 리듀서에서 관리할 상태, 그리고 리듀서에서 처리 할 모든 액션 타입을 넣어야한다.
 */
const counterReducer = createReducer<InitialCounterState, CounterAction>(
  initialState
)
  .handleAction(increase, (state) => ({ count: state.count++ }))
  .handleAction(decrease, (state) => ({ count: state.count-- }))
  .handleAction(increaseBy, (state, action) => ({
    count: state.count + action.payload,
  }));
export default counterReducer;
```

## Example 2).
```tsx
import { ActionType, createReducer, createAction } from "typesafe-actions";
```
```tsx
// * Actions Types *
const ADD_TODO = "todos/ADD_TODO" as const;
const TOGGLE_TODO = "todos/TOGGLE_TODO" as const;
const REMOVE_TODO = "todos/REMOVE_TODO" as const;

let nextId = 1; // 새로운 항목을 추가 할 때 사용할 고유 ID 값

```

#### Before typesafe-action
```tsx
// * Action Creators *
export const addTodo = (text: string) => ({
  type: ADD_TODO,
  payload: {
    id: nextId++,
    text,
  },
});
export const toggleTodo = (id: number) => ({
  type: TOGGLE_TODO,
  payload: id,
});
export const removeTodo = (id: number) => ({
  type: REMOVE_TODO,
  payload: id,
});

// * Types for all Action Creators *
type TodosAction =
  | ReturnType<typeof addTodo>
  | ReturnType<typeof toggleTodo>
  | ReturnType<typeof removeTodo>;

// * Types for Todo Data *
export type Todo = {
  id: number;
  text: string;
  isDone: boolean;
};

// * Type for Initial State(above) *
export type TodosState = Todo[];

// * Initial State *
const initialState: TodosState = [];

// * Reducer *
export default function todoReducer(
  state: TodosState = initialState,
  action: TodosAction
): TodosState {
  switch (action.type) {
    case ADD_TODO:
      return state.concat({
        id: action.payload.id,
        text: action.payload.text,
        isDone: false,
      });
    case TOGGLE_TODO:
      return state.map((todo) =>
        todo.id === action.payload ? { ...todo, isDone: !todo.isDone } : todo
      );
    case REMOVE_TODO:
      return state.filter((todo) => todo.id !== action.payload);
    default:
      return state;
  }
}

```

#### After typesafe-action
> There's currently a syntax error in the reducer below by not using `createStandardAction` since it's been deprecated
```tsx
// * Action Creators *
// 파라미터를 기반해서 커스터마이징된 payload를 설정해주기 떄문에 createAction 함수를 사용한다.
export const addTodo = createAction(ADD_TODO, (action) => (text: string) =>
  action({ id: nextId++, text })
);
export const toggleTodo = createAction(TOGGLE_TODO)<number>(); // payload가 그대로 들어감
export const removeTodo = createAction(REMOVE_TODO)<number>();

// * Types for all Action Creators *
const actions = {
  addTodo,
  toggleTodo,
  removeTodo,
};
type TodosAction = ActionType<typeof actions>;

// * Types for Todo Data *
export type Todo = {
  id: number;
  text: string;
  isDone: boolean;
};

// * Type for Initial State(above) *
export type TodosState = Todo[];

// * Initial State *
const initialState: TodosState = [];

// * Reducer *
const todoReducer = createReducer<TodosState, TodosAction>(initialState, {
  [ADD_TODO]: (state, action) =>
    state.concat({
      ...action.payload, // id, text 를 이 안에 넣기
      done: false,
    }),
  // destructuring으로 payload 값의 이름을 바꿀수 있다.
  [TOGGLE_TODO]: (state, { payload: id }) =>
    state.map((todo) =>
      todo.id === id ? { ...todo, isDone: !todo.isDone } : todo
    ),
  [REMOVE_TODO]: (state, { payload: id }) =>
    state.filter((todo) => todo.id !== id),
});
export default todoReducer;
```
