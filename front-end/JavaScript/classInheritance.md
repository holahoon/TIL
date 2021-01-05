# Class Inheritance

Using `extend` keyword in *class declaration* or *class expressions* is to create a class as a child of another class.
#### [reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes#Sub_classing_with_extends)
You have access to not only your own class, but the parent class as well.

> You can only inherit from one class

#### This tutorial continues using [Working with Classes](workingWithClasses.md)

```javascript
class Component {
  constructor(renderHookId) {
    this.hookId = renderHookId;
  }

  createRootElement(tag, cssClasses, attributes) {
    const rootElement = document.createElement(tag);
    if (cssClasses) rootElement.className = cssClasses;
    if (attributes && attributes.length > 0) {
      for (const attr of attributes) {
        rootElement.setAttribute(attr.name, attr.value);
      }
    }
    document.getElementById(this.hookId).append(rootElement);
    return rootElement;
  }
}

class ShoppingCart extends Component {
  items = [];

  set cartItems(value) {
    this.items = value;
    this.totalOutput.innerHTML = `<h2>Total: $${this.totalAmount.toFixed(
      2
    )}</h2>`;
  }

  get totalAmount() {
    const sum = this.items.reduce(
      (prevValue, currItem) => prevValue + currItem.price,
      0
    );
    return sum;
  }

  constructor(renderHookId) {
    super(renderHookId); // *1
  }

  addProduct(product) {
    const updatedItems = [...this.items];
    updatedItems.push(product);
    this.cartItems = updatedItems; // cartItems( value <-- updatedItems )
  }

  render() {
    // const cartEl = document.createElement("section");
    const cartEl = this.createRootElement("section", "cart"); // Inherited from Component class
    cartEl.innerHTML = `
      <h2>Total: $${0}</h2>
      <button>Order Now!</button> 
    `;
    cartEl.className = "cart";
    this.totalOutput = cartEl.querySelector("h2");

    return cartEl;
  }
}

class Shop {
  render() {
    const renderHook = document.getElementById("app");

    this.cart = new ShoppingCart("app"); // app is an (root)id of an element to be used to append child elements
    // const cartEl = this.cart.render();
    this.cart.render();
    const productList = new ProductList();
    const productListEl = productList.render();

    // renderHook.append(cartEl);
    renderHook.append(productListEl);
  }
}
```

*1. When in need to calling the parent classes' constructor from the child's class constructor, use `super()` keyword. Also, always call `super()` first before assigning values like `this.something = something` within the constructor.