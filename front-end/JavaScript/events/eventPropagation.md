# Event Propagation

It's important to know that all listeners you add with `addEventListner` are by default registered in `bubbling` phase. The browser ignores the capturing phase. This means that if I have a nested element like 

```html
<section>
  <div>
    <button></button>
  </div>
</section>
```
Say we have an event listener on `section` and on `button`. The `button` event listener will run first then `section` event listener will run second.

### example
assume that we have a `div` that's in the higher order than the `button`

```javascript
const div = document.querySelector("div").addEventListener(
  "click",
  (event) => {
    console.log("div: ", event);
  }
);

const btn = document
  .getElementById("btn")
  .addEventListener("click", (event) => {
    console.log("button: ", event);
  });

```

When clicking on the `button`, it will console log `("button: ", event)` and also will console log `("div: ", event)`. This is due to browser going through the `bubbling` phase.
If we add a third argument in the `addEventListener()`,

```javascript
const div = document.querySelector("div").addEventListener(
  "click",
  (event) => {
    console.log("div: ", event);
  },
  true
);
```
We tell the browser to `capture`. Basically, the `div` comes in the `capturing` phase(which comes before `bubbling`), and runs the `div` event first before the `button`. So the order of the console will be `div` -> `button`.

### To stop propagation
`Event propagation` basically is `bubbling`. It can be used in anywhere above in the ancestors.
If we need to prevent from `propagating(bubbling)`, we just add `event.stopPropagation()`. This will block any other listeners for the same type of event on some ancestor elements will NOT trigger their event listeners for this event.

```javascript
const btn = document
  .getElementById("btn")
  .addEventListener("click", (event) => {
    event.stopPropagation(); // <-
    console.log("button: ", event);
  });
```

Also, there's another method `event.stopImmediatePropagation()`. This is used when there are multiple event listeners on the same element(i.e. more event listeners on the `button` element), then after the first event listener, the other event listeners on the same element also wouldn't run anymore.

So, `event.stopPropagation()` - all event listeners will execute, just not on the ancestor.