# React Performance Optimization

We may run into a scenario where we need to render a lot of components on the screen at once. We may consider using `pagination` or `virtualization` for most of the cases, but what if that's not the case. What would be a way to not give up on UX and performance and still render a chunk of components?

## Get Started

Let's just build a simple app that renders 10k square boxes.

#### App.js
```jsx
import { useState, useCallback } from "react";

import Row from "./components/row/Row";

// function that makes 
const data = Array(10000)
  .fill()
  .map((_, idx) => ({ id: idx, key: `square-${idx}` }));

export default function App() {
  const [count, setCount] = useState(0);

  const squareClickHandler = useCallback(() => setCount((val) => val + 1), []);

  return (
    <div>
      <p>Count: {count}</p>

      {data.map(({ key }) => (
        <Square key={key} squareClickHandler={squareClickHandler} />
      ))}
    </div>
  );
}
```
> [Array( )]() creates an empty array with x amount of elements inside
> [fill( )](https://www.w3schools.com/jsref/jsref_fill.asp) fills the array with some values passed in(if not, undefined gets passed in)

#### square.css
```css
.square {
  border: 2px solid gray;
  border-radius: 1px;
  width: 20px;
  height: 20px;
  display: inline-block;
  margin: 3px;
  cursor: pointer;
}

.square:active {
  background-color: springgreen;
}
```

#### Square.js
```jsx
import "./square.css";

export default function Square({ squareClickHandler }) {  
  return <div className='square' onClick={squareClickHandler}></div>;
};
```

Say we have this very simple `Square` component being rendered 10k times in the parent container - `App`.

When I tried to run the app, it automatically complains that it's rendering too much and if I console log inside Square component, it just re-renders 10k again and again whenever the parent container(`App.js`) changes its state.
Not realistic to render a component 10k times, but it's got some room to improve.

## The Improvement

Let's go ahead and take an advantage of using React hooks: `useCallback` and `memo` to prevent unnecessary re-renders.
I'm going to restructure the app as: `[App.js]` -> `[Row.js]` -> `[Square.js]`

#### App.js
```jsx
import { useState, useCallback } from "react";

import Row from "./components/row/Row";

const data = Array(10000)
  .fill()
  .map((_, idx) => ({ id: idx, key: `square-${idx}` }));

const chunkArray = (array, chunkSize) => {
  const results = [];
  let idx = 1;

  while (array.length) {
    // cut array from index 0 ~ x and store it in 'items' key
    // push the object into a new array 'results'
    results.push({
      items: array.splice(0, chunkSize),
      key: String(idx),
    });
    idx++; // increment index by 1 on every push
  }
  return results;
};

// Get new array of data after splicing
const showItems = chunkArray(data, 500);

function App() {
  const [count, setCount] = useState(0);

  const squareClickHandler = useCallback(() => setCount((val) => val + 1), []);

  return (
    <div>
      <p>Count: {count}</p>

      {showItems.map(({ items, key }) => (
        <Row key={key} items={items} squareClickHandler={squareClickHandler} />
      ))}
    </div>
  );
}

export default App;
```
> `chunkArray` function takes two args: array to use and how many square boxes need to be shown. It returns an array with object as elements.

#### Row.js
```jsx
import { memo } from "react";

import Square from "../square/Square";

export default memo(function Row({ items, squareClickHandler }) {  
  return (
    <>
      {items.map(({ _, key }) => (
        <Square key={key} squareClickHandler={squareClickHandler} />
      ))}
    </>
  );
});
```

#### Square.js
```jsx
import { memo } from "react";

import "./square.css";

export default memo(function Square({ squareClickHandler }) {  
  return <div className='square' onClick={squareClickHandler}></div>;
});
```

`Row.js` renders depending on the length of the `showItems` array and `Square.js` renders depending on the `chunkSize` argument.
Now if I open up Chrome react Profiler and test out how fast it took to render, we can see that it took so much shorter.