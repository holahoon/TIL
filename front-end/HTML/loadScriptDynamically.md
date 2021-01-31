# Load Scripts Dynamically

There are times when we need to load a `script` tag dynamically. Well, this is really powerfuly, but dangerous. When dynamically adding `script` tag in the code, make sure that you **DON'T** do it based on user entered content, or at least validate that content and sanitize it before you execute it. This will be covered deeper in the security module.

Let's take a look at how this can be done though.

```html
<button id='send-analytics-btn'></button>
```

```javascript
function startAnalytics(){
    const analyticsScript = document.createElement('script'); // script is also a tag
    analyticsScript.src = 'assets/scripts/analytics.js'; // make sure that this path is same as if you were to call this js file in HTML
    analyticsScript.defer = true; // only load after when HTML parsing has been done

    document.head.append(analyticsScript); // add it to the head tag
}

document.getElementById('send-analytics-btn').addEventListener('click', startAnalytics)
```

When clicking on the button, it will dynamically add the `analytics.js` file to the head and will run whatever is in that `analytics.js` file.