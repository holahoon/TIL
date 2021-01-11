# Class Private Fields, Properties and Methods

#### [reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)

> This is **relatively new** not really recommended to be used now, but maybe in the future.

### Public
- Accessible from outside of the class/object
- The "things" you work with in your other code
- Ex). `product.buy();`

### Private
- Accessible ONLY from inside of the class/object
- The "things" you work with in your class only (to split & re-use code)
- Ex). hard coded(fallback) values, re-used class-specific logic

## Alternative
Since this `#`(private) is not fully supported, you might find many scripts using **"psued-private"** properties.
```javascript
class User {
    constructor() {
        this._role = 'admine';
    }
}

const product = {
    _internalId: 'abcd123'
}
```
It's a quite common convention to prefix private properties with underscore(`_`) to signal that they should not be accessed from outside the class/object.
Note, that it's just a **convention**, it does **NOT technically prevent** access.