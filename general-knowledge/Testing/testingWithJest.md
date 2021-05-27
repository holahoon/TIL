# Testing with Jest

Set up `node package manager` and install `jest`. I installed `@types/jest` because it helps for auto completion.
```bash
$ npm init -y
$ npm i jest
$ npm i @types/jest
```

Create a test file `[filename].test.js`. In there, you can obviously start testing your JS code.
Run `npx jest --watchAll` to watch all of the changes that happen in your JS file and it runs all of the test codes and test on the save. Before any configuration, this is the only command that'll work.
If you just run `npx jest`, it only runs the test at that moment.
One tip to know when you have `console logs` in your code, you can run `npx jest --silent` so that it won't print out any logs.

An advantage of TDD by unit testing is that if we have a code that returns the length of the argument + woof!
```javascript
[woof.js]
function woof(str) {
  return str.length + ' woof!';
}

module.exports = woof;
```
and test this function

```javascript
const woof = require('./woof');

test('should return number of woofs', () => {
  const result = woof('woofs');
  console.log(result); // 5 woof!
});
```

This works perfectly fine.
However, what if we don't pass anything in?
`const result = woof();`. This will fail the test.
A lot of times, we make human errors. This is where unit testing comes in handy.
We first intentionally fail the test to see corner cases and strengthen our code.
For this case, we can add
`if (typeof str !== 'string') return;` in the `woof` function to return if the passed in argument is not a string type.

## Matchers

#### value equality check
use `toBe()` to check if the value is correct or wrong
```javascript
const woof = require('./woof');

test('should return number of woofs', () => {
  const result = woof('woofs');

  expect(result).toBe('5 woof!'); // to match
  expect(result).not.toBe('hehe'); // to not match
});
```

#### object/array equality check
use `toEqual()` to check if an object or array is correct or wrong
```javascript
const woof = require('./woof');

test('should return number of woofs', () => {
  const result = woof('woofs');

  expect({ a: 1, b: 2 }).toEqual({ a: 1, b: 2 }); // pass
  expect({ a: 1, b: 2 }).toEqual({ a: 1, b: 'haha' }); // fail
});
```

#### using regex
use `toMatch()` to see if a specific word matches the result
```javascript
expect(result).toMatch(/woof/i); // woof is included - pass
```

#### check if a property exists in an array
```javascript
expect(['a', 'b', 'c', 'd']).toContain('b'); // pass
```

#### Add a todo message
`.todo` will display a message on the terminal. It literally acts just as the term says "todo".
```javascript
test.todo('should not allow numbers to be passed');
```

#### skip test
`.skip` will skip the test and just run other tests.
```javascript
test.skip('...', () => {})
```

#### only test
`.only` will only test that code and skip the other tests. Just as the word describes.
```javascript
test.only('...', () => {})
```

