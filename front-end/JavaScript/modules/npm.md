# NPM (Node Package Manager)

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