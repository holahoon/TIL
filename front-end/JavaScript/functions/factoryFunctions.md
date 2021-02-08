# Factory Functions

## What is Factory Function?

Is a function that returns a new object. When it's not a constructor function or class, it's called a factory function. In case if you are wondering what a constructor is, it forces callers to use the `new` keyword whereas the factories don't.
[reference](https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e)

#### Example

```javascript
function createTaxCal(tax) {
  function calcTax(amount) {
    return amount * tax;
  }

  return calcTax;
}

const calculateVat = createTaxCal(0.19);
const calculateIncomeTax = createTaxCal(0.25);

console.log(calculateVat(100)); // 19
console.log(calculateVat(200)); // 38
```
It basically creates a variable that holds the `createTaxCal` function by passing in some arguments as `tax`. Then, when we call the `calculateVat` function by passing in a value which goes into the `calcTax` function, it returns teh `calcTax` function.