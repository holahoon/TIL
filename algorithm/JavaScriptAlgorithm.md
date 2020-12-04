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

## String

#### 1.1 Given a string, reverse EACH word in the sentence "Hey there! I'm David!" should be become ""yeH !ereht m'I !divaD""
```javascript
const sentence = "Hey there! I'm David!";

const reverseBySeparator = (sentence, separator) => {
    return sentence.split(separator).reverse().join(separator);
};

const reverseEachLetter =  reverseBySeparator(string, "");
const reverseEachWord = reverseBySeparator(reverseEachLetter, " ");
console.log(reverseEachLetter); // "!divaD m'I !ereht yeH"
console.log(reverseEachWord); // "yeH !ereht m'I !divaD"
```

#### 1.2 Given two strings, return true if they are anagrams of one another "Mary" is an anagram of "Army"
```javascript
const firstWord = "Mary";
const secondWrod = "Army";

const isAnagram = (word1, word2) => {
    let a = word1.toLowerCase(); // 1
    let b = word2.toLowercase(); // 1

    a = a.split("").sort().join(""); // 2 ~ 4
    b = b.splie("").sort().join(""); // 2 ~ 4

    return a === b;
}
```
1. Turn the string into lowercase
2. Split the string by no space which turns into an array(i.e. `["M", "a", "r", "y"]`)
3. Sort the array using [sort()](https://www.w3schools.com/jsref/jsref_sort.asp) method
4. Join the sorted array by no space

#### 1.3 Check if a given string is a palindrome `"racecar" is a palindrome. "race car" should also be considered a palindrome. Case sensitivity should be taken into account`
```javascript
const testWord = "Race car";

const isPalindrome = (word) => {
    const lettersOnly = word.toLowerCase().replace(/\s/g, ""); // 1
    return lettersOnly === lettersOnly.split("").reverse().join(""); // 2
}
```
1. Turn the word into lowercase and replace all the non-chars with "" (outcome for this example will be `"racecar`)
2. compare the lettersOnly result by reversing it

