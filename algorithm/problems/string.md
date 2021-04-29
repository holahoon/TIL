# Algorithm examples

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

