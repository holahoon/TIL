# Promises

This was introduced to avoid a callback hell like a scenario below.

```javascript
getCurrentPosition(() => {
    setTimeout(() => {
        doMoreAsyncFn(() => {
            ...
        });
    }, 2000);
}, ...);

// vs

someAsyncTask()
.then(() => {
    return anotherTask1();
})
.then(() => {
    return anotherTask2();
})
.then(...);
```

It'd be really hard to read and understand the code if there's a multiple callback functions. Using promise can easily solve this issue when working with async code.

Here's an example of using creating promise and using it.

```javascript
const setTimer = (duration) => {
    const promise = new Promise((resolve, reject) => {    
        setTimeout(() = >{
            resolve('Completed!');
        }, duration);
    });

    return promise; // return the promise for it to be used
};
```

The `new Promise()` constructor function takes a function as an argument which receives two arguments - `resolve` and `reject` functions (*these names are up to me to name them, but I would go with the convention*). Obviously, the `resolve()` function gets called if no error occurs and vice versa. Whenever we chain the `.then()` method after calling the promise, if it succeeds, it will return whatever that's been passed into the `resolve` method.
If we were to use this `promise`,

```javascript
const handleClick = () => {
  navigator.geolocation.getCurrentPosition(
    (posData) => {
      setTimer(2000).then((data) => {
        console.log("got data: ", data); // 3 - after 2 seconds
        console.log("posData: ", posData); // 4
      });
    },
    (error) => {
      console.log("error: ", error);
    }
  );
  setTimer(0).then(() => console.log("timer done!")); // 2 - after 0 seconds
  console.log("Getting Position..."); // 1
};
```
will call in the order above.

### Promise Chaining

This chaining simply as the word describes, it chains promises after promises.
Let's take a look at an example.

```javascript
const setTimer = (duration) => {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("Done!"); // will be returned when .then() method is called on this promise (data)
    }, duration);
  });

  return promise;
};

const getPosition = (opts) => {
  const promise = new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(
      (success) => {
        resolve(success); // will be returned when .then() method is called on this promise (posData)
      },
      (error) => {},
      opts
    );
  });

  return promise;
};

const handleClick = () => {
  let positionData;
  getPosition()
    .then((posData) => {
      positionData = posData; // resolve(success) - success gets passed into the variable
      return setTimer(2000);
    })
    .then((data) => {
      console.log("posData: ", positionData); // has access to the positionData variable since it was declared outside of the scope
      console.log("data: ", data);
    });
  setTimer(0).then(() => console.log("timer done!"));
  console.log("Getting Position...");
};

button.addEventListener("click", handleClick);
```

Whatever gets passed into `resolve()` method will be returned when `.then()` method is called - success.
When we return a promise within promise, we can chain the returned promise - `setTimer` and use `.then()` which we can then call the `resolve()` method in `setTimer` promise. Basically, the `getPosition` promise is now pending in until the returned `setTimer` is done. The console will log as below.

```html
Getting Position...
timer done!
posData:  GeolocationPosition {coords: GeolocationCoordinates, timestamp: 1613296637689}
data: Done1
```

The posData and data will log 2 seconds after gelocation was resolved.
Also note that we dont' need to return a `promise` like above, we can always return any king of data and it will be converted to a `promise` and wrapped into a `promise`.

### Handling Error

This is done with `reject` argument.

```javascript
const getPosition = (opts) => {
  const promise = new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(
      (success) => {
        resolve(success); // will be returned when .then() method is called on this promise (posData)
      },
      (error) => {
        reject(error); // when error, this reject() method gets called and has access to the error
      },
      opts
    );
  });

  return promise;
};

const handleClick = () => {
  let positionData;
  getPosition()
    .then(
      (posData) => {
        positionData = posData; // resolve(success) - success gets passed into the variable
        return setTimer(2000);
      },
      //   (err) => {
      //     console.log(err);
      //   }
    )
    .catch((error) => console.log(error))
    .then((data) => {
      console.log("posData: ", positionData); // has access to the positionData variable since it was declared outside of the scope
      console.log("data: ", data);
    });
  setTimer(0).then(() => console.log("timer done!"));
  console.log("Getting Position...");
};
```

Passing in error handler as a second argument works (like above - commented line). But it will be harder to read as well.
A better approach is to use the `.catch()` method. Whenever `reject` gets called, it will skip all of the `.then()` methods and will find `.catch()` method to handle error.
Note, that `.catch()` method doesn't *cancel* the chain when error occurs, but it rather just does its job. So the next `.then()` method gets called. Basically, all of the prior `.then()` methods will be skipped, but the `.then()` methods after the `.catch()` will be called.
The position of the `.catch()` method is important if we want to handle an error.
Of course we can handle some error logics in the following `then()` block by returning some data inside the `catch()` block, but most of the times, we might want to end the chain by calling the `catch()` block at the end.

### Important !!!!!

>Keep in mind that both `.then()` and `.catch()` returns a new promise - either not resolving to anything or resolving to what you `return` inside of `.then()`.

Also, I have never used this, but there's a special block called `.finally()`. This unlike the other blocks doesn't return a new promise. It is reached no matter if you resolved or rejected before. This is used to do a final clean up work.
Don't have to use it though.