# Using router in redux-thunk

프로젝트를 개발하다보면, thunk 함수 내에서 라우터를 사용해야 할 때도 있다. ex). 로그인 요청을 하는데 로그인이 성공 할 시 / 경로로 이동시키고, 실패 할 시 경로를 유지 하는 형태로 구현 할 수도 있다.
뭐, 사실 컨테이너 컴포넌트내에서 그냥 [withRouter](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/withRouter.md)를 사용해서 props로 `history` 를 가져와서 사용해도 아무 상관이 없다. 그런데 thunk 함수 안에서 처리하면 코드가 더 깔끔해질 수 있다. 갠취

## customHistory 만들어서 적용 해보자.

> [redux-thunk](./reduxThunk.md)를 참고하자.

### [index.js] (React main index)
```jsx
import React from "react";
import ReactDOM from "react-dom";
import { combineReducers, createStore, applyMiddleware } from "redux";
import { Provider } from "react-redux";
import logger from "redux-logger";
import { composeWithDevTools } from "redux-devtools-extension";
import ReduxThunk from "redux-thunk";
import { Router } from "react-router-dom";
import { createBrowserHistory } from "history"; // <-- 요기 추가

import App from "./App";
import counterReducer from "./modules/counter";
import postsReducer from "./modules/posts";

// * Root Reducer *
const rootReducer = combineReducers({
  counterReducer,
  postsReducer,
});

const customHistory = createBrowserHistory(); // <-- 요기 추가

// * store instance *
const store = createStore(
  rootReducer,
  composeWithDevTools(
    applyMiddleware(
      ReduxThunk.withExtraArgument({ history: customHistory }), // <-- 요기 추가
      logger
    )
  )
);

ReactDOM.render(
  <React.StrictMode>
    <Router history={customHistory}> // <-- 이게 BrowserRouter 에서 Router로 바뀜
      <Provider store={store}>
        <App />
      </Provider>
    </Router>
  </React.StrictMode>,
  document.getElementById("root")
);
```
위에 사용되는 `import { createBrowserHistory } from "history";`는 [react router](https://reactrouter.com/web/api/Router) 를 참고하자.
그리고 redux-thunk의 `withExtraArgument`를 사용하면 thunk 함수에서 사전에 정해준 값들을 참조 할수 있다.

## 홈 화면으로 가는 thunk 만들어보자.

### [Posts.js] (redux module)
```jsx
...
// * Action Creators *
export const goHomePage = () => (dispatch, getState, { history }) => {
  history.push("/");
};
...
```
// 3번째 인자를 사용하면 `withExtraArgument` 에서 넣어준 값들을 사용 할 수 있다.


### [PostContainer.js] (React container)
```jsx
import React, { useEffect } from "react";
import { useSelector, useDispatch } from "react-redux";

import { getPostByIdAsync, goHomePage } from "../modules/posts"; // <-- 요기 추가

export default function PostContainer({ postId }) {
  const { data, loading, error } = useSelector(
    (state) => state.postsReducer.post[postId]
  ) || {
    loading: false,
    data: null,
    error: null,
  }; // 아예 데이터가 존재하지 않을 때가 있으므로, 비구조화 할당이 오류나지 않도록
  const dispatch = useDispatch();

  useEffect(() => {
    // if (data) return;
    dispatch(getPostByIdAsync(postId));
    // return () => {
    //   dispatch(clearPost());
    // };
  }, [dispatch, postId]);

  if (loading && !data) return <div>Loading...</div>;
  if (error) return <div>Error Occurred!</div>;
  if (!data) return null;

  const { title, body } = data;
  return (
    <div>
      <button onClick={() => dispatch(goHomePage())}>Go back home</button> // <-- 여기 추
      <h1>{title}</h1>
      <p>{body}</p>
    </div>
  );
}
```
버튼을 누르면 `goHomePage()` 을 디스패치 함으로 다시 `/` 으로 가게 된다.
위의 예시는 단순하게 클릭하면 홈으로 돌아가게 했지만, 실제 프로젝트에서는 `getState()` 를 사용하여 현재 리덕스 스토어의 상태를 확인해서 조건부로 이동을 하거나, 특정 API를 호출하여 성공했을 시에만 이동을 하는 형식으로 구현할 수 있다.