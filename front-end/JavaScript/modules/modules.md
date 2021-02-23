# JavaScript Modules.

There are times when we need to import bunch of JavaScript files into an HTML file like below.

```html
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Project Board</title>
    <link rel="stylesheet" href="assets/styles/app.css" />

    <script src="assets/scripts/Utility/DOMHelper.js" defer></script>
    <script src="assets/scripts/App/Component.js" defer></script>
    <script src="assets/scripts/App/ToolTip.js" defer'></script>
    <script src="assets/scripts/App/ProjectList.js" defer></script>
    <script src="assets/scripts/App/ProjectItem.js" defer></script>
    <script src="assets/scripts/app.js" defer></script>
</head>
```

Well, this is fine. But by doing such could be very cumbersome. An alternative and better way to do is to use `imports` and `export`. You are probably familiar with this concept when using ReactJS. This pretty much allows you to `export` your JS file so that it could be accessed in different JS files with the `import` keyword.
However in the HTML file, you would need to specify that you are using modules.

```html
<link rel="stylesheet" href="assets/styles/app.css" />
<!-- <script src="assets/scripts/Utility/DOMHelper.js" defer></script>
<script src="assets/scripts/App/Component.js" defer></script>
<script src="assets/scripts/App/ToolTip.js" defer type='module'></script>
<script src="assets/scripts/App/ProjectList.js" defer></script>
<script src="assets/scripts/App/ProjectItem.js" defer></script> -->
<script src="assets/scripts/app.js" defer type='module'></script>
```

All the other scripts are not needed, so I just commented them out and gave the `app.js` file a type of `module`.

Also, know that there's this special keyword called `globalThis` built in JavaScript. This is used when you have a variable in one JS file and need to access that variable from a different place.

```javascript
// [ app.js ]
globalThis.DEFAULT_VALUE = "DK!";

// [ ToolTip.js ] (example)
console.log(globalThis.DEFAULT_VALUE); // DK!
```

This is a replacement of using `window` object. If I just console log `globalThis`, it will console log the `window` object instead.