# Execution Context

When JavaScript engine runs your code, it creates an execution context.

### Global Execution Context

The first execution context that gets created is called **`global execution context`**.
Even there is no code to be executed, JavaScript engine still creates it.
Inside of the global execution context, has two properties: `window`(if running in Node environment, it's just called `global`) which points to the `global object`  and `this` which points to the `window` object.

Each execution context is going to have two phases: the `creation` phase and then later on the `execution` phase.

#### example

```javascript
var name = 'DK'
var occupation = 'Web dev'

function getUser() {
    return {
        name,
        occupation
    }
}
```

##### creation phase
In the `global execution context`, it will intialize the phase as `creation phase`.
Now, it will create an `window` global object, `this` object(which points to the window), `name: undefined`, `occupation: undefined`, `getUser: fn()`.
JS engine will create a memory space for the variables declarations `name` and `occupation` and assign them a default value of `undefined` and also create a memory space for `getUser()` function and sits entirely in the memory(because it's a function declaration).

##### execution phease
Then, after the `creation` phase is over, it enters the next phase: `execution phase`.
Keep in mind that the `window` and `this` objects will always be created no matter what.
After JS engine runs the first line in the execution phase, it will assign the value `'DK'` into the variable `name` that's in the memory.
So during the exeuction phase and after JS engine runs through the code again, it will have its corresponding values like `name` having `'DK'`, `occupation` having `'Web dev'` and `getUser()` having well, its function.

So, if we have a code like
```javascript
console.log('name: ', name) // undefined
console.log('occupation: ', occupation) // undefined
console.log('getUser: ', getUser) // f getUser() {return {name, occupation}}

var name = 'DK'
var occupation = 'Web dev'

function getUser() {
    return {
        name,
        occupation
    }
}
```
`name`, `occupation` will have value of `undefined` since it was initialized in the `creation phase` and was assigned `undefined` as its values.
If we then run `console.log(name)` again, it will successfully log `'DK'`.
Same goes for the `getUser()` function, but JS engine creates a memory and puts this function entirely in the memory.

**This is the concept of `hoisiting`**.

We often get confused of what hoisiting is. Well, nothing is actually "hoisted" or moved around. It is actually a **process of assigning a variable declaration a default value of undefined during the creation phase**.
That's all it is.

#### Summary

In summary, `global execution context`
1. creates a global object(`window` if not on Node env)
2. creates an object called `this`(points to the global object)
3. sets up a memory space for variables and functions
4. assigns variable declarations a default value of `undefined` while placing any function declaration in memory.

And `hoisting` is just a process in global execution context creation phase.

### Function Execution Context

This context is created whenever a function is invoked.

The difference between the `global execution context` and `function execution context` is only one thing: while `GEC` creates a global object(`window`), it doesn't make sense for `FEC` to create `window`. `FEC` creates what's an `argument object`.
When you invoke a function, that function can receive argument(s) right?

```javascript
var name = 'DK'
var occupation = 'Web dev'

function getUser() {
    return {
        name,
        occupation
    }
}

getUser()
```

As we learned, JS engine runs the code and enters in `GEC` `creation phase`, a.k.a hoisting, then goes into `execution phase` which then starts assigning its corresponding values.

Then the JS engine steps into the line where it invokes the `getUser()` function, which then creates the `FEC`: `getUser execution context`.
Just as the `GEC`, it also first steps into `creation phase`, which creates `arguments: {length: 0}` (since there's no arguments), and `this` which also points to the `window`.
Also, `getUser()` function doesn't have any variables within, the JS engine doesn't need to create any memory space/hoist those variables.

After `getUser()` function gets invoked(ran), the `getUser execution context` is popped off from **`"execution stack"`**.

### Execution Stack

So, as we know that JavaScript is a single-threaded, it only handles one task at a time.
Whenever a function is invoked, a new `execution context` is created and added to the `execution stack`. Then whenever a function is finished running through both the creation phase and execution phase, it is popped off the execution stack.