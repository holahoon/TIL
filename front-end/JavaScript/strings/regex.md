# Regex (Regular Expression)

Regular Expression doesn't only exists in JavaScript, but it exists in most of the programming languages. They help you search for patterns in strings.

We can create a regex in two different ways.

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