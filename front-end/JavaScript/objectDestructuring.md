# Object Destructuring

```javascript
const person = {
    info: {
        name: 'DK',
        age: 30
    },
    occupation: 'web developer',
    isCool: 😎
}
```

#### Basic Destructuring

```javascript
const { info, ...otherProps } = person;

console.log(info);
// {name: 'DK', age: 30}

console.log(otherProps);
// {occupation: 'web developer', isCool: 😎}
```
`info` is pulled out of the `person` object.
Also, by using rest parameter(`...`), will collect all properties that wasn't pulled out by name and will give a new object will all the collected remaining properties.
`...otherProps` doesn't need to be named as otherProps btw.

#### Destructuring with a new name
```javascript
const { info: myInformation } = person;
```
Times when the key name pulled out of the object clashes with say another variable within the scope, you can assign a new name to the key name that's been destructured.