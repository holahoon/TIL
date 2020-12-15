# 자바스크립트 기본 상식
면접 볼때 아주 유용하게 쓰이는 지식들을 찾아보며 모아봤다.

## Categories
  - [이벤트 루프(Event loop)](#이벤트-루프event-loop)
  - [동기(Synchronous) / 비동기(Asynchronous)](#동기synchronous--비동기asynchronous)
  - [호이스팅(Hositing)](#호이스팅hositing)
  - [스코프(Scope)](#스코프scope)
  - [클로저(Closure)](#클로저closure)
  - [this](#this-keyword)
  - [화살표 함수(Arrow function)](#화살표-함수arrow-function)
  - [이벤트](#이벤트event)
    - [이벤트 등록](#이벤트-등록event-registration)
    - [이벤트 버블링](#이벤트-버블링event-bubbling)
    - [이벤트 캡쳐](#이벤트-캡쳐event-capture)

---

## 콜 스택(Call stack)
프로그램이 함소 호출을 추적할 때 사용하는 것이다. 콜 스택은 function call 당 하나씩의 스택들로 이루어 져있다. 작동되는 순서에 따라 차례대로 콜 스택에 들어가고, 값을 반환하면 콜 스택에서 지워진다. 자바스크립트는 싱글 스레드를 기반하기 때문에 단 한줄의 콜 스택만이 존재한다.
```javascript
function A(){
  return 'hello';
}
function B(){
  return `${A()} {everyone!}`;
}
```
1. 먼저 소환된 `function B`를 콜 스택에 추가
2. `function B`가 `function A`를 소환
3. `function A`가 콜 스택에 추가
4. `function A`가 `'hello'`값 출력후 스택에서 삭제
5. `function B`가 `'hello everyone!'`값 출력후 스택에서 삭제
> *콜 스택은 LIFO (Last In First Out) 형식으로 취한다.*

## 큐(Queue)
대기열과 같은 개념이라고 이해하면 된다. 

## 이벤트 루프(Event loop)

자바스크립트 엔진은 `Memory Heap` 과 `Call Stack`으로 구성 되어있다. 가장 유명한 것은 구굴의 V8 engine 이다. 자바스크립트는 **단일 스레드(single thread) 프로그래밍 언어**, 바로 **`call stack` 이 하나** 라는 뜻이다(멀티가 되지 않고 하나씩 처리한다). *(stack: 선입후출의 룰을 따른다)*
- **Memory heap:** 메모리 할당이 일어나는 곳 이다. (선언한 변수나 함수 등이 담겨있다)
- **Call stack:** 코드가 실행될 때 쌓이는 곳. stack 형태로 쌓인다.

또한 **Web API**가 있는데 이건 자바스크립트 엔진과는 별개다. 이것은 브라우저에서 제공하는 API로 DOM, Ajax, timeout 등이 있다. 자바스크립트 엔진에 `call stack` 에서 실행된 비동기 함수는 이 Web API를 호출하고, Web API는 콜백함수를 `Callback Queue`에 밀어 넣는다. 이 callback queue 는 비동기적으로 실행되는 콜백함수가 보관되는 곳이다. *(queue: 선입선출의 룰을 따른다)*

그래서 **이벤트 루프는 `call stack` 과 `call queue`의 상태를 체크하여 `call stack` 이 빈 상태가 되면 `call queue` 에서 첫번째 콜백을 `call stack` 으로 밀어넣는다.**

## 동기(Synchronous) / 비동기(Asynchronous)

`동기(syncronous)`는 요청을 보낸 후 해당 요청의 응답을 받아야 다음 동작을 실행하는 방식을 의미한다. 하나의 이벤트가 끝나야 그 다음 이벤트를 처리하는 **실행 순서가 확실한 것을 동기적 방식이**라고 한다.
`비동기(asynchronous)`는 요청을 보낸 후 응답과 관계없이 다음 동작을 실행할 수 있는 방식을 의미한다. 연속적으로 발생하는 이벤트를 담은 후 완료되는 순서대로 일을 처리하는 **실행 순서가 확실하지 않는 것을 비동기적 방식**이라 한다.
*reference: [비동기 파헤치기](https://velog.io/@yejinh/%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0)*

## Promise

자바스크립트에선 대부분의 작업들이 비동기로 이루어진다. 동기적으로 처리를 해야할때 예전엔 콜백 함수로 처리하면 되는 문제였지만 프론트엔드의 규묘가 커지면서 코드의 복잡도가 높아지기 시작했다. 그러다보니 콜백이 중첩되는 상황이 생기기 시작한거다. 이를 해결할 방안이 바로 Promise 패턴이다. Promise는 말 그대로 "약속"이다. 정확히 말하자면 "지금은 없는데 이상없으면 이따가 주고(resolve) 없으면(reject) 알려줄게~".

## 호이스팅(Hositing)

> ES6 문법이 표준화가 되면서 사실 크게 신경쓰지 않아도 될 부분이지만, 자바스크립트의 특성을 가장 잘 보여주는 특성이기에 알아보자.

Hoist 라는 뜻은 끌어올리기 라는 뜻이다. 자바스크립트에서 끌어 올려지는 변수다. `var` 변수 선언과 함수선언문에서만 호이스팅이 일어난다. 호이스트란 변수의 정의가 그 범위에 따라 **선언(declaration)** 과 **할당(assignment)** 으로 분리되는 것을 의미한다. 즉, **변수가 함수 내에서 정의되었을 경우, 선언이 함수의 최상위로, 함수 바깥에서 정의되었을 경우, 전역 컨텍스트의 최상위로 변경이 된다.**

예를 들면 아래의 예제를 보자:
```javascript
function getX() {
  console.log(x); // undefined
  var x = 100;
  console.log(x); // 100
}
getX();
```
`var x;` 를 호이스트 하기 때문에 에러가 아닌 `undefined` 가 나오는 거다. 만약 이 예시를 JS parser 내부의 호이스팅의 결과를 보면,
```javascript
/* --- JS Parser 내부의 호이스팅(Hoisting)의 결과 - 위와 동일 --- */
function getX() {
  var x;
  console.log(x);
  x = 100;
  console.log(x);
}
getX();
```

함수에서는 다음과 같이 된다.
- 함수표현식(Function Expression)의 경우엔 호이스팅이 **안된다**. 함수표현식이란, 변수를 만들고 함수를 할당해주는 표현식이다.
- 함수선언식(Function Declaration)의 경우엔 호이스팅이 **된다**.
```javascript
declarationFunction();
assignmentFunction();

function declarationFunction(){ // 함수선언문
    console.log('declared!');
}
var assignmentFunction = function(){ // 함수할당문
    console.log('assigned!');
}
```
JS Parser 내부의 호이스팅 결과를 보면,
```javascript
/* --- JS Parser 내부의 호이스팅(Hoisting)의 결과 - 위와 동일 --- */
var assignmentFunction; // [hositing] - 함수할당식의 변수값 선언

function declarationFunction(){ // [hositing] - 함수선언문
    console.log('declared!');
}

declarationFunction();
assignmentFunction(); // Uncaught TypeError: assignmentFunction is not a function

var assignmentFunction = function(){ // 함수할당문
    console.log('assigned!');
}
```
할당된 함수는 함수 리터럴을 할당하는 구조이기 때문에 호이스팅 되지 않으며 그렇기 때문에 런타임 환경에서 Type Error를 발생시킨다.
*!주의: 변수에 할당된 함수표현식은 끌어 올려지지 않기 때문에 이때는 변수의 스코프 규칙을 그대로 따른다.*

[To Top](#categories)

## 스코프(Scope)

어떤 변수들에 접근할 수 있는지를 정의한다. 예를 들면
```javascript
if (true){
    const message = 'DK';
}
console.log(message); // ReferenceError: message is not defined
```
`if`문의 코드 블럭은 `message` 변수를 위한 *스코프* 를 만든다(the `if` code block creates a `scope` for `message` variable).
자, 이제 어떠한 종류의 스코프가 있는지 알아보자.
#### Block scope(블록 스코프)
블록 스코프는 코드블럭 중괄호`{}` 내부에서 `const` 또는 `let`으로 변수를 선언하면 그 변수는 중괄호 안에서만 사용이 가능하다.
```javascript
if (true) {
  // "if" block scope
  const message = 'Hello';
  console.log(message); // 'Hello'
}
console.log(message); // throws ReferenceError
```
`if`문 만이 아니더라도, `for`, `while` 도 스코프를 생성한다.(아래 예제와 같이)
```javascript
for (const number of [1, 2, 3]) {
  // "for" block scope
  const name = 'DK';
  console.log(number);   // 1, 2, 3
  console.log(name); // 'DK', 'DK', 'DK'
}
console.log(number);   // throws ReferenceError
console.log(name); // throws ReferenceError
```
또 아래와 같이, 자바스크립트에선 "standalone code block"을 정의할 수 있다.
```javascript
{
  // block scope
  const message = 'Hello';
  console.log(message); // 'Hello'
}
console.log(message); // throws ReferenceError
```
**참고 해야 할 것은, `var` 은 블록 스코프가 아니다!!**
> *A code block does not create a scope for `var` variables, but a function body does.*

#### Function scope(함수 스코프)
함수 스코프 내에선 `var`, `let`, `const`로 변수를 선언할수 있다. 하지만 이 변수들은 함수 밖에선 절대 접근이 안된다. 함수 안에서만 사용하고 접근할 수 있는 거다. 심지어 블록 스코프와 다르게 `var`도 함수 밖에서 접근이 안된다.

#### Global scope(적역 스코프)
말 그대로 가장 밖에 있는 스코프다. 아무 inner scope 안에서 그 전역 스코프에 선언된 변수에 접근할수 있다. 

#### Lexical scope(렉시컬 스코프)
```javascript
function outerFunc() {
  // the outer scope
  let outerVar = 'I am from outside!';

  function innerFunc() {
    // the inner scope
    console.log(outerVar); // 'I am from outside!'
  }

  return innerFunc;
}

const inner = outerFunc();
inner();
```
이건 좀 설명하기 어려운데, 영어로 이렇게 표기되있다.
> Lexical scoping means that the accessibility of variables is determined statically by the position of the variables within the nested function scopes: the inner function scope can access variables from the outer function scope.
> In the example, the lexical scope of innerFunc() consists of the scope of outerFunc().
> Moreover, the innerFunc() is a closure because it captures the variable outerVar from the lexical scope.

조금 번역 해보면 `innerFunc()`의 렉시컬 스코프는 `outerFunc()`의 스코프를 가지고 있다. 위와 같이 `innerFunc()`의 함수가 `outerFunc()` 함수 안에 있는 `outerVar` 변수에 접근할 수 있는거다. 그리고  innerFunc()는 렉시컬 스코프에서 outerVar를 데려올 수 있기 때문에(?) 클로저다.

reference: [JavaScript Scope](https://dmitripavlutin.com/javascript-scope/)

[To Top](#categories)

## 클로저(Closure)

렉시컬 스코프에 예제를 다시 한번 보겠다.
```javascript
function outerFunc() {
  // the outer scope
  let outerVar = 'I am from outside!';

  function innerFunc() {
    // the inner scope
    console.log(outerVar); // 'I am from outside!'
  }

  return innerFunc;
}

const myInnerFunc = outerFunc();
myInnerFunc();
```
변수 `myInnerFunc`에 저장된 함수는 `outerFunc` 이다. 코드를 이해해보면 `innerFunc`는 자기 렉시컬 스코프 밖에서 호출되고 있는거다. 그런데도 자신의 렉시컬 스코프에 있는 `outerVar`의 값을 사용할수 있다. 이게 클로저다.
> innerFunc() "closes over" (a.k.a. captures, remembers) the variable outerVar from its lexical scope.

바로 자신의 렉시컬 스코프안에 있는 변수를 "기억"하는거다. 그러므로 `innerFunc`는 클로저 인거다.
정의를 한번 보자면:
> The closure is a function that accesses its lexical scope even executed outside of its lexical scope.

reference: [JavaScript Closures](https://dmitripavlutin.com/simple-explanation-of-javascript-closures/)

[To Top](#categories)

## this keyword
이건 담배가 아니다 (TMI: 이 글을 적는 기준으로 금연 4년차 인데도 냄새도 싫다).
자바스크립트에서 함수를 실행될 때마다 함수 내부에 `this`라는 객체가 추가된다. `arguments`라는 유사 배열 객체와 함께 함수 내부로 암묵적으로 전달되는 것이다. 그렇기 때문에 자바스크립트에서의 `this`는 함수가 호출된 상황에 따라 그 모습을 달리한다. `this`는 가장 기본적으로 `window`인데 만약 `객체 메서드, bind call apply, new`일 때 `this`가 바뀐다.

#### 객체의 메서드를 호출할 때
객체의 프로퍼티가 함수일 경우 메서드라고 부른다. `this`는 함수를 실행할 때 함수를 소유하고 있는 객체(메서드를 포함하고 있는 인스턴스)를 참조한다.
```javascript
var myObject = {
  name: "DK",
  sayName: function() {
    console.log(this);
  }
};
myObject.sayName();
// console> Object {name: "DK", sayName: sayName()}
```
그래서 만약 `console.log.(this.name)`를 찍으면 `"DK"` 가 나오는거다.

#### 함수를 호출할 때
말 그대로 특정 개체안의 메서드가 아닌 그냥 함수를 호출할 때다.
```javascript
var value = 100;
var myObj = {
  value: 1,
  func1: function() {
    console.log(`func1's this.value: ${this.value}`);

    var func2 = function() {
      console.log(`func2's this.value ${this.value}`);
    };
    func2();
  }
};

myObj.func1();
// console> func1's this.value: 1
// console> func2's this.value: 100
```
위 예제를 보면 `func2`는 메서드가 아닌 그냥 함수다. 그렇기 때문에 이렇게 `func2`와 같이 해당 함수 내부에서 사용된 `this`는 그 객체가 아닌 전역객체에 바인딩 된다. 그렇기 때문에 콘솔에 찍힌걸 보면 1이 아닌 100이 되는거다.

#### 생성자 함수를 통해 객체를 생성할 때
그냥 함수를 호출 하는것이 아닌 `new`키워드를 통해 생성자 함수를 호출할 때 또 다르게 `this`가 바인딩 된다. `new` 키워드를 통해서 호출된 함수 내부에서의 `this`는 객체 자신이 된다.
```javascript
var Person = function(name) {
  console.log(this);
  this.name = name;
};

var foo = new Person("foo"); // Person
console.log(foo.name); // foo
```
`new` 연산자를 통해 함수를 생성자로 호출하게 되면, 일단 빈 객체가 생성되고 `this` 가 바인딩 된다. 이 객체는 함수를 통해 생성된 객체이며, 자신의 부모인 프로토타입 객체와 연결되어 있다. 그리고 return 문이 명시되어 있지 않은 경우에는 `this`로 바인딩 된 새로 생성한 객체가 리턴된다.

#### bind, call, apply 를 통한 호출
얘네들을 `this`를 주입하는 방법이다. `apply`나 `call`은 써본적은 없는데 `.bind()`는 자주 써본적이 있었다(리액트 클래스 컴포넌트 사용할때). 사실 이거의 대한 방법이 있는데 좀 귀찮아져서 이건 스킵하도록 하잣. 

[To Top](#categories)

## 화살표 함수(Arrow function)

화살표 함수는 일반함수와는 다르게 호이스팅이 불가능하다. 일반 함수와 다르게 객체에 메서드가 아니면 .bind()를 사용해서 그 객체를 가르키게 하는데 화살표 함수는 그게 안된다. 화살표 함수는 - it will lexically go up a scope, and use the value of `this` in the scope in which it was defined.
```javascript
// -- ES5 --
var obj = {
  name: 'DK',
  printName: function(){
    console.log(this.name) // "DK"
    setTimeout(function() {
        console.log(this.name) // "DK" (1초후에)
    }.bind(this), 1000) //  만약에 .bind() 가 안되면 위에 this 는 window 를 가르킨다 
  }
}

obj.printName();
```
```javascript
// -- ES6 --
var obj = {
  name: 'DK',
  printName: function(){
    console.log(this.name) // "DK"
    setTimeout(() => {
        console.log(this.name) // "DK" (1초후에)
    }, 1000)
  }
}

obj.printName();
```

> In classic function expressions, the this keyword is bound to different values based on the context in which it is called. With arrow functions however, this is lexically bound. It means that it usesthis from the code that contains the arrow function.

reference: [es6 arrow functions](https://www.freecodecamp.org/news/when-and-why-you-should-use-es6-arrow-functions-and-when-you-shouldnt-3d851d7f0b26/)

[To Top](#categories)

## 이벤트(Event)

### 이벤트 등록(Event Registration)
이벤트 등록이란 웹 애플리케이션에서 사용자의 입력을 받기 위해 필요한 기능이다.
```html
<button>add one item</button>
```
```javascript
var button = document.querySelector('button');
button.addEventListener('click', addItem);

function addItem(event) {
	console.log(event);
}
```
자, 그러면 브라우저는 어떻게 이벤트의 발생을 감지했을까? 두가지 방법이 있다. **이벤트 버블링**과 **이벤트 캡쳐**다.

### 이벤트 버블링(Event Bubbling)
```html
<body>
	<div class="one">
		<div class="two">
			<div class="three">
			</div>
		</div>
	</div>
</body>
```
```javascript
var divs = document.querySelectorAll('div');
divs.forEach(function(div) {
	div.addEventListener('click', logEvent);
});

function logEvent(event) {
	console.log(event.currentTarget.className);
}
```
다음과 같은 코드가 있을때 `<div class="three"></div>` 엘리먼트를 클릭하게 되면 콘솔엔 다음과 같이 찍힌다
```
three
two
one
```
이유는 바로 **이벤트 버블링** 때문이다. 브라우저는 특정 화면 요소에서 이벤트가 발생했을때 그 이벤트를 화면 최상위에 있는 화면 요소까지 이벤트를 전파시킨다. 이같이 하위에서 상위 요소로 이벤트 전파 방식을 **이벤트 버블링**이라고 한다.

### 이벤트 캡쳐(Event Capture)
반대로 이벤트 캡쳐는 버블링과 반대 방향으로 진행되는 이벤트 전파 방식이다.
```javascript
var divs = document.querySelectorAll('div');
divs.forEach(function(div) {
	div.addEventListener('click', logEvent, {
		capture: true // default value is false
	});
});

function logEvent(event) {
	console.log(event.currentTarget.className);
}
```
아까와 동일하게 `<div class="three"></div>` 엘리먼트를 클릭하게 되면 콘솔엔 버블링과는 다르게 다음과 같이 찍힌다
```
one
two
three
```

reference: [event bubbling - captain pangyo](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/)

[To Top](#categories)