# Tagged Template Literals

Using backticks, we can call/store a function.
An example can be found in [Styled-Component](https://styled-components.com/).

```javascript
function productDesc(strings, productName, productPrice) {
  console.log(strings);
  console.log(productName);
  console.log(productPrice);
  return "This is a product";
}

const prodName = "You don't know JS";
const prodPrice = 29.99;

const productOutput = productDesc`This product (${prodName}) is $${prodPrice}`;

console.log(productOutput);

```

We would get

```html
["This product (", ") is $", "", raw: Array(3)]
    0: "This product ("
    1: ") is $"
    2: ""length: 3
You don't know JS
29.99
This is a product
```

JavaScript simply goes through the template literal and takes all non-dynamic parts and push them into the first argument which is an array, and then takes the dynamic parts and adds them in the right order as argument values to the function.

So, when do we use such thing?
This is useful when you have a scenario where you conveniently want to create some output. Could be a string or something, just based on some string input.

example:
```javascript
function productDesc(strings, productName, productPrice) {
  let priceCategory = "pretty cheap";
  if (productPrice > 20) {
    priceCategory = "fairly priced";
  }

  return `${strings[0]}${productName}${strings[1]}${priceCategory}`;
}

const prodName = "You don't know JS";
const prodPrice = 29.99;

const productOutput = productDesc`This product (${prodName}) is ${prodPrice}`;

console.log(productOutput);
// This product (You don't know JS) is fairly priced
```

By the way, it doesn't need to be a string we return, we could also return an object... for whatever reason.