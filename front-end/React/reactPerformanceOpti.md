# React Performance Optimization

We may run into a scenario where we need to render a lot of components on the screen at once. We may consider using `pagination` or `virtualization` for most of the cases, but what if that's not the case. What would be a way to not give up on UX and performance and still render a chunk of components?

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

export default function Square({ onSquareClick }) {  
  return <div className='square' onClick={onSquareClick}></div>;
};
```

Say we have this very simple `Square` component being rendered 10k times in the parent container - `App`.