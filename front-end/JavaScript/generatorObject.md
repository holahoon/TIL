# Generator Object

Generator 객체는 [generator function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*) 으로부터 반환된 값이며 [반복자와 반복자 프로토콜](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) 을 준수한다.
> The above generator function written as `function*` returns a Generator object.

[MDN generator documention - en](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
[MDN generator documention - kr](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Generator)
[Velopert redux-saga 강의](https://react.vlpt.us/redux-middleware/10-redux-saga.html)

## Generator 문법 배우기
이 문법의 핵심 기능은 함수를 작성 할 때 함수를 특정 구간에 멈춰놓을 수도 있고, 원할 때 다시 돌아가게 할 수도 있다. 또한, 결과값을 여러번 반환 할 수도 있다.
예를 들면,
```javascript
function weirdFunction() {
    return 1;
    return 2;
    return 3;
}
```

일반 함수에선 이렇게 할 수가 없다. 이 함수는 무조건 `1` 을 반환하게 될 거다.
그런데 제너레이터 함수를 사용하면 함수에서 값을 순차적으로 반환할 수 있다(f'ing awesome isn't it).

```javascript
function* generatorFunction() {
    console.log('Hey!');
    yield 1;
    console.log('generator function!');
    yield 2;
}
```

이런 식으로 작성을 하고 아래와 같이 호출 해보자.

```javascript
const generator = generatorFunction();
```

그리고 Generator.prototype.next() 메소드를 사용해 보면

```javascript
generator.next();
// Hey!
// {value: 1, done: false}
```

One more time!

```javascript
generator.next();
// generator function!
// {value: 2, done: false}
```

제너레이터 함수를 호출 한다고 해서 해당 함수 안의 코드가 실행되진 않는다. `generator.next()` 를 호출해야만 실행되며 `yield` 한 값을 반환하고 코드의 흐름을 멈춘다. 그리고 다시 `generator.next()` 를 호출하면 흐름이 이어서 시작된다.

#### 또 다른 예시를 보자!

`next`를 호출 할 때 인자를 전달해서 이를 제너레이터 함수 내부에서 사용할 수 있다.
```javascript
function* sumGenerator() {
    console.log('generator started!');
    let a = yield;
    console.log('got value a');
    let b = yield;
    console.log('got value b');
    yield a + b;
}
```

```javascript
const sumGen = sumGenerator();
```
이제 호출 ㄱㄱ

```javascript
sumGen.next();
// generator started!
// {value: undefined, done: false}
sumGen.next(10);
// got value a
// {value: undefined, done: false}
sumGen.next(20);
// got value b
// {value: 30, done: false}
```