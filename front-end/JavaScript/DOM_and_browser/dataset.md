# dataset - access data-* attributes

[reference W3](https://www.w3schools.com/tags/att_data-.asp)

The `data-*` attribute is used to store custom data private to the page or application.
The `data-*` attribute gives us the ability to embed custom data attributes on all HTML elements.
The stored (custom) data can then be used in the page's JavaScript to create a more engaging user experience (without any Ajax calls or server-side database queries).

```html
<h2 data-what-ever="some value here" id="some-h2">Hello DK</h2>
```

The idea is attach some data to the DOM element and work with it.

### How to read from the `data-*`

There's a special property - `dataset`

```javascript
const myH2 = document.getElementById('some-h2');
console.log(myH2);
```

myH2 will console log what's called `DOMStringMap`. Also, notice how `whatEver` is converted to use camelCase.
```
{
whatEver: "some value here"
__proto__: DOMStringMap
}
```

### Set some data value to the `data`

```javascript
myH2.dataset.someData = 'Testing to see if it does work';
```

This will create a new dataset named `data-some-data` to the DOM and can console log to see someData: "Testing to see if it does work"

