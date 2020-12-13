# Redux Middleware

미들웨어는 하나의 함수다. 함수를 연달아서 두번 리턴하는 함수.

```javascript
ES6
const middleware = (store) => (next) => (action) => {
    // task to do...
}

ES5
function middleware (store){
    return function (next){
        return function (action){
            // task to do...
        }
    }
}
```
첫번째 `store`는 `redux store instance`이다. 이 안에 `dispatch`, `getState`, `subscribe` 내장함수들이 들어있다.

두번째 `next`는 액션을 다음 미들웨어에게 전달하는 함수다. `next(action)`와 같은 형태로 사용한다. 만약 다음 미들웨어가 없으면 리듀서에게 `action`을 전달해준다. 또한 만약 `next`를 호출하지 않게되면 `action`이 무시처리되어 리듀서에게 전달되지 않는다.

세번째 `action`은 현재 처리하고 있는 액션객체다.

## Getting Started

예제. 리액트 컴포넌트는 사실 increase/decrease 해서 number 보여주는 컴포넌트는 쉽게 만들수 있으니 그 부분은 스킵하겠다.

#### counter.js
먼저 아래와 같이 리덕스 파일을 만들어 준다.
```jsx
// * Action Types *
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

// * Action Creators *
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// * Initial State *
const initialState = 0;

// * Reducer *
export default function counterReducer(state = initialState, action) {
  switch (action.type) {
    case INCREASE:
      return state + 1;
    case DECREASE:
      return state - 1;
    default:
      return state;
  }
}
```

#### myLogger.js (middleware)
```jsx
const myLogger = (store) => (next) => (action) => {
  console.log(action); // 먼저 액션을 출력한다.
  const result = next(action); // 다음 미들웨어 (또는 리듀서) 에게 전달한다.
  console.log("\t", store.getState());
  return result; // 이 반환값이 dispatch(action)의 결과물이 된다. 기본은 undefined 다.
};

export default myLogger;
```

#### index.js (react component)
```jsx
import React from "react";
import ReactDOM from "react-dom";
import { combineReducers, createStore, applyMiddleware } from "redux";
import { Provider } from "react-redux";

import App from "./App";
import counterReducer from "./modules/counter";
import myLogger from "./middlewares/myLogger";

// * Root Reducer *
const rootReducer = combineReducers({
  counterReducer,
});

const store = createStore(rootReducer, applyMiddleware(myLogger)); // 여러개의 미들웨어를 받을수 있다.

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);


```

자 이제, <button>+</button> 버튼을 눌렀을때 `increase` 액션 함수가, <button>-</button> 버튼을 눌렀을때 `decrease` 액션 함수가 호출되는 컴포넌트를 만들자.

<button>+</button> 두번 누르면
```
{type: "counter/INCREASE"}
{counterReducer: 1}
{type: "counter/INCREASE"}
{counterReducer: 2}
```
<button>-</button> 한번 누르면
```
{type: "counter/DECREASE"}
{counterReducer: 1}
```