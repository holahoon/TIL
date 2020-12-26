# Copying an object

Object and array are reference types. Therefore, when they are just "copied", the copied variable still points at the original variable.

#### mutation
```javascript
const person = {
    name: 'DK',
    age: 30
}

const person2 = person;
person2.sex = 'male';

console.log(person);
// {name: 'DK', age: 30, sex: 'male'}
```
As you can see above, by updating the copied variable, it still mutates the original variable `person` because `person2` points at the address the `person` is located.

#### non-mutation (clone)
```javascript
const person = {
    name: 'DK',
    age: 30
}

const copiedPerson = {...person};
delete copiedPerson.age; // deleted an age property in copiedPerson object

console.log(copiedPerson);
// {name: 'DK'}
console.log(person);
// {name: 'DK', age: 30}
```

When in need(most of the times) to copy an object, this method is recommended.

#### cloning an object and assign an updated value
```javascript
const person = {
    name: 'DK',
    age: 30,
}

const person3 = {...person, age: 51}
console.log(person3);
// {name: 'DK', age: 51}
console.log(person);
// {name: 'DK', age: 30}
```

> However, `[...]` spread operator does NOT deep clone reference types within object/array.