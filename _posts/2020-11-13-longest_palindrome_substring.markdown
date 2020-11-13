---
layout: post
title:      "Longest palindrome substring👾"
date:       2020-11-13 16:20:53 +0000
permalink:  longest_palindrome_substring
---


This might not be a very traditional explanation/approach to this algorithm, but maybe it will help seeing these problems from a perspective that could help clarify things for some! Even though it is an intricate topic!

I was asked this question on a technical interview and was surprised by how much learning could come out of a single question. The problem description itself can require some googling to figure out. But what this problem is asking for is to find if there are any substrings that if split in half they are proportional. For example:

*madam*
Or
*noon*

Are both palindromes and if the string was `'goodafternoonmadam'` the longest palindrome substring would be *madam*. 

## Approach

[Solution in Sandbox](https://codesandbox.io/s/longest-palindrome-challenge-4juzy?file=/src/index.js)

I used javascript to solve this algorithm, but to give an overview to the challenge we can start looking at the edge cases this problem considers from the start:

* The String is 1 or less elements long.
* The whole String is a palindrome.
* All Characters are the same.
* The substring is a palindrome starting between two Characters (noon).
* The substring is a palindrome starting from a Character (madam).

We check if the string is 1 or less elements:

``` javascript
    if(string.length <= 1){ 
        // exit if string in 1 or less elements
        return string[0]
    }
```

To iterate over a string and modify/analyze it in javascript we can convert it into an array as follows:

`let initialChecks = string.split('')`

Then to check if the whole string is a palindrome we reverse the `initialChecks` array with the string characters as elements and compare it to the initial string.

``` javascript
    if (string === initialChecks.reverse().join('')){
        return string
    }
```

Then use the .every method to compare the each character to the first character(`initialChecks[0]`), and if they are equal we return the original string as it would be already a palindrome from the start.


``` javascript
    if(initialChecks.every( (character) => character === initialChecks[0] )){ // exit if all charactes are equal
        return string
    }
```

## Checking for palindrome substrings

So the first thing we do to start looking for actual palindrome substrings, is to add an empty string/blank space between every character in our `initialChecks` array and define an array with spaces (`arrSp`). That way, we can check for palindromes that are proportional from the space between two characters like *noon* or from a character *madam*.

``` javascript
const arrSp = initialChecks.join(' ').split("")
```

Now we can iterate over this new array with blank spaces between each character of the string and get the main work that the problem asks for.

In summary, we use a nested loop to visit each element in our prepared array (`arrSp`) to be able to expand on each element (`center`) and check if the characters are the same on the left (`i-j`) and the right (`i+j`) of our `center`. 

We add the equivalent surrounding characters that are not spaces or empty strings into a `palindrome` array that will contain each substring, and as we find more palindromes, we push them into an array which we called `results` here. On this array containing all of the palindrome subtrings, we can check which one is the longest, and thus find the final answer.

``` javascript
for(let i = 0;  i < arrSp.length; i++){
 let palindrome = [];
 let center;
 for(let j = 1;  j < arrSp.length; j++){ // inner loop to expand from each center (space or letter)
  center = arrSp[i]
  if(arrSp[i-j] && arrSp[i+j] && (arrSp[i-j] === arrSp[i+j]) ){ // loop outwards on every center
  // and keep expanding if equivalent characters found 
  // but only push if elements are not falsy a.k.a. our empty strings we added earlier
  arrSp[i-j].trim() ? palindrome.unshift(arrSp[i-j]) : null
  arrSp[i+j].trim() ? palindrome.push(arrSp[i+j]) : null 
  }else{
   break;
  }                
 }       
 !!center.trim() ? palindrome.splice(palindrome.length / 2, 0, center) : null 
 // add center back into palindrome at the end of outside of loop
 // but only if the center is not a blank space
 // by inserting into half of length
  palindrome.length ? result.push(palindrome) : null
 // add palindrome to result which is the collection of all substring palindromes in the string       
}

```

## Breaking it down

Using an if statement, we can check each of the surrounding elements of each `center` to see if the surrounding elements are the same character. The centers are accessed by the top loop index `i` and we use the nested index `j` to expand to the left and the right of each center.

``` javascript
if(arrSp[i-j] && arrSp[i+j] && (arrSp[i-j] === arrSp[i+j]) ){ // loop outwards on every center
// and keep expanding if equivalent characters found 
// but only push if elements are not falsey a.k.a. our empty strings/blank spaces we added earlier
  arrSp[i-j].trim() ? palindrome.unshift(arrSp[i-j]) : null
  arrSp[i+j].trim() ? palindrome.push(arrSp[i+j]) : null }else{
 break;
}                
```
** This algorithm's nested loops make O(n^2) so it could be optimized

Since we added blank spaces we use the `.trim()` method  to make sure we only add actual characters to rebuild each palindrome we find. We add these equivalent characters to left of the center with `.unshift(arrSp[i-j])` and to the right of the center with `.push(arrSp[i+j])`. Then if we stop having a palindrome center we exit out of the loop and move on to the next center by triggering the `break`.

After we found all the proportional sides of the palindrome substring, we add the center back into the palindrome, but only if its a character and not a blank space.

``` javascript
!!center.trim() ? palindrome.splice(palindrome.length / 2, 0, center) : null 
 // add center back into palindrome at the end of outside of loop
 // but only if the center is not a blank space
 // by inserting into half of length
  palindrome.length ? result.push(palindrome.join('')) : null
 // add palindrome to result which is the collection of all substring palindromes in the string   
```

And then we can push the palindrome we just rebuilt into the `result` array where we are collecting all the palindrome substrings from the original string.

## How do we find the longest string in the `result` array?

We can just use a `.sort()` method as follows:

``` javascript
 return result.sort((a,b) => b.length - a.length)[0]
```

We sort the array by decreasing palindrome length and then return the first element of the sorted array. 

Feel free to check out the [code in the sandbox.](https://codesandbox.io/s/longest-palindrome-challenge-4juzy?file=/src/index.js)

Any comments/ideas are more than welcome!

Feel more than welcome to reach out! :)

[LinkedIn](https://www.linkedin.com/in/santiago-salazar-pavajeau/)
[Twitter](https://twitter.com/santispavajeau)


