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

#### Install Webpack
```bash
$ npm install --save-dev webpack webpack-cli
```

#### How to use
We first need to create a webpack file (in the same directory as where the `package.json` lives) - `webpack.config.js`.
This is the file where the webpack will do its job. This file under the hood is executed by nodeJS.
In nodeJS world, we export not by simply using `export`, but we should 
```javascript
module.exports = {...}
```
This simply is a syntax nodeJS uses for exposing that object outside of this file. Then, the webpack tool will go ahead and import that object. This is pretty much a configuration object.

Create a folder called `src` (which is a conventional name). This is where all the input files will live (JS files).
Say we have multiple nested JS files within the `src` folder and assume that we have an `app.js` file which is the main file. We want to specify the `app.js` file as the *entry point*, and need to also create another folder (say we name it as `scripts`) and specify that as the *output point*