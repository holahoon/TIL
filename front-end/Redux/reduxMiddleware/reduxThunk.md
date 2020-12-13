# redux-thunk

## Understanding redux-thunk

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

