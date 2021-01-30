# Template tag

The `template` tag doesn't render, but is part of the DOM. Basically, it doesn't get rendered by defaul in the beginning. This can be useful when in need to render some element by using JavaScript.

### How to use

Inside the `body` tag, 

```html
<template id='toolip'>
    <h2>More Information</h2>
    <p></p>
</template>
```

And then in JavaScript, we can access that `template`
```javascript
const tooltipTemplate = document.getElementById('tooltip');
const tooltipBody = document.importNode(tooltipTemplate.content, true);
```

This bascially gives us the content of the selected template tag which are the `h2` and `p` tag. And it creates a new node based on that. The `true` deep imports all the contents.

### Some extra

Here's a simple use case
```javascript
const tooltipElement = document.createElement('div'); // creates a div
tooltipElement.className = 'card'; // sets the tooltipElement with 'card' class

const tooltipTemplate = document.getElementById('tooltip');
const tooltipBody = document.importNode(tooltipTemplate.content, true);

tooltipBody.querySelector('p').textContent = 'This is a sample text';
tooltipTemplate.append(tooltipBody);
```