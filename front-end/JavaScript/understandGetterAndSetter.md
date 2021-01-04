# Understanding Getters and Setters

## A quick example
아주 조금 배워봤다. 예제를 보자.

```javascript
const inputTitle = document.getElementById("title").value; // input value

const newMovie = {
    info: {
        // setter
        set title(val) {
            if (val.trim() === '') {
                this._title = 'DEFAULT TITLE';
                return; // exit out
            }
            this._title = val;
        }
        // getter
        get title() {
            return this._title;
        }
    },
    getFormattedTitle() {
        return this.info.title.toUpperCase();
    }
}

newMovie.info.title = inputTitle; // setter runs
const getTitle = newMovie.info.title; // getter runs
```

`setter` sets a value on `_title` (underscore is just an indicator that it's an inner title reference(?)).
`getter` gets the value that lives on `_title`.
the above `val.trim() ...` validation just checks to see if the `val` is empty which will set the `_title` as `'DEFAULT TITLE'`;

> Keep in mind that you CANNOT use getter without setter.

## Continues from [Working with Classes](workingWithClasses.md)

```javascript
class ShoppingCart {
  items = [];

  set cartItems(value) {
    this.items = value;
    this.totalOutput.innerHTML = `<h2>Total: $${this.totalAmount.toFixed(2)}</h2>`;
  }

  get totalAmount() {
    const sum = this.items.reduce(
      (prevValue, currItem) => prevValue + currItem.price,
      0
    );
    return sum;
  }

  addProduct(product) {
    const updatedItems = [...this.items];
    updatedItems.push(product);
    this.cartItems = updatedItems; // cartItems( value <-- updatedItems )
  }

  render() {
    const cartEl = document.createElement("section");
    cartEl.innerHTML = `
      <h2>Total: $${0}</h2>
      <button>Order Now!</button> 
    `;
    cartEl.className = "cart";
    this.totalOutput = cartEl.querySelector("h2");

    return cartEl;
  }
}
```
The argument that `cartItems(value)` receives is `updatedItems` as noted above.