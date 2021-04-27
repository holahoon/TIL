# Execution Context

[reference](https://medium.com/@happymishra66/execution-context-in-javascript-319dd72e8e2c)

#### Execution Context (EC)
is defined as the environment in which the JS is executed. By enviroment - meaning the value of `this`, *variables, objects,* and *functions* JavaScript code has access to at a particular time.

- **Global Execution Context (GEC)** is the default execution context in which JS code start its execution when the file first loads in the browser. All of the global code i.e. code which is not inside any function or object is executed inside global execution context. GEC cannot be more than one because only one global environment is possible for JS code execution as the JS engine is single thread.

- **Functional Execution Context (FEC)** is defined as the context created by the JS engine whenever it finds any funciton call. Each function has its own execution context. It can be more than one. Functional execution context has access to all the code of the global execution context through vice versa is not applicable. *While executing the global execution context code, if JS engine finds a function call, it creates a new functional execution context for that function.* In the browser context, if the code is executing in `strict` mode, the value of `this` is `undefined` else it is `window` object in the function execution context.

- **Eval** is an execution context inside `eval` function.

#### Execution Context Stack (ECS)
is a stack data structure, i.e. *last in first out* data structure, to store all the execution stacks created during the life cycle of the script. **Global execution context** is present by default in execution context stack. While executing the global execution context code, if JS engines find a function call, it creates a functional execution context for that function and pushes it on top of the execution context stack. Once all the code of the function is executed, JS engines pop out that function's execution context and starts executing the function which is below it.

Let's take a look at an example:

```javascript
var a = 10; // (1) Global Execution Context - gets pushed in ECS

function funcA(){
    console.log('Start funcA');

    function funcB(){
        console.log('In funcB');
    } // (4) pops out funcB() out of ECS

    funcB(); // (3) funcB Execution Context
}; // (5) pops out funcA() out of ECS

funcA(); // (2) funcA Execution Context

console.log('GlobalContext'); // (6) gets called - removes all of GEC
```

As soon as the code above loads into the browser, JS engine pushes the global execution context in the execution context stack. When `funcA` is called from global execution context, JS engine pushes `func` execution context in the execution context stack and starts executin `funcA`.

When `funcB` is called from `funcA` execution context, JS engine pushes `funcB` execution context in the execution context stack. Once all the code of `funcB` gets executed, JS engine pops out `funcB` execution tonext. After this, as `funcA` execution context is on top of the execution context stack, JS engine starts executing the remaining code of `funcA`.

Once all the code from `funcA` gets executed, JS engine pops out `funcA` execution context from execution context stack and starts executing remaining code from the global execution context.

When all the code is executed, JS engine pops out the global execution context and execution of JavaScript ends.

This is how JavaScript engine handles the execution context.

---

Now, let's see how it creates the execution context.

JavaScript engine creates the execution context in the following two stages:

1. **Creation phase**
2. **Execution phase**

#### Creation phase
is the phase in which the JS engine has called a function but its execution has not started. In the creation phase, JS engine is in the compilation phase and it just scans over the function code to compile the code, it doesn't execute any code.
In the creation phase, JS engine performs the following task:

1. **Creates the Activation object for the Variable object**: Activation object is a special object in JS which contain all the variables, function arguments and inner functions declaration information. As activation object is a special object, it does not have the `dunder proto (__proto__)` property.
2. **Creates the scope chain**: Once the activation object gets created, the JS engine initializes the scope chain which is a list of all the variables objects inside which the current function exists. This also includes the variable object of the global execution context. Scope chain also contains the current function variable object.
3. **Determines the value of this**: After the scope chain, the JS engine initializes the value of `this`.

Let's take a look at how JS engine creates the acivation object:

```javascript
function funcA(a, b){
    var c = 3;
    var d = 2;

    d = function(){
        return a - b;
    }
}

funcA(3, 2);
```

Just after `funcA` is called and before code execution of `funcA` starts, JS engine creates an `executionContextObj` for `funcA` which can be represented as shown below:

```javascript
executionContextObj = {
    variableObject: {}, // all the variable, arguments and inner function details of the funcA
    scopeChain: [], // list of all the scopes inside which the current function is
    this // value of this
}
```

Activation object or variable object contains the argument object which has details about the arguments of the function.

It will have a property name for each of the variables and function which are declared inside the current function. Activation object or the variable object in our case will be shown below:

```javascript
variableObject = {
    argumentObject: {
        0: a,
        1: b,
        length: 2
    },
    a: 3,
    b: 2,
    c: undefined,
    d: undefined // then pointer to the function definition of d
}
```

**ArgumentObject**: JS engines will create the argument object as shown in the above code. It will also have the `length` property indicating the total number of arguments in the function. It will just have the property name, not its value.
Now, for each variable in the function, JS engine will create a property on the *activation object* or *variable object* and will initialize it with `undefined`. As arguments are also variables inside the function, they are also present as a property of the argument object.
If the variable already exists as a property of the argument object JS engine will not do anything and will move to the next line.
When JS engine encounters a function definition inside the current function, it will create a new property by the name of the function. Function definitions in the creation phase are stored in heap memory, they are not stored in the execution context stack. Function name property points to its defintion in the heap memory.

Hence in our case, first, `d` will get the value of `undefined` as it is a variable but when JS engine encounters a function with the same name it overrides its value to point it to the definition of function `d` stored in the heap.
After this JS engines will create the scope chain and will determine the value of `this`.

#### Execution phase:

In the execution phase, JS engines will again scan through the function to update the variable object with the values of the variables and will execute the code.
After the execution stage, the variable object will look like this:

```javascript
variableObject = {
    argumentObject: {
        0: a,
        1: b,
        length: 2
    },
    a: 3,
    b: 2,
    c: 3,
    d: undefined // then pointer to the function definition of d
}
```

##### example:

```javascript
a = 1;

var b = 2;

cFunc = function(e){
    var c = 10;
    var d = 15;

    a = 3;

    function dFunc(){
        var f = 5;
    }

    dFunc();
}

cFunc(10);
```

When the above code loads in the browser, JS engine will enter the compilation phase to create the execution objects. In the compilation phase, Js engine will handle only the declarations, it won't be bother about the values. This is the **creation phase of the execution context**.

**Line 1:** When variable `a` is assigned a value of `1`, JS engine does not think of it as a variable declaration of function declaration and it moves to the next line. It doesn't do anything with this line in the compilation phase as it is not any declaration.

**Line 3:** As the code is in the global scope and it's a variable declaration, JS negines will create a property with the name of this variable in the global execution context object and will initialize it with an `undefined` value.

**Line 5:** JS engine finds a function declaration, os it will store the function definition in heap memory and creates a property which will point to the location where function definition is stored. JS engine doesn't know what is inside of `cFunc`. It just points to its location.

**Last line:** This code is not a declaration, so JS engine will not do anything.

#### Global Execution Context object after the creation phase stage:

```javascript
globalExecutionContextObj = {
    activationObj: {
        argumentObj: {
            length: 0
        },
        b: undefined,
        cFunc: pointer to the function definition
    },
    scopeChain: [global execution context variable object],
    this: value of this
}
```

As further there is not code, JS engine will not enter the **execution phase** and will scan the function again. Here, it will update the variable value and will execute the code.

**Line 1:** JS engines find that there is no property with the name `a` in the variable object, hence it adds this property in the global execution context and initializes its value to `1`.

**Line 3:** JS engines checks that there is a property with the name `b` in the variable object and hence update its value to `2`.

**Line 5:** As it is a function declaration, it doesn't do anything and moves to the last line.

#### Global Execution Context object after the execution phase:

```javascript
globalExecutionContextObj = {
    activationObj: {
        argumentObj: {
            length: 0
        },
        b: 2,
        cFunc: pointer to the function definition,
        a: 1
    },
    scopeChain: [global execution context variable object],
    this: value of this
}
```

**Last Line:** Here, `cFunc` is called, so JS engine again enters the compilation phase to create the execution context object of `cFunc` by scanning it.
As `cFunc` has `e` as an argument, JS engine will add `e` in the argument object of `cFunc` execution context object and create a property by the name of `e` and will initialize it to `2`.

**Line 6:** JS engine will check if `c` is a property in the activation object of `cFunc`. As there is no property by that name, it will add `c` as property and will initialize its value to `undefined`.

**Line 7:** Same as line 6

**Line 9:** As this line is not a declaration, JS engine will move to the next line

**Line 11:** JS engine finds a function declaration, so it will store the function definition in the heap memory and create a property `dFunc` which will point to the location where function definition is stored. JS engine doesn't know what is inside `dFunc`.

#### `cFunc` Execution Context object after the compilation phase:

```javascript
globalExecutionContextObj = {
    activationObj: {
        argumentObj: {
            0: e,
            length: 1
        },
        e: 10,
        c: undefined,
        d: undefined
        dFunc: pointer to the function definition
    },
    scopeChain: [global execution context variable object],
    this: value of this
}
```

**Line 15:** As this statement is not a declaration, JS engine will not do anything.

As furthere there are no lines in this function. JS engine will enter the execution phase and will execute `cFunc` by scanning it again.

**Line 6 and 7:** `c` and `d` gets the value of 10 and 15 respectively

**Line 9:** As `a` is not a property on `cFunc` execution context object and it's  not a declaration, JS engine will move to the global execution context with the help of scope chain and checks if a property with the name `a` exists in the global execution context object. If the property does not exist, it will create a new one and will initialize it. Here, as property with the name `a` already exists on the global execution context object, it will update its value to `3` from `1`. Js engine moves to global execution context in this case only i.e. when it finds a variable in the execution phease which is not a property on the current execution context object

**Line 11:** JS engines will create a `dFunc` property and will point to its heap location

#### Execution Context object of `cFunc` after the execution phase:

```javascript
globalExecutionContextObj = {
    activationObj: {
        argumentObj: {
            0: e,
            length: 1
        },
        e: 10,
        c: 10,
        d: 15
        dFunc: pointer to the function definition
    },
    scopeChain: [global execution context variable object],
    this: value of this
}
```

**Line 15:** As this is a function call, JS engines will again enter the compilation phase to create `dFunc` execution context object.
`dFunc` execution context object has access to all the variables and functions defined on `cFunc` and in the global scope using the scope chain.
similarly, `cFunc` has access to all the variables and objects in the global scope but it does not have any access to the `dFunc` variables and objects.
Global execution context does not have access to `cFunc` or `dFunc` variables or objects.

With the above concepts, it's easier to understand how **hoisting** works in JavaScript.

---

#### Scope Chain

The scope chain is a list of all the variable objects of functions inside which the current function exists. Scope chain also consists of the current funciton execution object.

