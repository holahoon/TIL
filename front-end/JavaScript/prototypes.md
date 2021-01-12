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
`.toString()` works because somehow the `me` object has that method. If we console log `me`, we'll get the following property.

```javascript
age: 31
greet: ƒ ()
name: "DK"
__proto__: Object // <--
```

This `__proto__` shouldn't be used, but it's more of a connected object that we can reach out to ask to use that doesn't exist in our object.

Basically, say we create a constructor function or class and we instantiate it like
```javascript
const me = new Person;
```
and we call
```javascript
me.talk();
```
what JavaScript will do is that it will look for the `talk()` and see if it's defined in `Person` itself (i.e. set up in the blue print). If it is, it will trigger the function, but if it's not, it will look for `p.__proto__.talk()` which is the prototype of the `Person`. If it doesn't find the function, it goes deeper and deepr `p.__proto__.__proto__.talk()`... until it reaches Object.prototype (global "Object").

`__proto__` is present in EVERY object you create no matter what it is.

Now, if we do the following to look inside the `Person` constructor function,
```javascript
console.dir(Person);
```
it shows something different
```javascript
arguments: null
caller: null
length: 0
name: "Person"
prototype: {constructor: ƒ} // <--
__proto__: ƒ ()
```

This `prototype` ONLY exists in function objects. This is pretty much a fallback object.
Say we do the following,

```javascript
Person.prototype = {
    printAge() {
        console.log(this.age);
    }
};
```

it shows the following,

```javascript
arguments: (...)
caller: (...)
length: 0
name: "printAge"
__proto__: ƒ ()
[[FunctionLocation]]: app.js:22
[[Scopes]]: Scopes[2]
```

And we can call it like

```javascript
me.printAge();
// 31
```

However the above code replaces the old default object which is assigned as prototype with a new object. A better approach is to reach out to prototype and dynamically add the new property or method.
```javascript
Person.prototype.printAge = function() {
    console.log(this.age);
};
```

## Summarize

Prototypes can be a confusing and tricky topic - that's why it's important to really understand them.

A prototype is an object (let's call it `"P"`) that is linked to another object (let's call it `"O"`) - it (the prototype object) kind of acts as a "fallback object" to which the other object (`"O"`) can reach out if you try to work with a property or method that's not defined on the object (`"O"`) itself.

EVERY object in JavaScript by default has such a fallback object (i.e. a prototype object) - more on that in the next lectures.

It can be especially confusing when we look at how you configure the prototype objects for "to be created" objects based on constructor functions (that is done via the .prototype property of the constructor function object).

Consider this example:
```javascript
function User() {
    ... // some logic, doesn't matter => configures which properties etc. user objects will have
}
User.prototype = { age: 30 }; // sets prototype object for "to be created" user objects, NOT for User function object
```
The User function here also has a prototype object of course (i.e. a connected fallback object) - but that is NOT the object the prototype property points at. Instead, you access the connected fallback/ prototype object via the special __proto__ property which EVERY object (remember, functions are objects) has.

The prototype property does something different: It sets the prototype object, which new objects created with this User constructor function will have.

That means:
```javascript
const userA = new User();
userA.__proto__ === User.prototype; // true
userA.__proto__ === User.__proto__ // false
```