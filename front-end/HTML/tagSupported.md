# Check if a tag is supported in a browser

When we work with `template` tag, it's not supported in Internet Explorer XD. Well, we simply need to check it such tag "exists".
This can be done using JavaScript.

```javascript
if ('content' in document.createElement('template')) {
    // if it exists,
} else {
    // fallback code
}
```

The `document.createElement('template')` creates a new DOM element which in this case is `template` element. This will be `undefined` in IE.
The `'content' in` will not exist as a property if the right is is `undefined`.