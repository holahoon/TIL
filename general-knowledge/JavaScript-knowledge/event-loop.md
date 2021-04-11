# Event Loop

[reference](https://hackernoon.com/understanding-js-the-event-loop-959beae3ac40)

So, I've had a pretty good understanding of what call stack and event queue are. But, what the heck exactly is an event loop?

After reading the article above, it made a clear sense what an event loop is.

Basically, we know that functions are pushed into the call stack which is array-like data structure but with some limitations.
But what if we have async functions that needs to be executed such as `setTimeout`. Theoretically, it should wait until the timer is finished and freeze the entire process, but that's not how it works right?
Well, that's because all of async operations are added to the **event table**. Once the event occurs (timeout, click, mousemove, etc...), it sends notice. However, the event table does not execute functions, but its sole purpose is to keep track of events and send them to the **event queue**.
The event queue is also a data structure similar to the stack. You add items to the back but can only remove items from the front. It receives the function calls from the event table, but it somehow needs to send them to the call stack.

*This is where event loop comes in!*

Event loop constantly runs by looking at the call stack and see if the call stack is empty. If it is, it then checks the event queue. If there's something in the event queue waiting, it is moved to the call stack.

Pretty cool huh?
yup.