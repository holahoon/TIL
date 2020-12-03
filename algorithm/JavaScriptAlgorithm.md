# JavaScript Algorithm

## Array

#### 1.1 Removing duplicates of an array and returning an array of only unique elements

```javascript
const duplicatedArray = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];

const findUniqueElements = (array) => {
    const hashmap = {};
    const uniqueArray = [];

    // iterate through the passed in array
    for (let i = 0; i < array.length; i++){
        console.log(array[i]);
        if (!hashmap.hasOwnProperty(array[i])){
            console.log(hashmap[array[i]]);
            hashmap[array[i]] = 1;
            uniqueArray.push(array[i])
        }
    }
    console.log(hashmap);
    return uniqueArray;
}

const finalizedAnswer = findUniqueElements(duplicatedArray);
console.log(finalizedAnswer); // [1, 2, 3, 5, 9, 8]
```
1. `console.log(array[i])` obviously console logs 1, 2, 3, 5, 1, ...
The [hasOwnProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty) returns a boolean indicating whether the object has the specified property as its own property (as opposed to inheriting it).
2. Conditional statement where it checks if `!hashmap.hasOwnProperty(array[i])` is checking if hashmap object does NOT have the `array[i]` value. Therefore, the `console.log(hashmap[array[i]])` will print `undefined`, which it is evaluated as `false`. In short, it will print `undefined` if number doest not exist in the object and will print nothing if number exists in the object.
3. `hashmap[array[i]] = 1` assigns as key: value pair in the `hashmap` object. So, if you check the `console.log(hashmap);`, it will show as following
```javascript
[object Object] {
    1: 1,
    2: 1,
    3: 1,
    5: 1,
    8: 1,
    9: 1
}
```
4. Lastly, it pushes each(`array[i]`) number if it meets the condition.