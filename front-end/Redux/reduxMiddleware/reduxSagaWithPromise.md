# Handling promise with redux-saga

redux-thunk는 함수를 만들어서 비동기 작업을 하고 필요한 시점에 특정 액션을 디스패치 한다. 
반면, redux-saga는 비동기 작업을 처리 할 때 다른 방식으로 처리한다. redux-saga에서는 특정 액션을 모니터링 하도록 하고, 해당 액션이 주어지면 이에 따라 제너레이터 함수를 실행해서 비동기 작업을 처리 후 액션을 디스패치 한다.

#### 👉 아래 예제는 [redux-thunk](./reduxThunk.md) 파일을 참고.

## Get Started

#### modules/posts.js (redux modules posts)
```jsx
(...)
// * Action Creators *
// 아주 쉽게 thunk 함수를 만들 수 있게 되었다.
export const getPostsAsync = createPromiseThunk(GET_POSTS, postsAPI.getPosts);
export const getPostByIdAsync = createPromiseThunkById(
  GET_POST,
  postsAPI.getPostById
);
// ---- disregard codes above (related to redux-thunk)----

// * Redux Saga *
function* getPostsSaga() {
  try {
    const posts = yield call(postsAPI.getPosts); // call() 을 사용하면 특정 함수를 호출하고, 결과물이 반환 될 때까지 기다려줄 수 있다
    yield put({
      type: GET_POSTS_SUCCESS,
      payload: posts,
    });
  } catch (error) {
    yield put({
      type: GET_POSTS_FAIL,
      payload: error,
    });
  }
}

// 액션이 지니고 있는 값을 조회하고 싶으면 action을 파라미터로 받아와서 사용할 수 있다
function* getPostSagaById(action) {
  const param = action.payload;
  console.log("param: ", param);
  const id = action.meta;
  try {
    const post = yield call(postsAPI.getPostById, param); // API 함수에 넣어줘야 하는 인자는 call 함수 두번째 인바부터 순서대로 넣어주면 된다
    yield put({
      type: GET_POST_SUCCESS,
      payload: post,
      meta: id,
    });
  } catch (error) {
    yield put({
      type: GET_POST_FAIL,
      payload: error,
      meta: id,
    });
  }
}

// * Combine Sagas *
export function* postsSaga() {
  yield takeEvery(GET_POSTS, getPostsSaga);
  yield takeEvery(GET_POST, getPostByIdSaga);
}

// export const clearPost = () => ({ type: CLEAR_POST });
export const goHomePage = () => (dispatch, getState, { history }) => {
  history.push("/");
};
(...)
```
redux-saga를 사용하면서 순수 액션 객체를 반환하는 액션 생성 함수로 구현하게 되었다. 액션을 모니터링해서 특정 액션이 발생했을 때 호출할 사가 함수에서는 파라미터를 해당 액션을 받아올수 있다. 예를 들어 위에 `getPostByIdSaga`함수의 경우 액션을 파라미터로 받아와서 해당 액션의 `id`값을 참조할 수 있다.

#### modules/index.js (redux modules main index)
```jsx
import { combineReducers } from "redux";
import { all } from "redux-saga/effects";

// * Root Reducer *
import counterReducer, { counterSaga } from "./counter";
import postsReducer, { postsSaga } from "./posts"; // <-- 여기 추가

const rootReducer = combineReducers({
  counterReducer,
  postsReducer,
});

export function* rootSaga() {
  yield all([counterSaga(), postsSaga()]); // all() 은 배열 안에 여러 사가를 동시에 실행시켜 준다.
}

export default rootReducer;
```

## 리팩토링

#### lib/asyncUtils.js (utility function)
> `createPromiseThunk`, `createPromiseThunkById` 는 주석 처리됨

```jsx
import { call, put } from "redux-saga/effects";
(...)
// * Redux Saga *
// - Promise를 기다렸다가 결과를 디스패치 하는 사가
export const createPromiseSaga = (type, promiseCreator) => {
  const [SUCCESS, FAIL] = [`${type}_SUCCESS`, `${type}_FAIL`];

  return function* saga(action) {
    try {
      // 재사용성을 위하여 promiseCreator 의 파라미터엔 action.payload 값을 넣도록 설정한다.
      const payload = yield call(promiseCreator, action.payload);
      yield put({ type: SUCCESS, payload });
    } catch (error) {
      yield put({ type: FAIL, payload: error, error: true });
    }
  };
};

// - 특정 id의 데이터를 조회하는 용도로 사용하는 사가
// - API를 호출 할 때 파라미터는 action.payload를 넣고, id 값을 action.meta로 설정한다.
export const createPromiseSagaById = (type, promiseCreator) => {
  const [SUCCESS, FAIL] = [`${type}_SUCCESS`, `${type}_FAIL`];

  return function* saga(action) {
    const id = action.meta;
    try {
      const payload = yield call(promiseCreator, action.payload);
      yield put({ type: SUCCESS, payload, meta: id });
    } catch (error) {
      yield put({ type: FAIL, error, meta: id });
    }
  };
};
(...)
```
이제 사가를 통해서 비동기 작업을 처리 할 때 API 함수의 인자는 액션에서부터 참조한다. 액션 객체에서 사용 할 함수의 인자의 이름은 `payload` 로 통일 시키도록 하겠다. 그리고, 특정 id를 위한 비동기작업을 처리하는 `createPromiseSagaById`와 `createPromiseThunkById` 에서는 id 값을 `action.meta` 에서 참조하도록 하겠다.

#### modules/posts.js (redux modules)

```jsx
(...)
// * Action Creators *
// 아주 쉽게 thunk 함수를 만들 수 있게 되었다.
// export const getPostsAsync = createPromiseThunk(GET_POSTS, postsAPI.getPosts);
// export const getPostByIdAsync = createPromiseThunkById(
//   GET_POST,
//   postsAPI.getPostById
// );
export const getPostsAsync = () => ({ type: GET_POSTS });
export const getPostByIdAsync = (id) => ({
  type: GET_POST,
  payload: id,
  meta: id,
});

// * Redux Saga * <-- 요기 리팩토링
export const getPostsSaga = createPromiseSaga(GET_POSTS, postsAPI.getPosts);
export const getPostSagaById = createPromiseSagaById(
  GET_POST,
  postsAPI.getPostById
);

// * Combine Sagas *
export function* postsSaga() {
  yield takeEvery(GET_POSTS, getPostsSaga);
  yield takeEvery(GET_POST, getPostSagaById);
}
(...)
```