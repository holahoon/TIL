# json-server
[json-server documentation](https://github.com/typicode/json-server)

프로젝트와 비슷한 경험을 갖기 위해서 dummy data 를 json 형태로 가져올 수 있는 도구다.
이 글은 리덕스 튜토리얼 중 [redux-thunk](./../Redux/reduxMiddleware/reduxThunk.md) 파일과 함께 사용 되고 있다. 궁금하다면 참고 하도록 =)

## Installation
Installs globaly

```sh
$ npm install -g json-server
```
```sh
$ yarn global add json-server
```

## Getting Started

##### dummyData.json
```json
{
  "posts": [
    {
      "id": 1,
      "title": "리덕스 미들웨어를 배워봅시다",
      "body": "리덕스 미들웨어를 직접 만들어보면 이해하기 쉽죠."
    },
    {
      "id": 2,
      "title": "redux-thunk를 사용해봅시다",
      "body": "redux-thunk를 사용해서 비동기 작업을 처리해봅시다!"
    },
    {
      "id": 3,
      "title": "redux-saga도 사용해봅시다",
      "body": "나중엔 redux-saga를 사용해서 비동기 작업을 처리하는 방법도 배워볼 거예요."
    }
  ]
}
```
이런 식으로 json 파일을 프로젝트 디렉토리에(src 디렉토리 밖에) 만들어 준다.
그리고 터미널에 다음과 같이 커맨드를 돌리므로 로컬 호스트를 열어주자.
```sh
$ json-server ./dummyData.json --port 4000
```
localhost 4000에 돌리는 이유는 이미 리액트는 3000에 호스팅 되있으니깐. 사실 굳이 이렇게 커맨드를 안해도 [json-server documentation](https://github.com/typicode/json-server)을 읽으면 다른 방법으로도 기재 되있다.
그럼 이제 아래와 같은 메시지가 나올거다
```sh
  \{^_^}/ hi!

  Loading ./dummydata.json
  Done

  Resources
  http://localhost:4000/posts

  Home
  http://localhost:4000

  Type s + enter at any time to create a snapshot of the database
```
졸커염이다ㅎ 
자, 이렇게 해서 가짜 서버 준비가 완료 되었다.

## Using Axios to call API

자, 일단 axios 를 설치해주자.
```sh
$ yarn add axios
```

다시한번 [redux-thunk](./../Redux/reduxMiddleware/reduxThunk.md) 파일을 참고해보면 여기에 포스트 목록과 id 파라미터를 받아서 포스트를 가져오는 비동기 함수가 있다. `setTimeout`을 사용해서 만들었지만 지금은 json 파일을 사용해서 로컬에 서버를 돌리고 있으니 그걸 사용 하는거다.

##### api/posts.js
```jsx
import axios from "axios";

// 포스트 목록을 가져오는 비동기 함수
export const getPosts = async () => {
  const response = await axios.get("http://localhost:4000/posts");
  return response.data;
};

// ID로 포스트를 조회하는 비동기 함수
export const getPostById = async (id) => {
  const response = await axios.get(`http://localhost:4000/posts/${id}`);
  return response.data;
};
```
이제 진짜 axios를 사용해서 데이터를 전달받는 함수를 구현하였다.