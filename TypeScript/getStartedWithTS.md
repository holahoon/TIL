# What is TypeScript?

## TypeScript
> Is a programming language, but also a tool. It's a powerful compiler which you run over your code to compile your TypeScript code to JavaScript.
> It adds "types". It adds features to JavaScript language which gives the developer an opportunity of identifying errors in the code eariler before the script runs and the error occurs at run time in the browser.

### Categories
- [Get Started](#get-started-with-typescript)
- [Basic Types](#basic-types)
- [Functional Types](#functional-types)
- [Interface](#interface)
- [Type Alias](#type-alias)

우리가 JavaScript 에서 흔하게 아는 타입은 string, number, boolean, object, array가 있다. 물론 더 있을 수 있겠지만...

### Get Started with TypeScript

##### 벨로퍼트님의 [블로그](https://react.vlpt.us/using-typescript/01-practice.html)에서 엄청 공부하고 참조한 글 입니다. 

먼저 간단하게 이렇게 setup를 해보자.
```bash
$ mkdir ts-practice # ts-practice 라는 디렉터리 생성
$ cd ts-practice # 해당 디렉터리로 이동
$ yarn init -y # 또는 npm init -y
```
이제 ts-practice 폴더 안에 `package.json` 파일이 생성 되있을거다.

일단 TypeScript가 글로벌로 설치 되있다는 가정하에 시작한다. 안 되있으면
```bash
$ yarn global add typescript
```

자, 이제 `tsc --init` 로 tsconfig.json 파일을 생성 해주도록 하자
안에 엄청난 코드가 있을거고 주석 처리가 되있을건데 너무 겁먹지 말자, 우리에겐 베스트프렌드 구글과 스택오버플로우가 있잖아 =) (라고 말하고 겁나 무섭다)

이제 우리가 딱 하나만 바꿀것은 `"outDir": "./"` 이 라인이다. Uncomment를 해주고 `"outDir": "./dist",` 이렇게 써준다. 이제 `tsc`를 돌렸을때 `js파일`은 무조건 `dist 폴더` 안에 생성되게 된다.

이렇게 해놓고 이제 파일을 만들어 볼거다.

`src` 파일을 만들고 그 안에 `practice.ts` 를 만들어 준다. 그런 다음에 안에 다음과 같이 친다.

```ts
const greetings: string = "Hello DK!";
console.log(greetings);
```
여기서 `:string` 은 말 그대로 greetings에 들어간 값은 무조건 string(문자열)이다. 만약 문자열이 아닐 경우에는 바로 컴플레인 한다.

이제 cli에 `tsc` 라고 쳐보자. 그러면 `practice.ts` 와 같은 이름으로 dist 폴더 안에 `practice.js` 가 생성 될거다. 브라우저는 멍청해서 절대 타입스크립트를 알아듣지 못한다. 그러기 때문에 타입스크립트는 무조건 브라우저가 알아 들을 수 있는 자바스크립트로 컴파일을 하는거다. 생성된 `practice.js`는 이렇게 생겼을거다. (아님 말구ㅎㅎ)
```js
"use strict";
var message = "hello DK";
console.log(message);
```

## Basic Types
```ts
// Type number
let count = 0;
count += 1;
count = "갑자기 문자?"; // will throw an error! [x]

// Type string
const message: string = "I'm sexy and I know it";

// Type boolean
const isDone: boolean = true;

// Type number array
const numbers: number[] = [1, 2, 3];

// Type string array
const messages: string[] = ["a", "b", "c"];
messages.push(1); // Cannot add number type to a string array! [x]

// Union Type
// could be string or undefined
let mightBeUndefined: string | undefined = undefined;
// could be number or null
let nullableNumber: number | null = null;
// one of the string values
let color: "red" | "orange" | "yellow" = "red";
color = "yellow"; // fine
color = "blue"; // not cool bro! [x]

```

## Functional Types

##### 두 숫자의 합을 구하는 함수를 구현해볼까나
```ts
function sum(x: number, y: number): number {
  return x + y;
}

sum(10, 20);
sum("I'm", "sexy"); // No bro, no! [x]

```
위의 예제는 `sum()`의 파라미터에 들어오는 `x`와 `y`는 `number 타입` 이고 `sum 함수`가 `return`하는 `값의 타입` 또한 `number` 라는걸 얘기한다.

##### 숫자 배열의 총 합을 구하는 함수를 구현해보자

```ts
function sumArray(numbers: number[]): number {
  return numbers.reduce((acc, curr) => acc + curr, 0);
}

const totalSum = sumArray([1, 2, 3, 4, 5]);
```

##### 아무것도 반환하지 않는다면 이렇게
```ts
function returnNothing(): void {
  console.log('Did you think I was going to return a value?');
}
```
`: void` 타입을 넣어주면 nothing's being returned

## Interface
interface는 **클래스** 또는 **객체**를 위한 타입을 지정 할 때 사용되는 문법이다.

#### 클래스에서 interface를 사용해보잣 

```ts
interface Shape {
  getArea(): number; // Shape interface 에는 getArea 라는 함수가 꼭 있어야 하며 해당 함수의 반환값은 숫자입니다.
}

/* Circle class */
class Circle implements Shape {
  // `implements` 키워드를 사용하여 해당 클래스가 Shape interface 의 조건을 충족하겠다는 것을 명시합니다.
  radius: number; // 멤버 변수 radius 값을 설정합니다.

  constructor(radius: number) {
    this.radius = radius;
  }

  getArea() {
    return this.radius * this.radius * Math.PI;
  }
}

/* Rectangle class */
class Rectangle implements Shape {
  width: number; // 들어오는 width의 타입은 number
  height: number; // 들어오는 height의 타입은 number

  constructor(width: number, height: number) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

const shapes: Shape[] = [new Circle(5), new Rectangle(10, 20)];

shapes.forEach((shape) => {
  console.log("each shape: ", shape);
  console.log("each area: ", shape.getArea());
});
```

자, 이제 tsc 로 컴파일 하고 node dist/practice.js 를 돌려보자.

```
each shape:  Circle { radius: 5 }
each area:  78.53981633974483
each shape:  Rectangle { width: 10, height: 20 }
each area:  200
```
이렇게 콘솔이 찍힐거다.

자, 이제 [accessor](https://www.typescriptlang.org/docs/handbook/classes.html#accessors)를 사용해보자. `public` 이나 `private` 를 사용하면 위의 예제와 같이 직접 하나하나 설정을 생략해줄 수 있다

```ts
interface Shape {
  getArea(): number;
}

class Circle implements Shape {
  constructor(public radius: number) {
    this.radius = radius;
  }

  getArea() {
    return this.radius * this.radius * Math.PI;
  }
}

class Rectangle implements Shape {
  constructor(private width: number, private height: number) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

const circle = new Circle(5);
const rectangle = new Rectangle(10, 20);

console.log(circle.radius);
console.log(rectangle.width); // Property 'width' is private and only accessible within class 'Rectangle'

const shapes: Shape[] = [new Circle(5), new Rectangle(10, 20)];

shapes.forEach((shape) => console.log(shape.getArea()));
```
여기서 `private`으로 되있는 프로퍼티는 클래스 밖에선 access가 될 수 없다.

#### 일반 객체를 interface 로 타입 설정해보자

```ts
interface Person {
  name: string;
  age?: number;
}

interface Developer {
  name: string;
  age?: number;
  skills: string[];
}

const person: Person = {
  name: "David",
  age: 30,
};

const career: Developer = {
  name: "Dev DK",
  skills: ["React.js", "Redux.js", "TypeScript"],
};

const persons: Person[] = [person, career];
console.log(persons);
```
여기에 ? 는 설정을 해도 되고 안해도 되는 값이다.
```
[
  { name: 'David', age: 30 },
  { name: 'Dev DK', skills: [ 'React.js', 'Redux.js', 'TypeScript' ] }
]
```

여기서 잘 보면 `Person` 과 `Developer` interface 가 매우 흡사하다. 이럴때는 `extends` 키워드를 써서 상속받을 수 있다.

```ts
interface Person {
  name: string;
  age?: number;
}

interface Developer extends Person {
  skills: string[];
}
```

## Type Alias

`type` 은 특정 타입에 별칭을 붙이는 용도로 사용한다. 이를 사용하여 객체를 위한 타입을 설정할 수도 있고, 배열, 또는 그 어떤 타입이던 별칭을 지어줄 수 있다.

```ts
type Person = {
  name: string;
  age?: number;
};

type Developer = Person & {
  skills: string[];
};

const person: Person = {
  name: "David",
};

const occupation: Developer = {
  name: "David",
  skills: ["JavaScript", "TypeScript"],
};

type People = Person[];
const people: People = [person, occupation];

console.log(people);
```
```
[
  { name: 'David' },
  { name: 'David', skills: [ 'JavaScript', 'TypeScript' ] }
]
```

& 는 Intersection 으로서 두개 이상의 타입들을 합쳐준다 ([reference](https://www.typescriptlang.org/docs/handbook/advanced-types.html#intersection-types))

##### Tip
> 💡 Interface 와 Type 을 어느 상황에 사용하는게 좋을까? 
> 클래스와 관련된 타입의 경우엔 interface 를 사용하는게 좋고, 일반 객체의 타입의 경우엔 그냥 type을 사용해도 무방하다. 사실 객체를 위한 타입을 정의할때 무엇이든 써도 상관 없는데 일관성 있게만 쓰면 된다. [reference](https://joonsungum.github.io/post/2019-02-25-typescript-interface-and-type-alias/)

## Generics

제너릭(Generics)은 타입스크립트에서 함수, 클래스, interface, type alias 를 사용하게 될 때 여러 종류의 타입에 대하여 호환을 맞춰야 하는 상황에서 사용하는 문법이다.
예를 들어 우리가 객체 A 와 객체 B 를 합쳐주는 merge 라는 함수를 만든다고 가정해보자. 그 상황에선 A 와 B 가 어떤 타입이 올 지 모르기 떄문에 이런 상황에서는 any 라는 타입을 쓸 수도 있다.
아래 예제를 보자
```ts
function merge(a: any, b: any): any {
  return {
    ...a,
    ...b,
  };
}

const merged = merge({ foo: 1 }, { bar: 2 });
```
이렇게 하게되면 merged 안에 무엇이 있는지 알 수가 없는거다. 이건 완전 타입 유추가 다 깨진거나 마찬가지다.
이럴 경우에는 다음과 같이 Generics를 사용해보자.

```ts
function merge<A, B>(a: A, b: B): A & B {
  return {
    ...a,
    ...b,
  };
}

const merged = merge({ foo: 1 }, { bar: 2 });
```

`merged`에 마우스를 갖다 대보면 이렇게 나온다.

```ts
const merged: {
    foo: number;
} & {
    bar: string;
}
```

또는 이렇게도 사용할 수 있다.
```ts
function wrap<T>(param: T) {
  return {
    param,
  };
}

const wrapper = wrap(true);
```

`wrapper`에 마우스를 갖다 대보면 이렇게 나온다.

```ts
const wrapper: {
    param: boolean;
}
```

#### Interface 에서 Generics 사용해보자

```ts
interface Items<T> {
  list: T[];
}

const items: Items<string> = {
  list: ["a", "b", "c"],
};

console.log(items);
```

#### Types 에서 Generics 사용해보자
```ts
type Items<T> = {
  list: T[];
};

const items: Items<string> = {
  list: ["a", "b", "c"],
};

console.log(items);
```

위와 아래의 예제의 결과는 이렇게 나온다.
```
{ list: [ 'a', 'b', 'c' ] }
```

#### Class 에서 Generics 사용해보자

```ts
class Queue<T> {
  list: T[] = [];

  get length() {
    return this.list;
  }

  enqueue(item: T) {
    this.list.push(item);
  }

  dequeue() {
    return this.list.shift(); // Removes the first element and when assigned, it returns the element that's been removed
  }
}

const queue = new Queue<number>();

queue.enqueue(10);
queue.enqueue(20);
queue.enqueue(30);
console.log(queue.list);
queue.dequeue();
console.log(queue.list);
```
```
[ 10, 20, 30 ]
[ 20, 30 ]
```