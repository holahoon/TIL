# Function Expression & Declaration

Functions are considered as **First Class Citizen in JavaScript** and it's really important to understand the concept of creating a function in JS.

Unlike other programming languages, we can create function in 3 distinct ways:
1. Function Declaration
2. Function Expression
3. Named Function Expression

## Function Declaration

```javascript
function someName(param1, param2, ...param3) {
    // function body & logic
}
```

> One major advantage in declaraing a function like above is that the complete function is **hoisted**. Because of that, we can execute the function before declaring it.

##### example
```javascript
var num1 = 10;
var num2 = 20;

var result = add(num1, num2); // => 30 [executes before declaring]

function add(param1, param2){
    return param1 + param2;
}
```

**However, it is best practice to declare the function frst and then execute it instead of relying on JavaScript hoisting!**

## Function Expression

In case of function expression, a function is created without any name and then assigned to a variable.

```javascript
var someName = function(param1, param2, ...param3){
    // function body & logic
}
```

> In this case, the function **cannot** be executed until defined. This means that function expression is **NOT hoisted**.

##### example
```javascript
var num1 = 10;
var num2 = 20;

var result = add(num1, num2);  

// Uncaught TypeError: add is not a function
var add = function(param1, param2) {
    return param1 + param2 ;
} 
```

## Named Function Expressions (combination of both approaches)

```javascript
var addValues = function addFunction(param1, param2){
    return param1 + param2;
}
```

Notice that the above function **NOT** anonymous function anymore and its got a name `addFunction`.