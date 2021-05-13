# Ject Basic

**Still needs to be updated**

const fn = require('./fn');

/**
 * string / number 같은 기본 타입 값을 비교할 때 사용한다.
 * - toBe()
 */
// test('1 is 1', () => {
//   expect(1).toBe(1);
// });

// test('2 + 3 = 5', () => {
//   expect(fn.add(2, 3)).toBe(5);
// });

// test('3 + 3 != 4', () => {
//   expect(fn.add(3, 3)).not.toBe(4);
// });

/**
 * object 같은 타입의 값을 비교할 때 사용한다.
 * - toEqual()
 * 더 빡시게 값을 비교하기 위해서 사용된다.
 * - toStrictEqual()
 */

// test('expect to receive name and age as an object', () => {
//   expect(fn.makeUser('DK', 30)).toEqual({
//     name: 'DK',
//     age: 30,
//   });
// });

// test('test2: expect to receive name and age as an object', () => {
//   expect(fn.makeUser('DK', 30)).toStrictEqual({
//     name: 'DK',
//     age: 30,
//   });
// });

/**
 * Check if values are null, undefined or defined
 * - toBeNull()
 * - toBeUndefined()
 * - toBeDefined()
 */

// test('null is null', () => {
//   expect(null).toBeNull();
// });

// test('the value should be undefined', () => {
//   expect(undefined).toBeUndefined();
// });

/**
 * Check if values are boolean
 * - toBeTruthy()
 * - toBeFalsy()
 */

// test('0 should be falsy', () => {
//   expect(fn.add(1, -1)).toBeFalsy();
// });

// test('non-empty string should be truthy', () => {
//   expect(fn.add('hello', 'DK')).toBeTruthy();
// });

/**
 * Check for inequality
 * - toBeGreaterThan()
 * - toBeGreaterThanOrEqual()
 * - toBeLessThan()
 * - toBeLessThanOrEqual()
 */

// test('ID should be less than 10 characters long', () => {
//   const id = 'abcdef';
//   expect(id.length).toBeLessThan(10);
// });

// test('password should be greater than 6', () => {
//   const pw = '123456';
//   expect(pw.length).toBeGreaterThanOrEqual(6);
// });

/**
 * One key note: in JavaScript, when 0.1 + 0.2 !== 0.3
 * This is because JS doesn't know how to round up
 * - toBeCloseTo()
 */

// test('0.1 + 0.2 = 0.3', () => {
//   expect(fn.add(0.1, 0.2)).toBeCloseTo(0.3);
// });

/**
 * Working with strings - using regex
 * - toMatch()
 */

// test("Does 'a' exists in the Hello world?", () => {
//   expect('Hello world').toMatch(/h/i); // both upper/lowercase
// });

/**
 * Working with arrays
 */

// test('Is user Mike in the list?', () => {
//   const user = 'Mike';
//   const userList = ['Tom', 'Jerry', 'DK'];
//   expect(userList).toContain(user);
// });

/**
 * Working with errors (throw)
 * - toThrow(arg: optional)
 */

// test('Throwing an error', () => {
//   expect(() => fn.throwErr()).toThrow('xx');
// });

/**
 * Working with async fns - "Callback pattern"
 * pass in "done" callback function so that until "done" is called, jest doesn't end the test
 */

// test('get DK after 3 seconds', (done) => {
//   function cb(name) {
//     expect(name).toBe('DK');
//     done();
//   }
//   fn.getName(cb);
// });

// test('Test server', (done) => {
//   function testServer() {
//     try {
//       expect(name).toBe('DK');
//       done();
//     } catch (error) {
//       done();
//     }
//   }
//   fn.getAPI();
// });

/**
 * Working with async fns - "Promise"
 * must return the function when working with promises
 */

// test('should get an error after 3 seconds', () => {
//   return fn.getAge().then((age) => {
//     expect(age).toBe(31);
//   });
// or user matcher - resolves / rejects
//   return expect(fn.getAge()).resolves.toBe(31);
//   return expect(fn.getAge()).rejects.toMatch('error...');
// });

/**
 * Working with async fns - "Async/Await"
 */

// test('should get an age of 31 after 3 seconds', async () => {
//   const age = await fn.getAge();
//   expect(age).toBe(31);
// or
//   await expect(fn.getAge()).resolves.toBe(31);
// });

/**
 * Tasks before/after test
 * - beforeEach() - runs before each execution
 * - afterEach() - runs after each execution
 */

// let num = 0;

// beforeEach(() => {
//   num = 0;
// });

// test('0 + 1 is 1', () => {
//   num = fn.add(num, 1);
//   expect(num).toBe(1);
// });

// test('0 + 2 is 2', () => {
//   num = fn.add(num, 2);
//   expect(num).toBe(2);
// });

// test('0 + 3 is 3', () => {
//   num = fn.add(num, 3);
//   expect(num).toBe(3);
// });

/**
 * Let's take a look at a when connecting and disconnecting with a DB
 * We want to run all the tests in between before and after all cycle
 * - beforeAll()
 * - afterAll()
 */
let user;

// beforeEach(async () => {
//   user = await fn.connectUserDB();
// });

// afterEach(() => {
//   return fn.disconnectUserDB();
// });
beforeAll(async () => {
  user = await fn.connectUserDB();
});
afterAll(() => {
  return fn.disconnectUserDB();
});

test('name should be DK', () => {
  expect(user.name).toBe('DK');
});

test('age should be 31', () => {
  expect(user.age).toBe(31);
});

test('gender sould be male', () => {
  expect(user.gender).toBe('male');
});
