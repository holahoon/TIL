# Microtasks, Macrotasks and queueMicrotask

[reference](https://medium.com/swlh/a-practical-guide-to-macro-tasks-micro-tasks-and-queuemicrotask-in-javascript-ca65c393699e)

Let's take a look at the below JavaScript code.

```javascript
console.log(1);
setTimeout(() => {
    console.log(2);
})
Promise.resolve().then(() => console.log(3));
console.log(4);
```

it will output result
```bash
1
4
3
undefined
2
```

Hmm... Its got two async functions - `setTimeout` and `Promise`.
Why does it log 3 then 2?
Let's take a look at why this happens.

The `setTimeout` is handled by the browser API. The browser API will handle the `setTimeout` and put the callback in the task queue if no timer is specified. When the timer expires, the browser api puts the callback in *macrotask* queue.

However for `Promise`, its callback is put in to the *microtask* queue.
Tasks from microtask queue are handled at the end of an execution cycle. They will run after the current task has completed its work when there's no code waiting to be ran before control of the **execution context** is returned to the browser's event loop. A task from macrotask queue will only be put in call stack if the call stack (**execution context stack**) is empty. After runing sync functions which prints 1 and 4, it processess microtask which prints 3, then the script returns - hence **undefined**. Now it will check if there's any new task available - which we still have the `setTimeout` in macrotask queue - prints 2.

Remember - **sync tasks** --> **micro tasks** --> **macro tasks**

## QueueMicrotask

This function will explicitly put a task in microtask queue. This is what happens when a promise resolves. If you chain multiple `then()` statements, they will all be put in the microtask queue and are guaranteed to be executed before handling control back to browser’s event loop (aka macotask).
However if you keep queuing tasks in the microtask forever, your browser will become unresponsive. Can't click buttons, can't scroll, nothing... GIFs will stop. The control will never be passed to browser rendnerer.

### Quiz

```javascript
console.log(1);

setTimeout(() => {
    queueMictrotask(() => console.log(2));
    console.log(3);
})

Promise.resolve().then(() => console.log(4));

queueMicrotask(() => {
    console.log(5);
    queueMicrotask(() => {
        console.log(6);
    })
})

console.log(7);
```

#### answer
```bash
1
7
4
5
6
undefined
3
2
```

1,7 are sync so get printed. The the microtask queue by the main execution context is checked. There are two types of micro tasks here one is the promise callback and the other is task queue by queueMicroTask, which further puts another task in micro task queue. So first 4 is printed, then 5 and as there is still a task remaining in microtask queue 6 is also printed. After this the main execution ends and undefined is printed and the program returns. Now it’s time to look for tasks in macrotask queue. Here the callback of setTimeout will be invoked. 3 is printed as its sync. The the microtask queue is checked again and 2 is printed finally.