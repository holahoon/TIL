# Introduction to Testing

### Why test?
You can get an error if you break your code - can be integrated into build workflow. You can also save time by breaking up complex dependencies. Lastly, you can think about possible issues & bugs which can improve your code.

### Different kinds of Tests

- Unit Tests: Fully Isloated (e.g. testing on function)
- Integration Tests: With Dependencies (e.g. testing a function that calls a function)
- End-to-End(E2E) Tests: Full Flow (e.g. validating the DOM after a click)

### Kinds of Testing Setup

- Test Runner: exectue your tests, summarize results. (for unit + integration) - e.g. `Mocha`
- Assertion Library: Define testing logic, conditions (for unit + integration) - e.g. `Chai`
- Headless Browser: Simulates browser interaction (for e2e) - e.g. `Puppeteer`

## Basic Test with [Jest](https://jestjs.io/)

```bash
$ npm install --save-dev jest @types/jest
```
You should also install `@types/jest` for working on both JS and TS.

You first need to create a file to test your code. You typically name your testing file same as the file you want to test with `.spec.js` or `.test.js`. Jest will automatically detect your file with the name `.spec` or `.test`.

## Unit Tests

#### [ util.js ]
```javascript
exports.generateText = (name, age) => {
  // Returns output text
  return `${name} (${age} years old)`;
};
```

#### [ util.test.js ]
```javascript
const { generateText } = require("./util");

test("should output name and age", () => {
  const text = generateText("DK", 31);
  expect(text).toBe("DK (31 years old)");

  const text2 = generateText("David", 25);
  expect(text2).toBe("David (25 years old)");
});

test("should output data-less text", () => {
  const text = generateText();
  expect(text).toBe("undefined (undefined years old)");
});
```

`test()` takes two arguments - first a test that you want to output (be descriptive as it will tell you what you are testing for), and a callback function that you want to run your test.
We can then use the `expect()` method by passing in the text we want to test and chain it using `toBe()` to pass in what output we expect.

We then need to configure our `package.json` file

#### [ package.json ]
```javascript
{
  "name": "js-testing-introduction",
  "version": "1.0.0",
  "description": "An introduction to JS testing",
  "main": "app.js",
  "scripts": {
    "start": "webpack app.js --mode development --watch",
    "test": "jest --watchAll"
  },(...)
  "author": "DK",
  "license": "ISC",
  "devDependencies": {
    "@types/jest": "^26.0.22",
    "jest": "^26.6.3",
    "webpack": "^4.20.2",
    "webpack-cli": "^3.1.2"
  }
}
```

Change the `"test"` from its default value to `"jest"`. You will notice that it has `--watchAll`. You can just pass in `--watch` to run the test everything your file gets saved, but it seems like it throws an error just by passing in `--watch` - alternative recommended was `--watchAll`.

## Integration Tests

#### [ util.js ]
```javascript
exports.checkAndGenerate = (name, age) => {
  if (!validateInput(name, true, false) || !validateInput(age, false, true)) {
    return false;
  }
  return generateText(name, age);
};
```

#### [ util.test.js ]
```javascript
test("should generate a valid text output", () => {
  const text = checkAndGenerate("DK", 30);
  expect(text).toBe("DK (30 years old)");
});
```