# Using hashmap

### 1.1 Removing duplicates of an array and returning an array of only unique elements

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

---

### 1.2 Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

**example**
> Input: nums = [2,7,11,15], target = 9
>Output: [0,1]
> Output: Because nums[0] + nums[1] == 9, we return [0, 1].

**constraints**
> 2 <= nums.length <= 103
>-109 <= nums[i] <= 109
>-109 <= target <= 109

**test case**
`nums`: `[11,15, 4, 7, 5]`
`target`: `9`

**solution**
```javascript
function twoSum(nums: number[], target: number): number[] {
    const numsMap: {[key: string]: number} = {}
    
    for (let i = 0; i < nums.length; i++){
        const foundIdx: number = numsMap[nums[i]]
        
        if (foundIdx >= 0){
            return [foundIdx, i]
        } else {
            const numToFind: number = target - nums[i]
            numsMap[numToFind] = i
        }
    }
    return null
}
```

`numsMap` has `key: value` pairs of key being the number to find after subtracting the `nums[i]` from the target number and the value being each index. It stores the number that needs to be found in the `numsMap`.

In the first conditional where it checks if `foundIdx >= 0`, it will just pass this conditional since our initial `foundIdx` has a value of `undefined` since it stores the value of the key that matches the `nums[i]` value. This is because we pretty much just skip the first index and move on to the next index. Also, it will keep having `undefined` due to `numsMap` not having the key that matches `nums[i]` up to the point where it does match.

The `numsMap` object so far has `{ '-2': 0, '-6': 1, '5': 2, '2': 3 }`. Once the it iterates the `nums` array and `numsMap` matches `nums[i]` which the code above does `numsMap[nums[i]]`, it won't spit out `undefined` but rather it spits out the value(which is the index that we stored) that it stores which in our case is 2. Since `foundIdx` will have a value that not `undefined`, it will pass the `foundIdx >= 0` conditional (0 is considered as a falsy value in JS). It finally returns the index that it has = `2` and the current `i` which is `4`.

As result, it will return `[2, 4]`.

It was pretty tough to get my stupid head around trying to understand the concept of hashmap, but I think I got a good hang of it.