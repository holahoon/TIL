# redux-saga에서 라우터 연동하기

예를 들어 로그인 요청을 할 때 성공할 시 특정 주소로 이동시키고, 실패 할 시엔 그대로 유지하는 기능을 구현을 해야될 때 컨테이너 컴포넌트에서 `withRouter`를 사용해서 구현을 하는 것 보다 사가 내부에서 처리하는게 훨씬 편하다.

#### 👉 아래 예제는 [redux-saga](reduxSaga.md), [redux-saga with promise](./reduxSagaWithPromise.md) 파일을 참고.

## Get Started

#### index.js (main React index)
```jsx
(...)
const customHistory = createBrowserHistory();
const sagaMiddleware = createSagaMiddleware({
  context: {
    history: customHistory,
  },
}); // create saga middleware
(...)
```
위와 같이 미들웨어를 만들때 `context`를 설정해주면 추후 사가에서 `getContext` 함수를 통해서 조회할 수 있다.

#### modules/posts.js (redux modules)
```jsx
(...)
const GO_HOME = "posts/GO_HOME";

export const getPostsSaga = createPromiseSaga(GET_POSTS, postsAPI.getPosts);
export const getPostSagaById = createPromiseSagaById(
  GET_POST,
  postsAPI.getPostById
);
export const goHomePage = () => ({ type: GO_HOME }); // <-- 요기 추가

function* goHomeSaga() {
  const history = yield getContext("history");
  history.push("/");
}

// * Combine Sagas *
export function* postsSaga() {
  yield takeEvery(GET_POSTS, getPostsSaga);
  yield takeEvery(GET_POST, getPostSagaById);
  yield takeEvery(GO_HOME, goHomeSaga);
}

// export const clearPost = () => ({ type: CLEAR_POST });
// export const goHomePage = () => (dispatch, getState, { history }) => {
//   history.push("/");
// };
(...)
```