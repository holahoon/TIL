# Regex (Regular Expression)

Regular Expression doesn't only exists in JavaScript, but it exists in most of the programming languages. They help you search for patterns in strings.

We can create a regex in two different ways.

### Example

This is for testing email if it includes @ and .
```javascript
// method 1
const regex1 = new RegExp('')
// method 2
const regex2 = /^\S+@\S+\.\S+$/
```

`^` - we start from the left (beginning)
`S+` - we accept any string (word)
`\` - then
`$` - we end with
and so on...

```javascript
regex2.test('haha'); // false
regex2.test('test123@email.com'); // true
```

### Another example
```javascript
const regex = /hello/;

regex.test('hello'); // true
regex.test('hi'); // false
regex.test('well, hello~~ there good looking young man'); // true
regex.test('Hello'); // false
```
It would return true for any string that contains `hello`. Also, it's case sensitive.

- For cases when you want to allow capital `H`,
```javascript
const regex2 = /(h|H)ello/;
```

- For cases when you want to check for `ello`, but don't really care about the starting character,
```javascript
const regex2 = /.ello/;

console.log(regex2.test("HHHHHHHello")); // true
console.log(regex2.test("....ellow")); // true
console.log(regex2.test("ello")); // false
```

### Bonus,
You can also use `.match()` or `.exec()` methods to test the regex
```javascript
'hi jello'.match(regex2);
```
This would return an array with some properties
