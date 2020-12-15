# redux-thunk

## Understanding redux-thunk
리덕스에서 비동기 작업을 처리 할 때 가장 많이 사용되는 미들웨어다. 이 미들웽어를 사용하면 **액션객체가 아닌 함수를 디스패치 할 수 있다**. 리덕스의 창시자인 Dan Abramov가 만들었단다.
redux-thunk의 [코드](https://github.com/reduxjs/redux-thunk/blob/master/src/index.js)는 고작 14줄 밖에 안되지만 엄청난 다운로드 수를 가지고 있다.

함수를 디스패치 할 때에는, 해당 함수에서 `dispatch` 와 `getState` 를 파라미터로 받아와야 한다. 이 함수를 **만들어주는 함수를 thunk** 라고 부른다.

### thunk의 사용 예시
```jsx
const getComments = () => (dispatch, getState) => {
    // 이 안에서 dispatch 도 할수 있고,
    // getState을 사용해서 현재 상태를 볼 수 도 있다. 👍
    const id = getState.post.activeId;

    // 요청이 시작했음을 알리는 액션
    dispatch({type: 'GET_COMMENTS_START'});

    // 댓글을 조회하는 프로미스를 반환하는 getComments 가 있다고 가정해보자.
    api
    .getComments(id)
    .then((comments) => dispatch({
        type: 'GET_COMMENTS_SUCCESS',
        id,
        comments
    }))
    .catch((e) => dispatch({type: 'GET_COMMENTS_ERROR', error: e}));
}
```

또한 thunk 함수에서 async/await 을 사용해도 좋다.
```jsx
const getComments = () => async (dispatch, getState) => {
    const id = getState().post.activeId;
    dispatch({type: 'GET_COMMENTS_START'});

    try {
        const comments = await api.getComments(id);
        dispatch({type: 'GET_COMMENTS_SUCCESS', id, comments})
    } catch (e) {
        dispatch({type: 'GET_COMMETNS_FAIL', error: e});
    }
}
```

## Installing [redux-thunk](https://github.com/reduxjs/redux-thunk)
```sh
$ npm install redux-thunk

$ yarn add redux-thunk
```

#### Usage
```jsx
import ReduxThunk from 'redux-thunk';
```

#### Set up example
*[What is middleware](./whatIsMiddleware.md)에 예제가 비슷하다. 참고~*

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { combineReducers, createStore, applyMiddleware } from "redux";
import { Provider } from "react-redux";
import logger from "redux-logger";
import { composeWithDevTools } from "redux-devtools-extension";
import ReduxThunk from "redux-thunk";

import App from "./App";
import counterReducer from "./modules/counter";

// * Root Reducer *
const rootReducer = combineReducers({
  counterReducer,
});

const store = createStore(
  rootReducer,
  composeWithDevTools(applyMiddleware(ReduxThunk, logger)) // logger를 사용할 경우 맨 마지막으로 와야한다.
); // 여러개의 미들웨어를 적용 할 수 있다.

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

#### counter.js (redux module)
```jsx
// * Action Types *
const INCREASE = "counter/INCREASE";
const DECREASE = "counter/DECREASE";

// * Action Creators *
export const increase = () => ({ type: INCREASE });
export const decrease = () => ({ type: DECREASE });

// 아래의 async 함수들을 사용하자.
// getState를 안쓰면 굳이 파라미터로 받아올 필요는 없다.
export const increaseAsync = () => (dispatch) => {
  setTimeout(() => dispatch(increase()), 1000);
};
export const decreaseAsync = () => (dispatch) => {
  setTimeout(() => dispatch(decrease()), 1000);
};

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

똑같이 <button>+</button> 버튼을 눌렀을때 `increaseAsync` 액션 함수가, <button>-</button> 버튼을 눌렀을때 `decreaseAsync` 액션 함수가 호출되는 컴포넌트를 만들자.
그러면 1초후에 `state`가 업데이트 되는걸 볼수 있을거다(redux devtool로 말이다ㅎ).

## Handling Promise(async/await) with redux-thunk
#### posts.js (fake api)
```jsx
// n 밀리세컨드 동안 기다리는 프로미스를 만들어주는 함수
const sleepPromise = (seconds) =>
  new Promise((resolve, reject) => setTimeout(resolve, seconds));

// 가짜 포스트 목록 데이터
const posts = [
  {
    id: 1,
    title: "리덕스 미들웨어를 배워봅시다",
    body: "리덕스 미들웨어를 직접 만들어보면 이해하기 쉽죠.",
  },
  {
    id: 2,
    title: "redux-thunk를 사용해봅시다",
    body: "redux-thunk를 사용해서 비동기 작업을 처리해봅시다!",
  },
  {
    id: 3,
    title: "redux-saga도 사용해봅시다",
    body:
      "나중엔 redux-saga를 사용해서 비동기 작업을 처리하는 방법도 배워볼 거예요.",
  },
];

// 포스트 목록을 가져오는 비동기 함수
export const getPosts = async () => {
  await sleepPromise(1000); // wait 1 second.
  return posts; // posts 배열 반환.
};

// ID로 포스트를 조회하는 비동기 함수
export const getPostById = async (id) => {
  await sleepPromise(1000); // wait 1 second.
  return posts.find((post) => post.id === id); // id로 찾아서 반환.
};
```
위엔 가짜 포스터 목록을 반환 해주는 파일이다.

#### posts.js (redux module)
```jsx
import * as postsAPI from "../api/posts";

// * Action Types *
// 포스트 여러개
const GET_POSTS = "posts/GET_POSTS";
const GET_POSTS_SUCCESS = "posts/GET_POSTS_SUCCESS";
const GET_POSTS_FAIL = "posts/GET_POSTS_FAIL";

// 포스트 하나
const GET_POST = "/posts/GET_POST";
const GET_POST_SUCCESS = "/posts/GET_POST_SUCCESS";
const GET_POST_FAIL = "/posts/GET_POST_FAIL";

// * Action Creators *
// thunk를 사용할 때, 꼭 모든 액션들에 대해 액션 생성함수를 만들 필요는 없다.
// 그냥 thunk 함수에서 바로 액션 객체를 만들어줘도 괜찮다.
export const getPostsAsync = () => async (dispatch) => {
  dispatch({ type: GET_POSTS });

  try {
    const retrievedPosts = await postsAPI.getPosts(); // API 호출
    dispatch({ type: GET_POSTS_SUCCESS, posts: retrievedPosts }); // Success
  } catch (e) {
    dispatch({ type: GET_POSTS_FAIL, error: e }); // Fail
  }
};

export const getPostByIdAsync = (id) => async (dispatch) => {
  dispatch({ type: GET_POST });

  try {
    const retrievedPost = await postsAPI.getPosts(id);
    dispatch({ type: GET_POST_SUCCESS, post: retrievedPost });
  } catch (e) {
    dispatch({ type: GET_POST_FAIL, error: e });
  }
};

// * Initial State *
const initialState = {
  posts: {
    loading: false,
    data: null,
    error: null,
  },
  post: {
    loading: false,
    data: null,
    error: null,
  },
};

// * Reducer *
export default function postsReducer(state = initialState, action) {
  switch (action.type) {
    case GET_POSTS:
      return {
        ...state,
        posts: {
          loading: true,
          data: null,
          error: null,
        },
      };
    case GET_POSTS_SUCCESS:
      return {
        ...state,
        posts: {
          loading: false,
          data: action.posts,
          error: null,
        },
      };
    case GET_POSTS_FAIL:
      return {
        ...state,
        posts: {
          loading: false,
          data: null,
          error: action.error,
        },
      };
    case GET_POST:
      return {
        ...state,
        post: {
          loading: true,
          data: null,
          error: null,
        },
      };
    case GET_POST_SUCCESS:
      return {
        ...state,
        post: {
          loading: false,
          data: action.post,
          error: null,
        },
      };
    case GET_POST_FAIL:
      return {
        ...state,
        post: {
          loading: false,
          data: null,
          error: action.error,
        },
      };
    default:
      return state;
  }
}
```

일단, 위의 코드는 반복되는게 오지게 많으니 조금 리팩토링을 해주도록 하자.

### refactor 1 -

#### asyncUtils.js (utils)
```jsx
// * Promise에 기반한 Thunk를 만들어주는 함수이다. *
export const createPromiseThunk = (type, promiseCreator) => {
  const [SUCCESS, FAIL] = [`${type}_SUCCESS`, `${type}_FAIL`];

  // 이 함수는 promiseCreator가 단 하나의 파라미터만 받는다는 가정하에 작성된거다.
  // 만약에 여러 종류의 파라미터를 전달해야하는 상황에서는 객체 타입의 파라미터를 받아오면 된다.
  // Ex). writeComment({postId: 1, text: 'some comments'});
  return (param) => async (dispatch) => {
    // 요청 시작
    dispatch({ type, param });
    try {
      const payload = await promiseCreator(param); // 결과물의 이름을 payload로 통일시킨다.
      dispatch({ type: SUCCESS, payload }); // Success
    } catch (e) {
      dispatch({ type: FAIL, payload: e, error: true }); // Fail
    }
  };
};

// * 리듀서에서 사용 할 수 있는 여러 유틸 함수다. *
export const reducerUtils = {
  // * Initial *
  // 초기 상태의 값은 기본적으로 null 이지만 바꿀수도 있다.
  initial: (initialData = null) => ({
    loading: false,
    data: initialData,
    error: null,
  }),
  // * Loading *
  // prevState의 경우 기본값은 null 이지만
  // 따로 값을 지정하면 null로 바꾸지 않고 다른 값을 유지시킬 수 있다.
  loading: (prevState = null) => ({
    loading: true,
    data: prevState,
    error: null,
  }),
  // * Success *
  success: (payload) => ({
    loading: false,
    data: payload,
    error: null,
  }),
  // * Fail *
  fail: (error) => ({
    loading: false,
    data: null,
    error,
  }),
};

```
이 util 을 사용해서 리팩토링을 아래와 같이 해보자.
#### posts.js (redux module) - after refactor
```jsx
import * as postsAPI from "../api/posts";
import { createPromiseThunk, reducerUtils } from "../lib/asyncUtils";

...

// * Action Creators *
// 아주 쉽게 thunk 함수를 만들 수 있게 되었다.
export const getPostsAsync = createPromiseThunk(GET_POSTS, postsAPI.getPosts);
export const getPostByIdAsync = createPromiseThunk(
  GET_POST,
  postsAPI.getPostById
);

// * Initial State *
// initialState 쪽도 반복되는 코드를 initial() 함수를 사용해서 리팩토링 됬다.
const initialState = {
  posts: reducerUtils.initial(),
  post: reducerUtils.initial()
};

// * Reducer *
export default function postsReducer(state = initialState, action) {
  switch (action.type) {
    case GET_POSTS:
      return {
        ...state,
        posts: reducerUtils.loading(),
      };
    case GET_POSTS_SUCCESS:
      return {
        ...state,
        posts: reducerUtils.success(action.payload), // action.posts -> action.payload
      };
    case GET_POSTS_FAIL:
      return {
        ...state,
        posts: reducerUtils.fail(action.error),
      };
    case GET_POST:
      return {
        ...state,
        post: reducerUtils.loading(),
      };
    case GET_POST_SUCCESS:
      return {
        ...state,
        post: reducerUtils.success(action.payload), // action.posts -> action.payload
      };
    case GET_POST_FAIL:
      return {
        ...state,
        post: reducerUtils.fail(action.error),
      };
    default:
      return state;
  }
}
```

### refactor 2 -
여기서 조금더 리팩토링을 하여 반복되는 코드를 줄이면 다음과 같이 할 수 있다. 아래 파일은 위에 있는 같은 asyncUtils.js 파일에 추가한 코드다.
#### asyncUtils.js (utils)
```jsx
...
// 비동기 관련 액션을을 처리하는 리듀서를 만들어준다.
/**
 * type: 액션의 타입
 * key: 상태의 key (i.e. posts, post)
 */
export const handlAsyncActions = (type, key) => {
  const [SUCCESS, FAIL] = [`${type}_SUCCESS`, `${type}_FAIL`];
  return (state, action) => {
    switch (action.type) {
      case type:
        return {
          ...state,
          [key]: reducerUtils.loading(),
        };
      case SUCCESS:
        return {
          ...state,
          [key]: reducerUtils.success(action.payload),
        };
      case FAIL:
        return {
          ...state,
          [key]: reducerUtils.fail(action.error),
        };
      default:
        return state;
    }
  };
};
```

#### posts.js (redux module) - after more refactor
```jsx
...
// * Reducer *
export default function postsReducer(state = initialState, action) {
  switch (action.type) {
    case GET_POSTS:
    case GET_POSTS_SUCCESS:
    case GET_POSTS_FAIL:
      return handlAsyncActions(GET_POSTS, "posts")(state, action);

    case GET_POST:
    case GET_POST_SUCCESS:
    case GET_POST_FAIL:
      return handlAsyncActions(GET_POST, "post")(state, action);
    default:
      return state;
  }
}
```
위에 `return` 하는 함수가 살짝 헷갈리긴 하는데, 심히 간단하다. asyncUtils.js 파일 안에 `handlAsyncAction` 함수는 `type` 과 `key` 를 파라미터로 받아오는데 바로 위에 `GET_POST` 나 `'post'` 같은 녀석들을 받고, 그 함수는 또 다른 함수를 `return` 하는데 그 함수 역시 `state` 와 `action` 을 파라미터로 받는다. 저걸 조금 풀어 쓰면 다음과 같다.
```jsx
    ...
    case GET_POST:
    case GET_POST_SUCCESS:
    case GET_POST_FAIL:
      //   return handlAsyncActions(GET_POST, "post")(state, action);
      const postReducer = handlAsyncActions(GET_POST, "post");
      return postReducer(state, action);
    default:
```

위와 같이 리덕스를 잘 짰으니 이제 리액트 컴포넌트를 만들어보자

#### PostListContainer.js (container)
```jsx
import { useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";

import { getPostsAsync } from "../modules/posts";

export default function PostListContainer() {
  const { data, loading, error } = useSelector(
    (state) => state.postsReducer.posts
  );
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(getPostsAsync());
  }, [dispatch]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error occurred!</div>;
  if (!data) return null;

  return (
    <ul>
      {data.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```
그리고 간단하게 pages라는 폴더를 만들고 그 안에 아래와 같이 프레젠테이셔널 컴포넌트를 넣자.
#### PostListPage.js (pages/React component)
```jsx
import PostListContainer from "../containers/PostListContainer";

export default function PostListPage() {
  return <PostListContainer />;
}
```
#### PostPage.js (pages/React component)
```jsx
import PostContainer from "../containers/PostContainer";

export default function PostPage({ match }) {
  const { id } = match.params;

  return <PostContainer postId={+id} />;
}
```

이제, react-router-dom 을 설치하고 아래와 같이 index와 App 컴포넌트에도 넣어주장.
#### index.js (main)
```jsx
import React from "react";
import ReactDOM from "react-dom";
...
import { BrowserRouter } from "react-router-dom";
...
ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter>
      <Provider store={store}>
        <App />
      </Provider>
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById("root")
);
```
#### App.js (main)
```jsx
import { Route } from "react-router-dom";

import PostListPage from "./pages/PostListPage";
import PostPage from "./pages/PostPage";

function App() {
  return (
    <>
      <Route path='/' component={PostListPage} />
      <Route path='/:id' component={PostPage} />
    </>
  );
}

export default App;
```

그러면 이제 뭐 아주 자연스럽게 작동할거다. 물론 링크에 드러갈 때마다 계속 1초동안 로딩되는건 있지만 이건 벨로퍼트님의 다음 강의가 있으니 그걸 보자 =)

---

Super shout out to [Velopert](https://react.vlpt.us/redux-middleware/05-redux-thunk-with-promise.html)