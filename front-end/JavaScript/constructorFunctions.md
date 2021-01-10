# Constructor Function

It is a blueprint for objects and has properties and methods just like class.

## Example

```javascript
// class Person {
//   name = "DK";

//   constructor() {
//     this.age = 31;
//   }

//   greet() {
//     console.log(`Hello, I'm ${this.name}, and I'm only ${this.age} years old`);
//   }
// }

function Person() {
  this.name = "DK";
  this.age = 31;
  this.greet = function () {
    console.log(`Hello, I'm ${this.name}, and I'm only ${this.age} years old`);
  };
}

const me = new Person();
me.greet();
```

It's a convention to capitalize the name of constructor function. Alos, just like you would do in class, you initialize it with the `new` keyword to return an object.

The way the `new` keyword works is that it creates a `this` keyword and assigns an object that's going to be created like `this = {}`. Then, it adds all the properties to the empty object and returns `this`.
```javascript
function Person() {
  this = {};
  (...)
  return this;
}
```