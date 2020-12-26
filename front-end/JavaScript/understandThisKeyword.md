# Understanding `this` keyword

In default(non strict mode), `this` keyword points at the `window` object.

#### Basic Example

`this` keyword refers to whichever is responsible for calling the function. Like the example below.
```javascript
const person = {
    name: 'DK',
    age: 30,
    message: function(){
        return `Hello, ${this.name}`;
    }
}

const greetings = person.message();
console.log(greetings); // "Hello, DK"
```
Basically, `this` refers to whatever the thing is calling in front which in this case will be `person`.

#### Shorthand

```javascript
const person = {
    name: 'DK',
    age: 30,
    message(){ // <-- not really same as above though
        return `Hello, ${this.name}`;
    }
}
```

#### When destructuring the method

Say when you destructure a method inside your object,

```javascript
const { message } = person;

const greetings = message();
console.log(greetings);
// "Hello, "
```
As explained above, `this` keyword refers to whatever called the function that contains `this` (`person.message()` <-- here, `this` refers to `person`), but when destructured, it doesn't really "call" the function.
This is due to `this` keyword pointing at the `window` object(when not in strict mode).
There are couple of ways to fix this.

##### - using `.bind()`

```javascript
let { message } = person;
message = message.bind(person);

const greetings = message();
console.log(greetings);
// "Hello, DK"
```
`.bind()` method prepares a function for future execution. Basically returns a new function object.
It takes in couple of arguments and normally we pass in `this`, but for this case, by passinig in `this` still points to the `window` object. Therefore, we need to pass in the `person` for it to point at the `person` object.
There are different ways of adding `.bind()` method, but it'll do the magic for this case.

##### - using `.call()`

```javascript
const { message } = person;

const greetings = message.call(person);
console.log(greetings);
// "Hello, DK"
```
`.call()` method goes ahead and executes the function right away.
It allows to pass additional arguments as comma separated list (this, , , )

##### - using `.apply()`

```javascript
const { message } = person;

const greetings = message.apply(person);
console.log(greetings);
// "Hello, DK"
```
`.apply()` works a bit similar to `.call()` method.
Unlike the other methods, it only takes two arguments - arg1: `this` which it refers to, and arg2: `[]` which can hold different arguments that this function might be taking.

## `this` keyword with event listeners

It behaves a bit differently with event listeners. Take a look at the code below.

**ES5**

```javascript
const searchMovieHandler = function () {
  console.log(this);
  (...)
};

const searchMovieBtn = document.getElementById("search-btn");
searchMovieBtn.addEventListener("click", searchMovieHandler);
```

The `this` keyword in this case does NOT point to the global context, but it points to the element that's responsible for triggering this event. So, it will console log `<button id='search-btn'>Search</buton>` 

**ES6**

```javascript
const searchMovieHandler = () => {
  console.log(this);
  (...)
};

const searchMovieBtn = document.getElementById("search-btn");
searchMovieBtn.addEventListener("click", searchMovieHandler);
```
Using an arrow function, `this` will NOT point to the element that's triggering, but it will point to the global context which is the `window` object.
console logs `Window {window: Window, self: Window, document: document, name: "", location: Location, …}`
Basically, arrow function does NOT know `this` keyword. 😎

## Times when an arrow function is useful!!! 

```javascript
const members = {
    position: 'striker',
    players: ['DK', 'Son'],
    getPlayers() {
        this.players.forEach(function(p) {
            console.log(`${p} - ${this.position}`)
        })
    }
}

members.getPlayers()
// DK - undefined
// Son - undefined
```

In the code above, the `this` in `this.position` does not point to `memebers`, but it points to the global context, the `window` object. This is where an arrow function comes in handy. I think that's because `this` in `this.position` is pointing at the `forEach()` method.

```javascript
const members = {
    position: 'striker',
    players: ['DK', 'Son'],
    getPlayers() {
        this.players.forEach((p) => {
            console.log(`${p} - ${this.position}`)
        })
    }
}

members.getPlayers()
// DK - striker
// Son - striker
```