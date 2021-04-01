# ESLint 


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