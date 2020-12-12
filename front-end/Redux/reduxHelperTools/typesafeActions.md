# Typesafe-actions

#### [Github documentation](https://github.com/piotrwitek/typesafe-actions)

리덕스를 사용하는 프로젝트에서 action creators 와 reducer 를 좀더 쉽고 깔끔하게 작성 할 수 있게 해주는 라이브러리 이다.

##### Shout out to [Velopert](https://react.vlpt.us/using-typescript/05-ts-redux.html)

##### Installation
```sh
$ yarn add typesafe-actions
```

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