# Working with Classes

## What is Object-oriented Programming(OOP)?
Object-oriented Programming is a programing paradigm based on the concept of "objects", which can contain data (**attributes or properties**) and code (**methods**).

## What is Classes?
Classes allow us to build objects in an easier way, or to build objects based on some blueprint. Objects in classes are *instances (based on classes)*

## Getting Started with Class

```javascript
class MyProduct {
    title = 'DEFAULT TITLE';
    description;
    price;
}

const myproduct = new MyProduct();
console.log(myproudct);
// { title: 'DEFAULT TITLE', description: undefined, price: undefined }
```
The convention of class is to uppercase the name you want to name your class.
Define properties and methods you want the class to have.
