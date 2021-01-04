# Working with Classes

## What is Object-Oriented Programming(OOP)?
Object-oriented Programming is a programing paradigm based on the concept of "objects", which can contain data (**attributes or properties**) and code (**methods**).

## What is Classes?
Classes allow us to build objects in an easier way, or to build objects based on some blueprint. Objects in classes are *instances (based on classes)*

## Basic object structure
```javascript
productsList = {
  products: [
    {
      title: "NSX",
      imageUrl:
        "https://i.pinimg.com/originals/93/96/97/939697706133b449de845bdce4498620.jpg",
      price: 70000.0,
      description: "Honda NSX 1st generation",
    },
    {
      title: "S2000",
      imageUrl:
        "https://i.pinimg.com/originals/a0/e9/00/a0e90061b28071466bd43dc48a8f9028.jpg",
      price: 30000.0,
      description: "Honda S2000, the lengendary",
    },
  ],
  render() {
    const renderHook = document.getElementById("app");
    const prodList = document.createElement("ul");
    prodList.className = "product-list";

    for (const prod of this.products) {
      const prodEl = document.createElement("li");
      prodEl.className = "product-item";
      prodEl.innerHTML = `
            <div>
                <img src="${prod.imageUrl}" alt="${prod.description}" />
                <div class="product-item__content">
                    <h2>${prod.title}</h2>
                    <h3>$${prod.price}</h3>
                    <p>${prod.description}</p>
                    <button>Add to Cart</button>
                </div>
            </div>
        `;

      prodList.append(prodEl);
    }
    renderHook.append(prodList);
  },
};

productsList.render();
```

The code above renders two `li` with the title, description, price and an image.

## Getting Started with Class

```javascript
class MyProduct {
    title = 'DEFAULT TITLE';
    description;
    price;
}

const myproduct = new MyProduct(); // create new instance of the class
console.log(myproudct);
// { title: 'DEFAULT TITLE', description: undefined, price: undefined }
```
The convention of class is to uppercase the name you want to name your class.
Define properties and methods you want the class to have.

## Implementing Class

```javascript
class Product {
  // - class field -
  title = "DEFAULT VALUE";
  imageUrl;
  description;
  price;

  constructor(title, image, description, price) {
    // - class property -
    this.title = title;
    this.imageUrl = image;
    this.description = description;
    this.price = price;
  }
}

productsList = {
  products: [
    new Product(
      "NSX",
      "https://i.pinimg.com/originals/93/96/97/939697706133b449de845bdce4498620.jpg",
      "Honda NSX 1st generation",
      70000.0
    ),
    new Product(
      "S2000",
      "https://i.pinimg.com/originals/a0/e9/00/a0e90061b28071466bd43dc48a8f9028.jpg",
      "Honda S2000, the lengendary",
      30000.0
    ),
  ],
  render() {
      ...
  },
};

productsList.render();
```

`constructor` allows the class to accept arguments as parameter.
One tip to know is that the above class field is not required since the constructor has properties that gets passed in. The class property basically overrides the class fields in the case above. So the above code can be rewritten as;

```javascript
class Product {
  constructor(title, image, description, price) {
    // - class property -
    this.title = title;
    this.imageUrl = image;
    this.description = description;
    this.price = price;
  }
}
``` 

`this.title = title` adds a new "title" property to the eventually created objects.

### Refactor the code even more

```javascript
class Product {
  title = "DEFAULT VALUE";
  imageUrl;
  description;
  price;

  constructor(title, image, description, price) {
    this.title = title;
    this.imageUrl = image;
    this.description = description;
    this.price = price;
  }
}

class ProductItem {
  constructor(product) {
    this.product = product;
  }

  render() {
    const prodEl = document.createElement("li");
    prodEl.className = "product-item";
    prodEl.innerHTML = `
        <div>
            <img src="${this.product.imageUrl}" alt="${this.product.description}" />
            <div class="product-item__content">
            <h2>${this.product.title}</h2>
            <h3>$${this.product.price}</h3>
            <p>${this.product.description}</p>
            <button>Add to Cart</button>
            </div>
        </div>
      `;
    return prodEl;
  }
}

class ProductList {
  products = [
    new Product(
      "NSX",
      "https://i.pinimg.com/originals/93/96/97/939697706133b449de845bdce4498620.jpg",
      "Honda NSX 1st generation",
      70000.0
    ),
    new Product(
      "S2000",
      "https://i.pinimg.com/originals/a0/e9/00/a0e90061b28071466bd43dc48a8f9028.jpg",
      "Honda S2000, the lengendary",
      30000.0
    ),
  ];

  constructor() {}

  render() {
    const renderHook = document.getElementById("app");
    const prodList = document.createElement("ul");
    prodList.className = "product-list";

    for (const prod of this.products) {
      const productItem = new ProductItem(prod);
      const prodEl = productItem.render(); // returns a created div element

      prodList.append(prodEl);
    }
    renderHook.append(prodList);
  }
}

const productList = new ProductList();
productList.render();
```

## Using `bind()` method

In times when we need to add an event listener to an element and need to pass in a callback function,

```javascript
class ProductItem {
  constructor(product) {
    this.product = product;
  }

  addToCart() {
    console.log("adding product to cart...");
    console.log(this.product);
  }

  render() {
    const prodEl = document.createElement("li");
    ...

    const addCartButton = prodEl.querySelector("button");
    addCartButton.addEventListener("click", this.addToCart.bind(this));
    return prodEl;
  }
}
```