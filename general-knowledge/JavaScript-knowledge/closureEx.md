# Closure Example

[reference](https://wesbos.com/for-of-es6)

We now know that `closure` is a term that an inner function looks for its closest parent scope when trying to access a variable that is not defined within itself.

Well, what about using `var` and `let` ?

## Problem

```javascript
function closure(){
    for (var i = 0; i < 10; i++){
        console.log(i)
    }
}
```

This will obviously console `i` from `0` ~ `9`.

How about if we have an async function inside like

```javascript
function closure(){
    for (var i = 0; i < 10; i++){
        setTimeout(() => {
            console.log(i)
        }, 1000)
    }
}
```

Well, we would expect it to console `0` ~ `9` after 1 second. Unfortunately, it doesn't.

This is due to `var` being a `functional scope`. When `var i = 0` was created in the `for` loop(`block scope`), variable `i` pretty much was created outside of the `for` loop.
Basically, it's like saying

```javascript
function closure(){
    var i; // <- kind of the way it is doing
    for (i = 0; i < 10; i++){
        setTimeout(() => {
            console.log(i)
        }, 1000)
    }
    console.log('var i: ', i) // 10
}
```

This `i` will have a value of `10` by the time the `for` loop ends which ends quicker than the `setTimeout()`.
So instead of consoling `0` ~ `9`, it console logs `10`s 9 times.
You might be thinking why is `i` a `10`? Well, inside the `for` loop, `i` goes up to 10 and does not execute the logic inside the loop since `i = 10` is not less than `10`. So `i` goes up to `10` and just exits out.

In conclusion, the iteration iterates until `i` is `10` and updates the variable `i`.

## Solution

We can use `let` instead of using `var`. `let` is `block scope` so that this variable is declared inside the `for` loop.

```javascript
function closure(){
    for (let i = 0; i < 10; i++){
        setTimeout(() => {
            console.log(i)
        }, 1000)
    }
    console.log('var i: ', i) // referenceError: i is not defined
}
```

By doing so, the `let i = 0` gets declared inside the block scoped `for` loop so that the `setTimeout` method still has its `closure scope` which can access the `i` value.

As we can see, `'var i:'` throws a referenceError since the `let i` is only available within the `for` loop.

Now, `closure scope` still remembers the value of the variable that the parent scope had, it still know what `i` was before it incremented.

Therefore, it will console log from `0` ~ `9` successfully.

### Bonus,

If we need to console log `i` every 1 sec, we can do so by
```javascript
function closure(){
    for (let i = 0; i < 10; i++){
        setTimeout(() => {
            console.log(i)
        }, i * 1000)
    }
}
```

`setTimeout()` only executes after x amount of miliseconds right? Since we increment `i`, it incrementally executes and console logs `i`.

## Workaround

What if we can't use `let` for whatever reason?

We can easily wrap a function that calls the `setTimeout()` and execute it.

```javascript
const exe = (i) => {
  setTimeout(() => {
    console.log('var i: ', i);
  }, i * 1000);
};

const closure = () => {
  for (var i = 0; i < 10; i++) {
    exe(i);
  }
};

closure()
```

By passing in an argument to `exe()` function, it creates its own local scope with the value `i` that it received from outside. By doing so, `setTimeout` has a `closure scope` that it remembers the variable from its closest parent's scope.

Or we can also use an anonymous function:
```javascript
const closure = () => {
  for (var i = 0; i < 10; i++) {
    ((i) => {
      setTimeout(() => {
        console.log(i);
      }, i * 1000);
    })(i) // <- we need to pass in the i when executing the function
  }
};
```