# Working with Events

We know that there are many different events associated with browser - i.e `onclick`, `drag` and etc...

It's pretty common that we use `someElement.addEventListener('event', function);` to handle events.
There's other way of doing this as well.

```javascript
const buttonClickHandler = () => {
  console.log("button was clicked");
};

button.onclick = buttonClickHandler;
```

**This is NOT recommended** to use since it can only assign one function handler to the click event. Also, it can be really tricky when it comes to removing the event and stuff like that.

### recommended way of adding event

Rather it is recommended to use `addEventListner`

```javascript
button.addEventListener("dblclick", buttonClickHandler); // double click event btw
```

### remove event

We can also remove an event. Below removes an event after 2 seconds.

```javascript
button.removeEventListener('dblclick', buttonClickHandler);
```

Make sure to remove the same event and the same function that's been passed in.

**Be careful though**

```javascript
button.addEventListener("dblclick", () => {
    console.log('here');
});
button.removeEventListener('dblclick', () => {
    console.log('here');
}));
```
Keep in mind that if we pass in an anonymous function such as an arrow function, it will **not** be the same since they are two different objects.

Also,
```javascript
button.addEventListener("dblclick", buttonClickHandler.bind(this));
button.removeEventListener('dblclick', buttonClickHandler.bind(this));
```

This is **not** same as well. The `.bind(this, ...)` method creates a new function object.
One way of solve this issue to correctly remove the same function is to store the bound function on a variable.

```javascript
const boundFn = buttonClickHandler.bind(this);

button.addEventListener("dblclick", boundFn);
button.removeEventListener('dblclick', boundFn);
```
