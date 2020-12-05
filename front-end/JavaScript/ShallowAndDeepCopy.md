# JavaScript - Deep Copy

reference: [medium article](https://medium.com/@manjuladube/understanding-deep-and-shallow-copy-in-javascript-13438bad941c)

## What the eff is Shallow and Deep Copy?
- **Shallow Copy:** is a bit-wise copy of an object. A new object is created that has an exact copy of the values in the original object. If any of the fields of the object are references to other objects, just the reference addresses are copied i.e., only the memory address is copied.
- **Deep Copy:** A deep copy copies all fields, and makes copies of dynamically allocated memory pointed to by the fields. A deep copy occurrs when an object is copied along with the objects to which it refers.

#### Let's take an example.
**Shallow Copy** makes a copy of the reference to X into Y. So the address of X and Y will be the same(which they point to the same memory location). **Deep Copy** on the other hand makes a copy of all the members of X, allocates different memory location for Y and then assigns copied members to Y to achieve deep copy. By doing this, if X is vanished, Y still remains in the memory. They both point to different memory location.

>Remember, object and array are reference types, which means they don't hold values, but they are just a pointer to the value in the memory.
> Also, note that spread operator only goes one level deep.

## Array
```javascript
const array1 = ['a', 'b', 'c']
const array2 = array1 // do NOT do it this way!
const array3 = array1.slice() // ES5
const array4 = [...array1] // ES6
const array5 = Array.from(array1)

console.log(array1 === array2) // true
console.log(array1 === array3) // false
console.log(array1 === array4) // false

array1[0] = 'AA'

console.log(array1) // ['AA', 'b', 'c']
console.log(array2) // ['AA', 'b', 'c'] <- Because array2 points at the same location
console.log(array3) // ['a', 'b', 'c']
console.log(array4) // ['a', 'b', 'c']
```
Informations about the methods used: [Array.from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from), [spread sytax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax), [slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

## Object
```javascript
const obj1 = {
  name: 'DK',
  age: 30
}
const obj2 = obj1
const obj3 = Object.assign({}, obj1)
const obj4 = {...obj1}

console.log(obj1 === obj2) // true
console.log(obj1 === obj3) // false
console.log(obj1 === obj4) // false

obj1.name = 'Chris'

console.log(obj1.name) // "Chris"
console.log(obj2.name) // "Chris"
console.log(obj3.name) // "DK"
console.log(obj4.name) // "DK"
```
Informations about the methods used: [Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

## Tips,
Cloning an object or array could be quite cumbersome. If it isn't a problem to use a JavaScript utility library, using [lodash](https://lodash.com/) is pretty recommended. They provided many useful functions, but for cloning: they provide [_.clone()](https://lodash.com/docs/4.17.15#clone) and [_.cloneDeep()](https://lodash.com/docs/4.17.15#cloneDeep).