# Pure Functions & Side-Effects

## What is Pure Function?

Is a function that receives the same input and produces the same output. Also, it doesn't trigger any side-effects meaning it doesn't change anything outside of the function.

#### Examples

##### Pure function

```javascript
function add(num1, num2){
    return num1 + num2;
}
```

##### Impure function

```javascript
function addRandom(num1){
    return num1 + Math.random();
}
```

```javascript
let prevResult = 0;

function addMore(num1, num2){
    const sum = num1 + num2;
    prevResult = sum; // <- causes side-effect
    return sum;
}
```
above causes "side-effect" which changes a variable outside the function.

```javascript
const hobbies = ["Sports", "Cooking"];

function printHobbies(hob) {
  hob.push("New Hobby");
}

printHobbies(hobbies);
```
Because we change the original array since array is an object and it's a reference value. Above example doesn't copy the original array but rather it mutates.