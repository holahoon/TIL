# Async & Await Function

Looks like synchronous code (just a normal code), but it still utilizes promises.

```javascript
// Expression
const func1 = async () => {
    ... await
}
// this is now a promise - function func1(): Promise<void>

// Declaration
async function func2 (){
    ... await
}
// this is now a promise - function func2(): Promise<void>
```

### Example

Refer to the promise example in [Promises](./promises.md),

```javascript
const handleClick = () => {
  let positionData;

  getPosition()
  .then((posData) => {
    positionData = posData; // resolve(success) - success gets passed into the variable
    return setTimer(2000);
  })
  .catch((error) => {
    console.log(error);
    return "on we go..";
  })
  .then((data) => {
    console.log("posData: ", positionData); // has access to the positionData variable since it was declared outside of the scope
    console.log("data: ", data);
  });
};

// becomes ->

const handleClick = async () => {
  //   let positionData;
  const posData = await getPosition();
  const timerData = await setTimer(2000);
  console.log("posData: ", posData);
  console.log("timerData: ", timerData);
};
```

It just transforms the promise behind the scenes. It just looks different in our eyes, but it actually pretty much does the same thing as promise.then() behind the scenes.

### Error Handling

When handling error, we can just use the normal synchronous code.

```javascript
const handleClick = async () => {
  let posData;
  let timerData;

  try {
    // when resolves
    posData = await getPosition();
    timerData = await setTimer(2000);
  } catch (error) {
    // when rejects
    console.log(error);
  }
  // these codes will ALWAYS run after
  console.log("posData: ", posData);
  console.log("timerData: ", timerData);
}
```

Using `trycatch()` is recommended though in my opinion.

