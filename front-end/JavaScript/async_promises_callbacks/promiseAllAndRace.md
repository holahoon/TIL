# More on Promises

### Using `.race()`

`Promise.race()` accepts elements in an array and returns a promise with the result of the fastest promise that's been passed to it. All the elements in an array will be kicked off at the same time and will return the data that's the data from the fastest promise.

```javascript
const func1 = (duration) => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("function 1");
    }, duration);
  });

  return promise;
};

const func2 = (duration) => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("function 2");
    }, duration);
  });

  return promise;
};

Promise.race([func1(2000), func2(4000)]).then((data) => console.log(data))

// function 1
```

### Using `.all()`

Returns an array with the all the data.

```javascript
Promise.all([func1(2000), func2(4000)])
  .then((data) => console.log(data))
  .catch((err) => console.log(err));

// ["function 1", "function 2"]
```

Note that if one of the promises within gets rejected, then the promise that's been rejected will automatically get handled in the `catch()` block followed by the `then()` block and ignore the other promises.
```javascript
const func1 = (duration) => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("error in function 1"); // <- rejected
    }, duration);
  });

  return promise;
};

Promise.all([func1(2000), func2(4000)])
  .then((data) => console.log(data))
  .catch((err) => console.log(err));

// error in function 1
```

### Using `.allSettled()`

```javascript
const func1 = (duration) => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      reject("error in function 1"); // <- rejected
    }, duration);
  });

  return promise;
};

Promise.allSettled([func1(2000), func2(4000)])
  .then((data) => console.log(data))
  .catch((err) => console.log(err));

// (2) [{…}, {…}]
// 0: {status: "rejected", reason: "error in function 1"}
// 1: {status: "fulfilled", value: "function 2"}
```

Note that even though one of the promise gets rejected, it still continues to execute other promises. Returns an array with a summary of which failed and which resolved.