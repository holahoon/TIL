# Methods in Classes & in Constructors

```javascript
class Person {
  name = "David";

  constructor() {
    this.age = 30;
  }

  greet = function () {
    console.log(`Hey! I'm ${this.name}, and ${this.age} years old.`);
  };
  greetArrow = () => {
    console.log(
      `Hey! I'm ${this.name}, and ${this.age} years old. - from arrow function`
    );
  };
  greet2() {
    console.log(
      `Hey! I'm ${this.name}, and ${this.age} years old. - from greet2 function`
    );
  }
}

const p = new Person();
const p2 = new Person();
p.greet();

console.log(p);
console.log(p.__proto__ === p2.__proto__); // true
```

In console, it prints
```
{    
    age: 30
    greet: ƒ ()
    name: "David"
    __proto__: {
        constructor: class Person
        greet2: ƒ greet2()
        __proto__: Object
    }
}
```

Take a look at how `greet()` is part of the object, but `greet2()` is part of the prototype of the object. This means that `greet()` will be created for EVERY instance of the object(based on the `Person` class). This occupies more memory, but it depends on the project you build. However, not a huge impact, just kb or milliseconds even in less than that.

Now, take a look at `greetArrow()` arrow method, the `this` keyword automatically refers to the surrounded object. This is helpful when we bind this method to event listeners where we typically need to use `bind()` or `add()` methods. Using `bind()` is better in performance, but using arrow function could be better in readability.

```html
<button id="btn">greet me!</button>
```

```javascript
const btn = document.getElementById("btn");
btn.addEventListener("click", p.greetArrow);
btn.addEventListener("click", p.greet2.bind(p));
```

Just a personal preference.

## Summary of Method & Property Function(and arrow)

#### Method
```javascript
class Person {
    greet(){
        console.log("I'm method!");
    }
}
```
Assigned to `Person`'s prototype and hence shared across all instances (NOT re-created per instance).

#### Property Function
```javascript
class Person {
    greet = function() {...}
    constructor() {
        this.greet2 = function() {...}
    }
}
```
Assigned to individual instances and hence re-created per object. `this` refers to "what called the method".

#### Property Arrow Function
```javascript
class Person {
    greet = () => {...}
    constructor() {
        this.greet2 = () => {...}
    }
}
```
Assigned to individual instances and hence re-created per object. `this` always refers to instance.