---
layout: post
title:      "Improving Two Sum and Duplicate Integers with memoization"
date:       2020-12-02 20:53:42 +0000
permalink:  improving_two_sum_and_duplicate_integers_with_memoization
---


In this blog, I follow up on my earlier post: [Managing Big O Notation](https://dev.to/santispavajeau/managing-big-o-notation-3162) and give it a shot at explaining a technique to improve some algorithms.

I will be looking specifically at eliminating nested loops through memoization so these examples go from `O(n^2)` to `O(n)`. In an upcoming blog, I will take a look at improving some recursion solutions.

## Memoization

This technique involves using an `Object` in javascript or any other data structure with key-value pairs (in other languages) to temporarily store some data while the algorithm is being executed. A key-value pair data structure is used because keys are unique so the same key won't be generated more than once. So if certain data has to be accessed multiple times, it can be stored in only one run in the form of key value pairs and then it can be accessed multiple times without the need of regenerating it. When this technique is not used, identical data is created over and over again which makes the algorithm slower.

This approach also allows to add some logic that helps get the solution at the same time we access the data of the object; as we will see in the following example.

## Two Sum 
[Code in Sandbox](https://codesandbox.io/s/twosum-7vkp0?file=/src/index.js)

A basic example of using a memoization object (in javascript) is Two Sum which is Leetcode problem #1. Two Sum takes an array of integers and a target sum and asks to find any two numbers from the array that add to the target, but we return their indexes. The brute force solution is:

``` javascript
const twoSumSlow = (numbers, sum) => {// O(n^2) big o complexity
    
    for(let i = 0; i<numbers.length; i++){
        
        for(let j = i+1; j<numbers.length; j++){// nested loop j = i+1 to avoid adding same element
            
            if(numbers[i] + numbers[j] === sum){
                
                return [i, j]; // return index of elements that sum to target
            }
        }
    }
};

const numbers = [1,2,7,8,9]
const sum = 10
twoSumSlow(numbers, sum)
// returns => [0,4] which are the indexes of the correct numbers
// because 1 + 9  = 10
```

This solution uses a nested loop (numbers[i] vs numbers[j]) to check every combination of numbers in the array to see if they add to the required sum. 

However, what makes this solution slow is that every number is visited more than once by the nested loop so when the size of the array increases, the amount of visits by the parent and child loop to each number, grows exponentially, which makes the solution expensive.

Taking a look at the memoization object solution:

``` javascript
const twoSumFast = (numbers, sum) => {// O(n) big O time complexity

    const dataObject = {}
    for(let i =0; i< numbers.length; i++){
        dataObject[numbers[i]] = i // create memo object
    }

    for(let i =0; i< numbers.length; i++){
        const missingNumber = sum - numbers[i] 

        if(dataObject[missingNumber] && dataObject[missingNumber] !== i){ 

            return [dataObject[missingNumber], i] // return missing number's index and current index

        }

    }
}

const numbers = [1,2,7,8,9]
const sum = 10
twoSumFast(numbers, sum)
// returns => [0,4] which are the indexes of the correct numbers
// because 1 + 9  = 10
```

We implement memoization by creating a `dataObject` with the array of numbers as keys of the object and the index of each number in the array as the corresponding value.

```javascript
dataobject = {
 1: 0,
 2: 1,
 7: 2,
 8: 3,
 9: 4
}
```
 This way, we can add a second loop (which is not nested) that checks for the `missingNumber` that adds out to our desired value. 

Generating the 'memoization object' `dataObject` allows us to store all the numbers as unique keys that can be accessed as `dataObject[missingNumber]` to retrieve the index of the missing number for the 'two sum'.

The added/unique logic in this example comes from using an indirect way of checking for the sum through the missing number, which is found by subtracting the current number from the sum.

``` javascript
const missingNumber = sum - numbers[i]
```

Then we can add this logic when accessing the object key with `dataObject[missingNumber]`. And so we kill two birds with one store by generating the `missingNumber` and also seeing if it exists as a key of the object.

```javascript
if(dataObject[missingNumber] && dataObject[missingNumber] !== i){ 

  return [dataObject[missingNumber], i] 

}
```



In the nested loop example, we set the sum logic equality in the nested loop which increases the time complexity.

```javascript
//nested loop w/ i and j
if(numbers[i] + numbers[j] === sum){
                
 return [i, j]; 

}

```

## Counting Duplicates

This next example is an adaptation from [Aaron Martin (AJMANNTECH)](https://youtu.be/2tW3XDVqvqA) video on youtube. This algorithm takes a list of numbers and counts the duplicates. 

[Code in sandbox](https://codesandbox.io/s/slowduplicates-k0mpx?file=/src/index.js)

```javascript
const countDuplicatesSlow = (numbers) => { // O(n^2) big o complexity

    let result = []

    for(let i = 0; i<numbers.length;  i++){ 

        let count = 0

        for(let j = 0; j<numbers.length;  j++){

            if(numbers[i] === numbers[j]){ // if we find a duplicate as we compare all numbers to all numbers

                count++

            }
        }
        result.push(`Found a total of: (${count}) number ${numbers[i]}s`)
    }
    
    return [...new Set(result)]) // only unique
}

```

In this example, we use a nested loop to evaluate every item (outer for loop) against the rest of the items (inner for loop) and start to count how many duplicates we have on the array.

```javascript
const duplicateNumbers = [1,2,3,2,1,2]
countDuplicatesSlow(duplicateNumbers)
// returns => [Found a total of: (2) number 1s,
//             Found a total of: (3) number 2s,
//             Found a total of: (1) number 3s]
```

So first we create a loop to save the unique elements as keys to the object with an empty array as value and then we do a second loop to count the duplicates to the corresponding keys.

[Code in sandbox](https://codesandbox.io/s/fastduplicates-yd462?file=/src/index.js)

```javascript
const countDuplicates = (numbers) => { // O(n) big o complexity

    let result = {}

    for(let i = 0; i<numbers.length;  i++){

        if(!result[numbers[i]]){ // if key does not exist the value has not been accounted for

            let count = 1;

            result[numbers[i]] = numbers[i] //initialize key

            result[numbers[i]] = count // initialize value

        } else {

            result[numbers[i]]++ //increase count if key already exists
            
        }
    }
    return result
}
```

Not having a nested loop allows for the algorithm to be O(n) instead of O(n^2). 

