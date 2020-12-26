# Add, Modify & Delete properties in an object

Here's a simple object

```javascript
const persons = {
  name: "DK",
  age: 30,
  greet: function () {
    alert(`hey! ${this.name}`);
  },
};
```

#### Add a property
```javascript
persons.isAdmin = true;
console.log(persons.isAdmin); // true
```

#### Delete a property
```javascript
delete persons.age;
console.log(persons.age); // undefined
```

#### Dynamically add a property (dynamic property assignment)
having `[]` wrapped around the key in an object assigns the variable's value as key name. This is useful in cases when need to set the key as a user-defined property name.
```javascript
const somekey = "myCoolKey"; // should be declared before the object is created
const persons = {
  name: "DK",
  (...),
  [somekey]: "hehe",
};
console.log(persons.myCoolKey); // "hehe"
console.log(persons.["myCoolKey"]); // "hehe"
```