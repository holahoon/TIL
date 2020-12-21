# redux-saga middleware

[redux-saga documentation](https://github.com/redux-saga/redux-saga)

액션을 모니터링하고 있다가, 특정 액션이 발생하면 이에 따라 특정 작업을 하는 방식으로 사용한다. 여기서 특정 작업이란 자바스크립트를 실행하는 것 일수도 있고, 다른 액션을 디스패치 하는 것 일수도 있고, 현태 상태를 불러오는 것 일수도 있다.
redux-saga는 redux-thunk로 못한 다양한 작업들을 처리 할 수 있다. ex).

1. 비동기 작업을 할 때 기존 요청을 취소 처리 할 수 있다.
2. 특정 액션이 발생했을 때 이에 따라 다른 액션이 디스패치되게끔 하거나, 자바스크립트 코드를 실행 할 수 있다.
3. 웹소켓을 사용하는 경우 Channel 이라는 기능을 사용하여 더욱 효율적으로 코드를 관리 할 수 있다. [참고](https://medium.com/@pierremaoui/using-websockets-with-redux-sagas-a2bf26467cab)
4. API 요청이 실패했을 때 재요청하는 작업을 할 수 있다.

redux-saga는 다양한 상황에 사용될 수 있는 만큼, 제공되는 기능도 많고, 사용방법 진입장벽이 꽤나 크다. 특히 개인적으로 처음 보는 [Generator](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator)라는 문법을 사용한다. 일단 먼저 이것부터 집고 넘어가자.

