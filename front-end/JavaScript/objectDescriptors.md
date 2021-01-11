# Object Descriptors

#### [reference MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

Every property or method(basically a property that holds a function) you add, has a `descriptor`.
**This is useful when in need of locking down a property. (rarely)**

Say we have a pretty basic object like below.
```javascript
const person = {
    name: 'DK',
    greet(){
        console.log(person.name)
    }
}

person.greet(); // 'DK'
```

We can access the descriptor by using the following,
```javascript
/*
* @param1: object
* @param2: key of the object
*/
Object.getOwnPropertyDescriptor(person, 'name');
// OR using getOwnPropertyDescriptor"s", you can just pass in the object
Object.getOwnPropertyDescriptors(person);
```

This will return an object so called `descriptor`. For the case above, it will return,
```javascript
{
    greet: {
        configurable: true
        enumerable: true
        value: ƒ greet()
        writable: true
        __proto__: Object
    },
    name: {
        configurable: true
        enumerable: true
        value: "DK"
        writable: true
        __proto__: Object
    }
__proto__: Object
}
```

Let's take a look at `name`.
`value` - holds a value of `'DK'`
`writable` - can assign a new value
`configurable` - for example, can delete it
`enumerable` - appears in a `for in` loop

Now, we can modify this `descriptor` by using `Object.defineProperty()` which access the object, the property you want to modify, and an object to specify what you want to do.

```javascript
Object.defineProperty(person, 'name', {
    configurable: true,
    enumerable: true,
    value: person.name,
    writable: false
})
```

Say we set `writable` to `false`.
Let's try to change the value of the `person` object to `"David"`.

```javascript
person.name = "David";
console.log(person.name); // "DK"
```

It'll still show "DK" because it's been locked down.

Now, Let's try to set `configurable` to `false`.
```javascript
Object.defineProperty(person, 'name', {
    configurable: false,
    enumerable: true,
    value: person.name,
    writable: false
})
```

if we try to `delete person.name;` to delete the name property, it'll still be there.

Also, let's add a property called `occupation: 'dev'` and set `enumerable` to `false` for the `greet()` method,

```javascript
person.occupation = 'dev'; // person: {name: 'DK', occupation: 'dev', greet(){...}}

Object.defineProperty(person, 'greet', {
    configurable: true,
    enumerable: false,
    value: person.greet, // still holds the value of the greet() method
    writable: true
})
```

Let's use `for in` loop to iterate through the keys.

```javascript
for (const key in person){
    console.log(key);
}
// DK, dev, undefined
```

This basically skips the greet() method when iterating through key.