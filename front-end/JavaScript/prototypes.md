# Prototypes

Every constructor function you build has a special prototype property which is not added to the objects you create based on it. A prototype is an object itself and every object has a prototype. This is how JavaScript creates code. It basically is a "fallback objects".
Let's say we have an object called `person` which has properties: `name` property and `greet()` method. And let's say we have a code that calls the `person` and calls `sayHello()` like `person.sayHello()`. Clearly, there is not `sayHello()` method in the `person` object. It should fail. Basically, JS automatically looks into the `protoype` of the object and searches for the property of method there.

Here's an example of using a constructor function.

```javascript
function Person() {
  this.name = "DK";
  this.age = 31;
  this.greet = function () {
    console.log(`Hello, I'm ${this.name}, and I'm only ${this.age} years old`);
  };
}

const me = new Person();
me.greet();
console.log(me.toString()); // [object Object]
console.log(me.sayHello()); // uncaught TypeError: me.sayHello is not a function
```

We can see that `me.toString()` clearly is not a function in our Person(), but at least it returns something. However `me.sayHello()` returns an error.

