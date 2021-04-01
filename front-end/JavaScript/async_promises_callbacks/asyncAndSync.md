# Asynchronous & Synchronous

### Understanding Asynchronous and Synchronous Code Execution

JavaScript is "Single-Threaded". It means that JS can only execute one task at a time - code executes in step by step, in order.
Say we have a `setTimeout()` or HTTP request(waiting for response) and there's a function that doesn't need to wait for these functions to execute. This is not good if JS was asynchronous. Since JS synchronous, it would execute other functions in order and have the `setTimeout()` or HTTP request aside and have it managed in the browser detached from the running JS code.
The browser needs a way for it to communicate back to the JS code. That's when callback is handy. The idea is that for example, `setTimeout()`, we pass a callback function and this function is the function the browser should call once the operation is done.

Say if we have scenario like below,

```javascript
const button = document.querySelector('button');
const output = document.querySelector('p');

function trackUserHandler(){
    console.log('Clicked!');
}

button.addEventListener('click', trackUserHandler);

let result = 0;
for (let i = 0; i < 10000000; i++){
    result += 1;
}

console.log(result);
```

If we click the button whenever the page loads, the `console.log('Clicked!')` doesn't execute until the for loop ends.
Basically, we tell the browser that `trackUserHandler` is a function when click occurs on the button. Then, we continue with the JS runs the for loop. The callstack is now **not** empty since its got the for loop that runs and have the `trackUserHandler` function registered in the "todo" onto the message queue. Now here comes the `event loop` that sees that there's a "todo" task in the message queue and waits for that function to execute in message queue to be empty.
That's why even though we click that button, the `console log` doesn't show until the for loop is done.

Another example,
```javascript
const handleClick = () => {
  navigator.geolocation.getCurrentPosition(
    (posData) => {
      console.log("posData: ", posData);
    },
    (error) => {
      console.log("error: ", error);
    }
  );
  console.log("Getting Position...");
};
```

The `console.log("Getting Position...")` executes first no matter what. JS doesn't wait for the `posData` or `error` consoles.

Say we have a `setTimeout()` used in the example.

```javascript
const handleClick = () => {
  navigator.geolocation.getCurrentPosition(
    (posData) => {
      setTimeout(() => {
        console.log("posData after 2000"); // 4
      }, 2000);
      console.log("posData: ", posData); // 3
    },
    (error) => {
      console.log("error: ", error);
    }
  );
  setTimeout(() => {
    console.log("Timer done!!"); // 2
  }, 0);
  console.log("Getting Position..."); // 1
};
```
If we enable the position, it logs in the order above. Notice that even though we have a `0` second(s) in the timer, it still executes after the `"Getting Position..."`. This is because it's not really a "none" `setTimeout`, but it's just a minimum time that the `setTimeout` function can accept.

