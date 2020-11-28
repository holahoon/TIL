# React Functional Component with TypeScript tips

## Functional Component 안에 React.FC 사용/미사용의 차이점

```tsx
type GreetingsProps = {
  name: string;
};

export const Greetings: React.FC<GreetingsProps> = ({ name }) => (
  <div>Hello, {name}</div>
);
```
`React.FC` 를 사용 할 때는 props 의 타입을 Generics 로 넣어서 사용한다. 이렇게 `React.FC`를 사용할때 장/단점에 대해서 알아보자:
- **장점**
1. props 에 기본적으로 `children` 이 들어간다.
2. 컴포넌트의 defaultProps, propTypes, contextTypes 를 설정 할 때 자동완성이 될 수 있다.
- **단점**
1. children 이 옵셔널 형태로 들어가있다보니 컴포넌트의 props 의 타입이 확실하게 명백하지 않다. 어떤 경우에는 children이 무조건 있어야 할 경우도 있고 아닌 경우도 있다. React.FC를 사용하면 Props 타입에 children 을 설정해야한다. 아래 예제:
```tsx
type GreetingsProps = {
  name: string;
  children: React.ReactNode;
};
```
2. defaultProps가 (아직까지는) 작동하지 않는다. 아래의 예제를 보자.
##### [ Greetings.tsx ]
```tsx
type GreetingsProps = {
  name: string;
  age: number;
};

export const Greetings: React.FC<GreetingsProps> = ({ name, age }) => (
  <div>
    Hello, {name}, {age}
  </div>
);

Greetings.defaultProps = {
  age: 30,
};
```

##### [ App.tsx ]
```tsx
import { Greetings } from "./Greetings";

const App: React.FC = () => {
  return (
    <div>
      <h1>hello =)</h1>
      <Greetings name={"DK"} /> // Complains
    </div>
  );
};

export default App;
```
아무리 `Greetings.tsx` 파일 안에 `defaultProps를` 지정해준다 해도 `age 값`이 없다고 컴플레인 한다.

이제 아래 예제와 같이 `React.FC` 를 생략하면 아주 잘 작동이 된다.
```tsx
export const Greetings = ({ name, age }: GreetingsProps) => (
   ...
);
```
> 쓰지 말라는 [팁](https://medium.com/@martin_hotell/10-typescript-pro-tips-patterns-with-or-without-react-5799488d6680#78b9)도 존재한다.

화살표 함수를 사용하지 않고 일반 function 를 사용하면 다음과 같이 사용하면 된다 =)
```tsx
function Greetings({ name, mark }: GreetingsProps) {
  return (
    <div>
      Hello, {name} {mark}
    </div>
  );
}
```

### props로 함수를 받는다면 이렇게 한다.

##### [ Greetings.tsx ]
```tsx
type GreetingsProps = {
  name: string;
  age: number;
  printName: (name: string) => void; // <--
};

export const Greetings = ({ name, age, printName }: GreetingsProps) => {
  const handleClick = () => printName(name);

  return (
    <div>
      Hello, {name}, {age}
      <button onClick={handleClick}>greet</button>
    </div>
  );
};

Greetings.defaultProps = {
  age: 30,
};
```
##### [ App.tsx ]
```tsx
import { Greetings } from "./Greetings";

const App: React.FC = () => {
  const printName = (name: string) => {
    console.log(`Hello, ${name}`!);
  };

  return (
    <div>
      <h1>hello =)</h1>
      <Greetings name={"DK"} printName={printName} />
    </div>
  );
};

export default App;
```