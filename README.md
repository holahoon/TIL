# Today I Learned 

> 오늘 배운 내용 직접 정리하여 모아두는 나만의 공간 by DK
> Because we often forget what we learned, thanks to our brain XD

![Alt Text](./assets/thanks_brain.gif "Thanks, brain")
---

## 📝 Categories

* **Front-end**
  * [HTML](#HTML)
  * [JavaScript](#javascript)
  * [TypeScript](#typescript)
  * [React](#react)
  * [Redux](#redux)
* **Tools**
  * [Github](#github)
  * [Git](#git)
  * [CLI](#cli)
* **General knowledge**
  * [Front-end knowledge](#front-end-knowledge)
  * [Web knowledge](#web-knoweldge)
  * [JavaScript knowledge](#javascript-knowledge)
* **Algorithm**
  * [JavaScript Algorithm](#javascript-algorithm)

---

## 🙌 Front-end

### HTML
* [template tag](./front-end/HTML/templateTag.md) - using `template` tag to set up "to be used" HTML code.
* * [Load Scripts dynamically](./front-end/HTML/loadScriptDynamically.md) - Use JavaScript to add `script` dynamically instead of manually adding into HTML file.

### JavaScript
* [Modify an object](./front-end/JavaScript/modifyObject.md) - add, modify, deleting properties in an object. Dynamic property assignment.
* [Copy an object](./front-end/JavaScript/copyObject.md) - copy an object using spread operator or Object.assign along with deep(nested) copy.
* [Object destructuring]() - destructurting an object.
* [Object Descriptors](./front-end/JavaScript/objectDescriptors.md) - 
* [Ways to check if the property exists in an object](./front-end/JavaScript/PropertyInObject.md)
* [Shallow Copy & Deep copy](./front-end/JavaScript/ShallowAndDeepCopy.md)
* [Binary & Unary Operators](./front-end/JavaScript/BinaryAndUnaryOperators.md)
* [Generator Object](./front-end/JavaScript/generatorObject.md) - is returned by a generator function.
* [Understanding `this` keyword](./front-end/JavaScript/understandThisKeyword.md) - understanding `this` keyword in JavaScript.
* [Working with Classes](./front-end/JavaScript/workingWithClasses.md) - Object-Oriented Programming - basics
  * [Static methods, fields & properties](./front-end/JavaScript/staticMethodsAndProperties.md)
  * [Understanding `getters` and `setters`](./front-end/JavaScript/understandGetterAndSetter.md) - times when a property can be set or to get(retrieve) it.
  * [Class Inheritance](./front-end/JavaScript/classInheritance.md) - Inherit class
  * [Class Private Fields, Properties and Methods](./front-end/JavaScript/classPrivateProperties.md) - Class private properties
  * [Constructor Functions](./front-end/JavaScript/constructorFunctions.md) - how `contructor(){}` works in class pretty much.
  * [Prototypes](./front-end/JavaScript/prototypes.md) - continues using [constructor functions](./front-end/JavaScript/constructorFunctions.md)
  * [Methods in Classes & in Constructors](./front-end/JavaScript/methodsInClasses.md) - Declaring methods in a class and constructor. Summary of contructor funtions and methods as well.
  
##### DOM & Browser APIs
* [dataset](./front-end/JavaScript/DOM_and_browser/dataset.md) - Access data-* Attributes

##### Working with events
* [Browser Events](./events/../front-end/JavaScript/events/browserEvents.md) - working with events (addEventListener, removeEventListener).
* [Basic infinite scrolling](./front-end/JavaScript/events/basicInfiniteScroll.md) - an example to create a basic infinite scrolling
* [Scrolling Events Tips](./front-end/JavaScript/DOM_and_browser/scrollingEvents.md) - Some tips to know when scrolling.
* [Event Propagation](./front-end/JavaScript/events/eventPropagation.md) - event propagation.

##### Functions
* [Pure functions & Side-effects](./front-end/JavaScript/functions/pureFunctions.md) - What is "pure" function without "side effects"
* [Factory functions](./front-end/JavaScript/functions/pureFunctions.md) - A function that returns a new object.
* [Closures](./front-end/JavaScript/functions/closures.md) - Every function in JavaScript is a closure.
* [Recursion](./front-end/JavaScript/functions/recursion.md) - What is recursion? - a function that returns itself. whuuutttt

##### Strings
* [Tagged Template Literals](./strings/../front-end/JavaScript/strings/taggedTemplate.md) - This is a pretty interesting string using template literals.
* [Regex](./front-end/JavaScript/strings/regex.md) - Regular Expression.

##### Async, Promises and Callbacks
* [Asynchronous & Synchronous](./front-end/JavaScript/async_promises_callbacks/asyncAndSync.md) - Asynchronous & Synchronous code and Event Loop.
* [Promises](./front-end/JavaScript/async_promises_callbacks/promises.md) - Promises to avoid callback hell when working with asynchronous code.
* [Promise.all & Promise.race etc]() - In different cases when in need to be more dynamic - `all`, `race`, `allSettled`.
* [Async / Await function](./front-end/JavaScript/async_promises_callbacks/asyncAndAwait.md) - Async and Await function when working with asynchronous code.


### TypeScript

* [Get Started with TS](./front-end/TypeScript/getStartedWithTS.md)
* [Generics in TS](./front-end/TypeScript/genericsInTS.md)
* **TypeScript with React**
  * [TS with React](./front-end/TypeScript/reactWithTS.md)
  * [TS with React Functional Component](./front-end/TypeScript/reactFunctionalComponentTS.md)
  * [TS with React useState](./front-end/TypeScript/reactUseStateTS.md)
  * [TS with React useReucer](./front-end/TypeScript/reactUseReducerTS.md)
  * [TS with React Context API](./front-end/TypeScript/reactContextTS.md)

### React

* [React Performance Optimization](./front-end/React/reactPerformanceOpti.md) - In times when in need of **not** using `pagination` or `virtualization` but rather need to render a **chunk of components at once**.
* [When to use React.useCallback and React.useMemo](front-end/React/useCallbackAnduseMemo.md) - Diving deep into when to use React.useCallback or React.useMemo.

### Redux

* **Redux Middleware**
  * [What is middleware](./front-end/Redux/reduxMiddleware/whatIsMiddleware.md) - 미들웨어의 기본 지식
  * [redux-thunk](./front-end/Redux/reduxMiddleware/reduxThunk.md) - redux-thunk를 사용한 미들웨어
    * [redux-thunk with router](./front-end/Redux/reduxMiddleware/reduxThunkWithRouter.md) - redux-thunk에서 라우터 연동하기
  * [redux-saga](./front-end/Redux/reduxMiddleware/reduxSaga.md) - redux-saga를 사용한 미들웨어
    * [redux-saga with promise](./front-end/Redux/reduxMiddleware/reduxSagaWithPromise.md)- redux-saga로 프로미스 다루기
    * [redux-saga with router](./front-end/Redux/reduxMiddleware/reduxSagaWithRouter.md) - redux-saga에서 라우터 연동하기
  * [redux-logger](./front-end/Redux/reduxMiddleware/whatIsMiddleware.md) - console.log로 액션, 디스패치를 볼수 있는 툴(라이브러리) - [What is middleware](./front-end/Redux/reduxMiddleware/whatIsMiddleware.md) 를 참고
* **Redux helper tools**
  * [Redux-DevTools](./front-end/Redux/reduxMiddleware/whatIsMiddleware.md) - 리덕스 데브툴즈 익스텐션 - [What is middleware](./front-end/Redux/reduxMiddleware/whatIsMiddleware.md) 를 참고
  * [Typesafe-actions](./front-end/Redux/reduxHelperTools/typesafeActions.md) - action creator 와 reducer 를 좀더 쉽고 깔끔하게 작성 할 수 있게 해주는 라이브러리

#### ETC

* [json-server](./front-end/ETC/jsonServer.md) - 로컬 환경에 가짜 API 서버 를 만들기 위한 도구 - went over how to use React.js with json-server along with CORS and Proxy.

## 🔧 Tools

### Github
* [Create a personal access token](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token) - HTTPS 로 repo를 clone 할 경우 two-step authentication에 필요한 토큰을 받는 방법

### Git

### CLI

* [Terminal Commands](./tools/CLI/terminalCommands.md) - Terminal commands list
* [NVM](./tools/CLI/nvm.md) - Node Version Manager

### VSCode

* [User Snippet](./tools/VSCode/userSnippet.md)

## 🧩 General Knowledge

### Front-end Knowledge

* [Front-end Common Sense](./general-knowledge/Front-end-knowledge/front-endCommonSense.md)

#### Web Knowledge

* [Web Common Sense](./general-knowledge/Web-knowledge/webCommonSense.md)

#### JavaScript Knowledge
* [JavaScript Common Sense](./general-knowledge/JavaScript-knowledge/javaScriptCommonSense.md) - 이벤트 루프, 동기/비동기, 호이스팅, 스코프, 클로저, this, 화살표 함수, 이벤트

## 🧑🏻‍💻 Algorithm

### JavaScript Algorithm

* [JavaScript Algorithm](./algorithm/JavaScriptAlgorithm.md)

## License

© 2020 David Kim

This repository is licensed under the MIT license. See [`LICENSE`](./LICENSE) for details.