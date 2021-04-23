# React.useCallback vs React.useMemo

- **useMemo** is to memoize a calculation result between a function's calls and between renders
- **useCallback** is to memoize a callback itself (referential equality) between renders
- **useRef** is to keep data between renders (updating does not fire re-rendering)
- **useState** is to keep data between renders (updating will fire re-rendering)

[reference](https://levelup.gitconnected.com/understanding-the-difference-between-usememo-and-usecallback-ec956adb2004)
Let's take a look at the difference between these two React hooks!

As much as it is nice to use functional components, there's a slight little problem with functional components.
A functional component is the same as the render function we used to have in class components. It is a function that is being re-run on any props/state change.

This means:

1. A function called inside the component, it will be re-evaluated, again and again, on every re-render
2. If a function is created inside the component and is passed to a child component as props, it will be re-created, meaning the pointer will change, causing the child to re-render or call the function unnecessarily(depending on the situation)

To solve these particular issues, we typically we `React.useCallback` or `React.useMemo`. Well, what's the difference???????

## React.useMemo

Say we have a function declared outside of the component,

```javascript
import React, { useState } from "react";
import "./styles.css";

const plusFive = (num) => {
  console.log("i was called~!");
  return num + 5;
};

export default function App() {
  const [num, setNum] = useState(0);
  const [light, setLight] = useState(true);
  const numPlusFive = plusFive(num);

  return (
    <div className={light ? "light" : "dark"}>
      <div>
        <h1>Without useMemo</h1>
        <h2>
          Current number: {num}, Plus five: {numPlusFive}
        </h2>
        <div className="button-container">
          <button
            onClick={() => {
              setNum(num + 1);
            }}
          >
            Update Number
          </button>
          <button
            onClick={() => {
              setLight(!light);
            }}
          >
            Toggle the light
          </button>
        </div>
      </div>
    </div>
  );
}
```

Whenever we click on `Update Number` button which sets a new number or `Toggle the light` button which updates the boolean state, it will console log `"I was called~!"`. This console is from the `plusFive()` function. We can know that this function is called every time the states are updated.

#### For this case, we can use `React.useMemo`.

```javascript
const numPlusFive = useMemo(() => plusFive(num), [num]);
```

`useMemo` takes two parameters: A function that returns our function call(callback), and an array of dependencies. Only when one of the dependencies change, our callback function(`plusFive()`) will be called again. 
`useMemo` returns the results of that callback function execution and will store it in a memory to prevent the function from running again if the same parameters are used.

We can now see that only when we click `Update Number` button which updates the `num` state will only execute that `plusFive()` function.

## React.useCallback

Now let's take a look at how `useCallback` can be used.

```javascript
import React, { useState, useEffect } from "react";
import "./styles.css";

// Parent component
export default function App() {
  const [num, setNum] = useState(0);
  const [light, setLight] = useState(true);

  const plusFive = () => {
    console.log("I was called!");
    return num + 5;
  };

  return (
    <div className={light ? "light" : "dark"}>
      <div>
        <h1>Without useCallback </h1>
        <h2>
          Current number: {num},
          <SomeComp someFunc={plusFive} />
        </h2>
        <div className="button-container">
          <button
            onClick={() => {
              setNum(num + 1);
            }}
          >
            Update Number
          </button>
          <button
            onClick={() => {
              setLight(!light);
            }}
          >
            Toggle the light
          </button>
        </div>
      </div>
    </div>
  );
}

// Child component
const SomeComp = ({ someFunc }) => {
  const [calcNum, setCalcNum] = useState(0);
  useEffect(() => {
    setCalcNum(someFunc());
  }, [someFunc]);

  return <span> Plus five: {calcNum}</span>;
};
```

We have a function `plusFive()` that's being passed to a child component as props.
The `SomeComp` child component has `useEffect` that depends on the props function `plusFive()` that's being passed in. Whenever the `Update Number` button is clicked which updates the `num` state, it executes the `plusFive()` function since it triggers a side effect in the `useEffect` of the child component.
Well, this is fine, but notice that `Toggle the light` button also recreates and `plusFive()` function because the parent component re-renders after `light` state updates, which also re-renders the child component.
This is critical if that `plusFive()` function does some heavy calculation.

We can prevent our function from being recreated and change pointers on each render round, we need to pass in our function in the `useCallback()` hook.

```javascript
const plusFive = useCallback(() => {
  console.log("I was called!");
  return num + 5;
}, [num]);
```

This hooks will preserve the function, and it will be recreated only when one of the dependencies changes.

## Becareful!

Don't overuse these hooks unless doing some heavy calculations.
functions will be garbage collected when not using `useCallback`, but using it will store in a memory. Also, only use `useMemo` whenever trying to prevent re-running some expensive calculation functions that run a lot of times, or using a lot of resources.

## Summarize

### useMemo()
Keeps a function from being executed again if it didn't receive a set of parameters that were previously used. It returns the results of a function. Use it when you want to prevent some heavy or costly operations from being called on each render.

### useCallback()
Keeps a function from being re-created again, based on a list of dependencies. It returns the function itself. Use it when you want to propagate it to child components, and prevent from a costly function from re-rendering.