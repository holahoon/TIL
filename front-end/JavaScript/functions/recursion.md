# Recursion

The idea of recursion is that the function **calls itself**.

### Example

```javascript
function powerOf(x, n) {
  let result = 1;

  for (let i = 0; i < n; i++) {
    result *= x;
  }
  return result;
}
console.log(powerOf(2, 3)); // 2 * 2 * 2 -> 8
```

Say we have a function that receives two arguments and does the above.

By using recursion where the function calls itself,

```javascript
function powerOf(x, n) {
  if (n === 1) {
    return x;
  }
  return x * powerOf(x, n - 1);
}
```

It does the same thing as if we were using for loop, just that it calls itself until `x` is `1`(this is to avoid an infinite loop obviously). 
This can even be much shorter by using ternary expression.

```javascript
function powerOf(x, n) {  
  return n === 1 ? x : x * powerOf(x, n - 1);
}
```

### Advanced Example

Say we have an object like below
```javascript
const myself = {
  name: "DK",
  friends: [
    {
      name: "Jinho",
      friends: [
        {
          name: "Chris",
        },
      ],
    },
    {
      name: "Julia",
    },
  ],
};
```

This can easily be very tricky to loop through to just get the name of the friends
```javascript
function getFriendNames(person) {
  for (const friends of person.friends){
      for (const friendsFriends of friends.friends){
          for (what the fuck is this shit)
      }
  }
}
```
This is a completely shit code.

By using **recursion**, we can easily manage this.

```javascript
function getFriendNames(person) {
    const collectedNames = [];

    if (!person.friends) return []; // returns an empty array if there's not friends property

    for (const friend of person.friends){
        collectedNames.push(friend.name);
        collectedNames.push(...getFriendNames(friend));
    }

    return collectedNames;
}

console.log(getFriendNames(myself));
// [ "Jinho", "Chris", "Julia" ]
```