# redux-saga middleware

> 작업이 까다로울땐 사가 함수를 직접 작성하는것을 추천한다. 반면, 작업이 간단한 비동기 작업 처리를 할때는 redux-thunk를 사용해서 반복되는 로직을 함수화 해서 재사용여 훨씬 깔끔한 코드를 작성하는걸 추천한다.

[redux-saga documentation](https://github.com/redux-saga/redux-saga)

액션을 모니터링하고 있다가, 특정 액션이 발생하면 이에 따라 특정 작업을 하는 방식으로 사용한다. 여기서 특정 작업이란 자바스크립트를 실행하는 것 일수도 있고, 다른 액션을 디스패치 하는 것 일수도 있고, 현태 상태를 불러오는 것 일수도 있다.
redux-saga는 redux-thunk로 못한 다양한 작업들을 처리 할 수 있다. ex).

1. 비동기 작업을 할 때 기존 요청을 취소 처리 할 수 있다.
2. 특정 액션이 발생했을 때 이에 따라 다른 액션이 디스패치되게끔 하거나, 자바스크립트 코드를 실행 할 수 있다.
3. 웹소켓을 사용하는 경우 Channel 이라는 기능을 사용하여 더욱 효율적으로 코드를 관리 할 수 있다. [참고](https://medium.com/@pierremaoui/using-websockets-with-redux-sagas-a2bf26467cab)
4. API 요청이 실패했을 때 재요청하는 작업을 할 수 있다.

redux-saga는 다양한 상황에 사용될 수 있는 만큼, 제공되는 기능도 많고, 사용방법 진입장벽이 꽤나 크다. 특히 개인적으로 처음 보는 [Generator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator)라는 문법을 사용한다. 일단 먼저 이것부터 집고 넘어가자.
#### 👉 [Generator Operator](./../../JavaScript/generatorObject.md) 파일을 참고하자. 🤘

## Installation

```sh
$ yarn add redux-saga
```

## Getting Started

#### 👉 아래 예제는 [redux-thunk](./reduxThunk.md) 파일 참고

#### modules/counter.js (redux module - actionCreator/reducer/state)
```jsx
import { delay, put, takeEvery, takeLatest } from "redux-saga/effects";

// * Action Types *
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";
const INCREASE_ASYNC = "counter/INCREASE_ASYNC";
const DECREASE_ASYNC = "counter/DECREASE_ASYNC";

// * Action Creators *
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });
export const increaseAsync = () => ({ type: INCREASE_ASYNC });
export const decreaseAsync = () => ({ type: DECREASE_ASYNC });

// * Redux Saga Generator Function *
// redux-saga calls generator functions as "saga"
function* increaseSaga() {
  yield delay(1000); // waits for 1 second(s)
  yield put(increase()); // put() method dispatches a certain action
}
function* decreaseSaga() {
  yield delay(1000); // waits for 1 second(s)
  yield put(decrease()); // put() method dispatches a certain action
}

export function* counterSaga() {
  yield takeEvery(INCREASE_ASYNC, increaseSaga); // 모든 INCREASE_ASYNC 액션을 처리
  yield takeLatest(DECREASE_ASYNC, decreaseSaga); // 가장 마지막으로 디스패치된 DECREASE_ASYNC 액션만을 처리
}

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

`redux-saga/effects` 에는 다양한 유틸함수들이 들어있다. 여기서 `put`이라는 유틸 함수는 새로운 액션을 디스패치 할 수 있다.
또한 `takeEvery`, `takeLatest` 라는 유틸함수도 들어있는데, 얘네는 액션을 모니터링 하는 함수다.
`takeEvery` 유틸함수는 특정 액션 타입에 대하여 디스패치되는 모든 액션들을 처리한다.
`takeLatest` 유틸함수는 특정 액션 타입에 대하여 디스패치된 가장 마지막 액션만을 처리하는 함수다. 예를 들어 특정 액션을 처리하고 있는 동안 동일한 타입의 새로운 액션이 디스패치되면 기존에 하던 작업을 무시하고 새로운 작업을 시작한다.

#### modules/index.js (redux module - root reducer)
> [redux-thunk](./reduxThunk.md) 파일 안에선 리액트 index.js 파일안에 root reducer를 만들었는데 아예 modules 폴더 안에 이렇게 index 파일을 만들어서 root reducer를 만들어 주면 좀더 깔끔하다.
```jsx
import { combineReducers } from "redux";
import { all } from "redux-saga/effects";

// * Root Reducer *
import counterReducer, { counterSaga } from "./counter";
import postsReducer from "./posts";

const rootReducer = combineReducers({
  counterReducer,
  postsReducer,
});

export function* rootSaga() {
  yield all([counterSaga()]);
}

export default rootReducer;
```
위와 같이 rootSaga 제너레이터 함수를 만들어 주고 다른 파일에서도 사용할 수 있게 export 해준다.
`all()` 유틸함수는 배열 안의 여러 사가를 동시에 실행 시켜준다.

#### index.js (main React index)

이제 main index.js 파일 안에 있는 리덕스 스토어에 redux-saga 미들웨어를 적용해보자.
```jsx
import React from "react";
import ReactDOM from "react-dom";
import { createStore, applyMiddleware } from "redux";
import { Provider } from "react-redux";
import logger from "redux-logger";
import { composeWithDevTools } from "redux-devtools-extension";
import ReduxThunk from "redux-thunk";
import { Router } from "react-router-dom";
import { createBrowserHistory } from "history";
import createSagaMiddleware from "redux-saga"; // <-- 요기 추가

import rootReducer, { rootSaga } from "./modules";
import App from "./App";

const customHistory = createBrowserHistory();
const sagaMiddleware = createSagaMiddleware(); // <-- create saga middleware

// * store instance *
const store = createStore(
  rootReducer,
  composeWithDevTools(
    applyMiddleware(
      ReduxThunk.withExtraArgument({ history: customHistory }),
      sagaMiddleware, // <-- apply saga middlware here
      logger // logger ALWAYS comes last when using it
    )
  )
);

// * Run Root Saga (must run this code after the store's been created) *
sagaMiddleware.run(rootSaga);

ReactDOM.render(
  <React.StrictMode>
    <Router history={customHistory}>
      <Provider store={store}>
        <App />
      </Provider>
    </Router>
  </React.StrictMode>,
  document.getElementById("root")
);
```

#### App.js (main React App component)
```jsx
import { Route } from "react-router-dom";

import CounterContainer from "./containers/CounterContainer";
import PostListPage from "./pages/PostListPage"; // not needed
import PostPage from "./pages/PostPage"; // not needed

function App() {
  return (
    <>
      <CounterContainer />
      <Route path='/' exact component={PostListPage} />
      <Route path='/:id' component={PostPage} />
    </>
  );
}

export default App;
```

자, 이제 <button>+</button> 버튼과 <button>-</button> 버튼을 눌러보자.
<button>+</button> 버튼을 누를시 count 숫자가 누른만큼 올라가는걸 볼수 있다. 바로 `takeEvery` 유틸 함수 안에서 모든 INCREASE_ASYNC 액션을 다 처리하기 때문이다.
반면에 <button>-</button> 버튼을 누를시엔 count 숫자가 내려가긴 하지만 spamming click을 해도 한번밖에 내려가질 않는다. `takeLatest` 유틸 함수 에서 마지막 액션만을 처리하기 때문이다.
겁나 신기방기 해부러ㅎㅎ
