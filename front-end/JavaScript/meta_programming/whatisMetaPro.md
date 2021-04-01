# Meta-Programming

This is an advanced topic. It's really for people who build libraries and etc.

- Symbols, Iterators & Generators, Reflex API, Proxy API.
These are features that allow you to change the way certain parts of your code behave like from API stand point. 

## Symbols
Symbols are primitive values. It can be used as object property identifiers and it's built-in & created by developers. Its uniqueness is guaranteed which means, cannot over write.

```javascript
const uid = Symbol();

console.log(uid); // Symbol()
```

`Symbol()` creates a new "symbol" (notice we called it without the `new` keyword).

Let's imagine you are building a library and you expose certain objects.
In the objects you expose and have certain keys or ids. If you need to ensure the users don't overwrite the id or key.

here's an example.

```javascript
const user = {
  id: 'p1',
  name: 'DK',
  age: 30,
};
```

Let's say we have an object like above. We can easily change a value in the object by `user.id = 'p2';`. We want to prevent from this.

```javascript
const uid = Symbol('uid');

const user = {
  [uid]: 'p1',
  name: 'DK',
  age: 30,
};

console.log(user);
console.log(user[Symbol('uid')]); // undefined
console.log(Symbol('uid') === Symbol('uid')); // false
```

If we try to access the `Symbol('uid')`, it's undefined.
Also, it returns false if we compare the symbol that looks the same.
Of course we can change the value of the symbol "inside the library",

```javascript
const uid = Symbol('uid');

const user = {
  [uid]: 'p1',
  name: 'DK',
  age: 30,
};
user[uid] = 'p3';
```

## Iterators

...
