# Closures

#### Every function in JavaScript is a closure

This is closely related to scopes.
Block scopes when working with variables created with `const` or `let`. This is basically when variable created within a function is not accessible outside of that function.

Each function has its own **lexical environment**.

```javascript
function createTaxCal(tax) {
  function calcTax(amount) {
    return amount * tax;
  }

  return calcTax;
}
```
The outter function has its own environment, and the inner function has its own as well. Also, the `amount` argument has access to the outer environment(`createTaxCal()`).

```javascript
const calculateVat = createTaxCal(0.19);
const calculateIncomeTax = createTaxCal(0.25);
```
When we call the outer function, it actually creates the inner function. Also, note that we pass in two different values to as `tax` argument, but it prints different results. This is because the `tax` argument doesn't work as if we are storing a global variable outside of the `createTaxCal()` lexical scope, but it rather belongs to that scope.

So, it's called closure, because every function closes over the surrounding environment. Which means, it registers the surrounding environment and the variables registered there and it memorizes the values of these variables.

