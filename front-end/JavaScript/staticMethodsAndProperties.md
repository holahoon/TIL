# Static Properties, Fields & Methods

This note continues using [Working with Classes](./workingWithClasses.md)

#### Static Field / Property / Method
  - Defined with `static` keyword
  - Only accessible on class itself, without instantiation (i.e. not on instance)
  - Typically used in helper classes, global configuration, etc

#### Instance Field / Property / Method
- Defined **without** static keyword
- Only accessible on instances(= objects) based on class
- Used for core, re-usable logic

```javascript
(...)
class Shop {
  render() {
    const renderHook = document.getElementById("app");

    this.cart = new ShoppingCart(); // this has been changed
    const cartEl = this.cart.render(); // this has been changed
    const productList = new ProductList();
    const productListEl = productList.render();

    renderHook.append(cartEl);
    renderHook.append(productListEl);
  }
}

class App {
  static cart; // it's a good practice to declare the field for better readability

  static init() {
    const shop = new Shop();
    shop.render();
    this.cart = shop.cart;
  }
}

App.init();
// const shop = new Shop();
// shop.render();
```

This `static` keyword allows to directly use the class without instantiating.
Neither static methods nor static properties can be called on instances of the class. Instead, they're called on the class itself. 