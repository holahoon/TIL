# Tools - Linting & Webpack & NPM

### Categories
[Setting up npm](#setting-up-a-npm-project)
[EsLint](#eslint)
[Webpack](#webpack)

## Things to know

### Helpful tools when compiling your code

| **Tool Purpose** | **Tool Name** | **What it does & Why** | 
| ------------ |---------- | ------------------ |
| A Development | **webpack-dev-server** or **serve** | Serve under (more) realistic circumstances, auto-reload |
| Building Tool | **webpack** | Combine multiple files into bundled code (less files) |
| Code Optimization Tool | **webpack optimizer** | Optimize code(shorten funciton names, remove white spaces, ...) |
| Code Compilation Tool | **Babel** | Write modern code, get "older" browser support as an ouput |
| Code Quality Checker | **ESLint** | Check code quality, check for conventions & patterns |

### Workflow

We need NodeJS & npm installed globally to utilize in order to work with the above tools.
The desired workflow is the following,

#### During Development (upon "Save")
Linting (EsLint) -> Bundling (Webpack) -> Reload Dev Server

#### For Production (upon Command)
Linting (ESLint) -> Bundling (Webpack) -> Compilation (Babel) -> Optimization -> Production-ready Code

## Setting up a npm project

Within the desired project folder, run the following command,
```bash
$ npm init
```
Then, go through the questions it asks by either entering your desired answers or just skip each line by pressing enter key.
Once finished, you will have a `package.json` file created within your folder which contains your answers if entered.

With this `package.json` file, the project can be managed with `npm`. This means that we can now install project specific packages using `npm`.

### Installing packages

By running the following command,
```bash
$ npm install --save-dev [package name]
```
This basically signals to npm "we want to install the following package as a development dependency of this project". It's a package that I just want to use during the development process, not something I need to use in code to ship the product.

## ESLint 
```bash
$ npm install --save-dev eslint
```

After installing ESLint successfully, we should have the eslint package in our `package.json` file.
```json
{
  "name": "javascript-modular",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "David Kim",
  "license": "ISC",
  "devDependencies": {
    "eslint": "^7.21.0"
  }
}
```

The `^` infront of the version number of the package name means that we are not necessarily focusing only on the specific version, but we are fine with this version or higer.

We can also know that it installed `node_modules` folder which has the dependencies of the dependency we installed. However, never change the codes inside the `node_modules` folder. Also, if we delete the `node_modules` folder, we can run `npm install` which installs the `node_modules` folder again that contains the necessary dependencies of the dependency we installed.

#### Linting with ESLint

When going into your VSCode, hit `command` + `shift` + `p` and type in `eslint`. This will show you list of linting options. Select `Create ESLint Configuration` and go through the quetions it asks which will create a file called `.eslintrc.json`. Within this file, you can configure your desired settings to how you want to lint your JavaSCript code. 

Example, (I deleted all the other rules to simplify)
```json
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "rules": {
        "quotes": [
            "error",
            "single"
        ]
    }
}
```

This rule will throw errors when there are cases of using `double quotes`.

Here are some useful links to look at when needed,
[ESLint Official Doc - Config Options](https://eslint.org/docs/user-guide/configuring/)
[ESLint Official Doc - Rules](https://eslint.org/docs/rules/)
[Using Preset](https://www.npmjs.com/search?q=eslint-config)


## Webpack

### Install Webpack
```bash
$ npm install --save-dev webpack webpack-cli
```

### How to use
We first need to create a webpack file (in the same directory as where the `package.json` lives) - `webpack.config.js`.
This is the file where the webpack will do its job. This file under the hood is executed by nodeJS.
In nodeJS world, we export not by simply using `export`, but we should 
```javascript
module.exports = {...}
```
This simply is a syntax nodeJS uses for exposing that object outside of this file. Then, the webpack tool will go ahead and import that object. This is pretty much a configuration object.

Create a folder called `src` (which is a conventional name). This is where all the input files will live (JS files).
Say we have multiple nested JS files within the `src` folder and assume that we have an `app.js` file which is the main file. We want to specify the `app.js` file as the *entry point*, and need to also create another folder (say we name it as `scripts`) and specify that as the *output point*

#### Example

Say we have an `app.js` file that lives in `src` folder.

##### [ webpack.config.js ]
```javascript
const path = require('path');

module.exports = {
    entry: './src/app.js',
    output: {
        filename: 'app.js',
        path: path.resolve(__dirname, 'assets', 'scripts')
    }
}
```

`const path = require('path');` works same as if we were importing a package using `import` keyword, but in webpack.config.js, we import a packaged using `require`.
`'./src/app.js'` is our entry point where we want to use the file.
`output` is the thing that we want to output the configuration to. `filename` is what we want to name the file as. `path` is what path we want to create to output the created file. For the case above, it constructs a new absolute path and output starting at the current file(`__dirname`) adding `/assets/scripts` to the path.
** Make sure to delete all the file extensions when importing within JS file. i.e. `import SomeFile from '../someFile.js';` to `import SomeFile from '../someFile';` 

Now, we just need to configure the `package.json` file.
##### [ package.json ]
```javascript
{
  "name": "javascript-modular",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "David Kim",
  "license": "ISC",
  "devDependencies": {
    "eslint": "^7.21.0",
    "webpack": "^5.24.2",
    "webpack-cli": "^4.5.0"
  }
}
```

`"build"` can be named however you want, but using `build` keyword is ideal. We give what we want to run and for this case, `"webpack"`. This will use the `webpack-cli` tool and `webpack-cli` will use the `webpack` tool to do its thing. Eventually, it searches for `webpack.config.js` file. Now let's run `run npm build`.
It now generates a new folder called `assets` -> `scripts` -> `app.js` (keep in mind that it could generate some other js file). 

Also, we could have multiple scripts for different HTML pages. Hence, you might need more than one entry point.

##### [ webpack.config.js ]
```javascript
module.exports = {
    entry: {
        welcome: './welcome-page/welcome.js',
        about: './about-page/about.js',
        etc...
    }
}
```
Webpack will look up all these entry points and create one bundle per entry point.

At this point, since we have not specified the "mode option", webpack will automatically set it as production mode which the file will just be one long single lined code.

### Set mode option

##### [ webpack.config.js ]
```javascript
module.exports = {
    mode: 'development',
    entry: ...
    output: {
        ...
    }
}
```

### Set base path

Let's go ahead and set up a base path to specify where the files will live after building.
```javascript
module.exports = {
    mode: 'development',
    entry: ...
    output: {
        ...
    },
    publicPath: 'assets/scripts/'
}
```
**[reference](https://webpack.js.org/guides/public-path/)**

### Using webpack-dev-server
Let's go ahead and fire up a dev server.
webpack-dev-server so we can see our result in run-time.

**We are currently using webpack v5 and webpack-cli v4. This has brought quite a lot of changes. Let's downgrade the version to the following versions**
```bash
npm install --save-dev webpack@4 webpack-cli@3
```
This will install webpack v4(latest) and webpack-cli v3(latest) so we can use `web-dev-server` much more easy.

Let's install
```bash
npm install --save-dev webpack-dev-server
```

Let's go ahead and add `"build:dev": "webpack-dev-server"` command to fire up the server.
##### [ package.json ]
```javascript
{
{
  ...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "build:dev": "webpack-dev-server"
  },
  "author": "David Kim",
  "license": "ISC",
  "devDependencies": {
    "eslint": "^7.21.0",
    "webpack": "^4.46.0",
    "webpack-cli": "^3.3.12",
    "webpack-dev-server": "^3.11.2"
  }
}
```
> If using webpack version higher than 5, use `"webpack serve"` instead of `"webpack-dev-server"`. But for our case, we can just use webpack-dev-server

If we run `npm run build:dev`, it will allow you to run your project at http://localhost:8080/.
`webpack-dev-server` watches for changes in our JS files and auto-reloads the browser.

### Generate sourcemaps to debug

Let's generate sourcemaps to be able to debug our files.
[reference](https://webpack.js.org/configuration/devtool/)

##### [ webpack.config.js ]
```javascript
module.exports = {
  mode: 'development',
  entry: './src/app.js',
  output: {
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/',
  },
  devtool: 'eval-cheap-module-source-map',
};
```

By adding `devtool: 'eval-cheap-module-source-map'`, we can now go back to the browser and go to `Sources`, visit `webpack://` and there, we can find all of the original files to be able to add break points, etc to debug.

### Build for production

This means that we want to generate optimized codes that are as little as possible.
Now, we need to create a new webpack file
let's name it as `webpack.config.prod.js`.

##### [ webpack.config.prod.js ]
```javascript
const path = require('path'); // import 'path' package from node_modules

module.exports = {
  mode: 'production',
  entry: './src/app.js',
  output: {
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/',
  },
  devtool: 'eval-source-map',
};
```

##### [ package.json ]
```javascript
{
  ...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "build:dev": "webpack-dev-server",
    "build:prod": "webpack --config webpack.config.prod.js"
  },
  ...
}
```
We need to specify what file to use when running such command like above.
If we run `npm run build:prod`, it will build its appropriate folder for production.

So far, we have 2 work flows now,
`build:dev` - spins up the development server to use in development
`build:prod` - we use right before we are ready to deploy the project.

### Optimize to clean up the old-generated JS files

Whenever we build something new and files are name slightly differently, old files are kept around and new files are created. We need to clear all the files in the folder with every build, and then just add newly generated files there to get rid of old files that are not needed.
We need to install a package,
```bash
npm install --save-dev clean-webpack-plugin
```
This cleans up the webpack output folder.

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
  devtool: 'eval-cheap-module-source-map',
  plugins: [new CleanPlugin.CleanWebpackPlugin()],
};
```

##### [ webpack.config.prod.js ]
```javascript
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  mode: 'production',
  entry: './src/app.js',
  output: {
    filename: 'app.js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/',
  },
  devtool: 'eval-source-map',
  plugins: [new CleanPlugin.CleanWebpackPlugin()],
};
```

Within `plugins: []` is where we can use multiple plugins and for this case, the `CleanPlugin`.


#### Let's generate new named JS files upon prod build!
The browser caches the JS file if the name hasn't changed after the user visits the page. If we want the browser to always fetch the JS file from the server every time the user visits the page, we need to rename the built files on `npm run build:(prod)` (it really only matters in production though).

##### [ webpack.config.prod.js ]
```javascript
const path = require('path');
const CleanPlugin = require('clean-webpack-plugin');

module.exports = {
  ...
  output: {
    filename: '[contenthash].js',
    path: path.resolve(__dirname, 'assets', 'scripts'),
    publicPath: 'assets/scripts/',
  ...
};

```

`contenthash` is a keyword to webpack which tells webpack that a hash should be generated. A hash that is different whenever a file changed. If the file didn't change, webpack will keep the existing hash.
By running `npm run build:prod`, it will not create `app.js` in the generated folder, but rather it will generate some random JS file name.
Now the browser will always download new JS files whenever JS file changes.
Keep in mind that right now, our HTML file imports `app.js` file and we need to change this (only in production).
