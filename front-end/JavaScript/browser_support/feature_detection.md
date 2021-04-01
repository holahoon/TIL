# Feature Detection + Fallback Code & Using Polyfills & Transpilation

## Feature Detection + Fallback Code

Many different browsers support different features. Some features may not work in old browsers (typically I.E ugh). Ways we should handle such cases when work with features that are not fully supported in all browsers.

```html
(...)
  <body>
   <button>COPY</button>
   <p>Copy this paragraph!!</p>
  </body>
(...)
```

```javascript
const button = document.querySelector('button');
const textParagraph = document.querySelector('p');

button.addEventListener('click', () => {
  const text = textParagraph.textContent;

  if (navigator.clipboard) {
    navigator.clipboard
      .writeText(text)
      .then((res) => {
        console.log(res);
      })
      .catch((err) => {
        console.log(err);
      });
  } else {
    alert('Feature is not available in the current browser.');
  }
});
```

The above code copies the `p` text when `button` is clicked. The `navigator.clipboard` feature works perfectly in Chrome or Edge, but no in Firefox. When we click the button in Firefox, it console logs that the feature is undefined. This is not a good UX.
`if (navigator.clipboard)...` code block checks if such method is truthy and if so, runs the following code and if not, it alerts the user.
This is better than not working with any fallback code.

## Polyfills

It is a third part JavaScript code which adds an supported feature to the browser (for this script execution).

Well, an example in using polyfill is by going into [caniuse.com](caniuse.com). Let's try looking for a method `fetch`. Go into Resources tab and there's a polyfill link. Or we can search for such method's polyfill on Google.

## Transpilation

Transforms the modern code into an older code.
We mostly use third-party library like Babel.
Let's configure babel in our webpack.
[babel loader github](https://github.com/babel/babel-loader)

```bsh
npm install -D babel-loader @babel/core @babel/preset-env
```
(we don't need to install `webpack` if it's already installed).

As we read through the documentation (above), we need to add
```javascript
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: [
            ['@babel/preset-env', { targets: "defaults" }]
          ]
        }
      }
    }
  ]
}
```

in the `webpack.config.js` file (btw, should also add the code if we have `webpack.config.prod.js` file).

##### [ webpack.config.js ]
```javascript
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/app.js',
  output: {
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/',
  },
  devtool: 'cheap-module-eval-source-map',
  // devServer: {
  //   contentBase: './'
  // }
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [['@babel/preset-env', { targets: 'defaults' }]],
          },
        },
      },
    ],
  },
  plugins: [new CleanPlugin.CleanWebpackPlugin()],
};
```

Then in our `package.json` file,

##### [ package.json ]
```javascript
{
  "name": "javascript-complete-guide",
  "version": "1.0.0",
  "description": "JavaScript - The Complete Guide",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "build:dev": "webpack-dev-server",
    "build:prod": "webpack --config webpack.config.prod.js"
  },
  "author": "David",
  "license": "ISC",
  "browsersList": "> 2%", // <- browser market share > 2%
  "devDependencies": {
    "@babel/core": "^7.13.10",
    "@babel/preset-env": "^7.13.10",
    "babel-loader": "^8.2.2",
    "clean-webpack-plugin": "^3.0.0",
    "eslint": "^6.4.0",
    "webpack": "^4.40.2",
    "webpack-cli": "^3.3.9",
    "webpack-dev-server": "^3.8.1"
  }
}
```

`"browsersList": "> 2%"` - you want to output code that works in browsers that have market share of greater than 2%.

We can now run `npm run build` to build our app.

When we look at the built folder `assets/scripts/app.js`, we can see that it transpiled from `const` to `var` and `arrow function` to a normal `function`.

However, we have a problem that the `"promise"` in the `navigator.clipboard` is not transpiled which is not supported in old browsers.
In this case, we need to add polyfills.
We then look for promise polyfill and install it and blah blah blah...
Well, what if our project gets bigger and bigger? We can't really check for all the features if they are supported in older browsers.

What if babel automatically sees that we are using `promise` and automatically include polyfill for that? **It is capable of doing it!**
**[core-js](https://www.npmjs.com/package/core-js) / [github](https://github.com/zloirock/core-js) solves our issue!**

```bash
npm install --save core-js
```
> We don't install it as dev dependency btw.

Then in our `app.js` file, we can add `import 'core-js'`.

We have a problem. if we run `npm run build:dev` to run our development server, we can notice that the size of our `app.js` file is 2.4MB. It's HUGE!!
This is because we are importing everything that we don't need.

Instead, we can do
`import 'core-js/features/promise';`.
We can now notice that it's about half the size, 1.2MB.
We are importing only what we need which is good, but not ideal due to having to import manually.

We can configure our `webpack` to automatically import polyfill
##### [webpack.config.js]
```javascript
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/app.js',
  (...)
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              [
                '@babel/preset-env',
                {
                  targets: 'defaults',
                  useBuiltIns: 'usage',
                },
              ],
            ],
          },
        },
      },
    ],
  },
  plugins: [new CleanPlugin.CleanWebpackPlugin()],
};

};
```

By having `useBuiltIns: 'usage'`, we let babel to automatically import the needed polyfill.
We also need another package: [regenerator-runtime](https://www.npmjs.com/package/regenerator-runtime)

```bash
npm i regenerator-runtime
```

##### [webpack.config.js]
```javascript
(...)
{
    targets: 'defaults',
    useBuiltIns: 'usage',
    corejs: { version: 3 },
},
(...)
```

We specifically tell babel to use version 3 of `corejs` since there was a big change from version 2.