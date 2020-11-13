---
layout: post
title:      "Managing Big O Notation"
date:       2020-11-13 16:21:27 +0000
permalink:  managing_big_o_notation
---


In this blog I go over some concepts of Big O Notation that I've been breaking through after some months of practicing algorithms and might be helpful to others in the same process of improving their technical interview skills.

## Time complexity 

Tracks how long an algorithm takes to run (processing). We focus on time complexity when we really want to improve the performance of algorithms. Loops, recursion, and methods of iteration will usually increase the time complexity of algorithms and slow down our programs. Processing power is an expensive resource, and everyone needs websites to load fast so time complexity is has the higher priority when dealing with Big O.

## Space complexity

It tracks the memory taken by the assignment of variables (RAM) and data types like integers, strings, arrays etc. Integers take a constant amount of memory `O(1)`, but strings and arrays take more memory as they increase in size (n) `O(n)`. However, space complexity is not a priority in the improvement of Big O notation in algorithms as RAM resources run out less frequently.

## Nested Loops

Dealing with nested loops is a bit of a contradiction because most algorithms have a 'brute force' or 'intuitive solution' that uses nested loops. However every time we nest a loop the time complexity increases exponentially.

For example:

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
    
    console.log([...new Set(result)]) // print only unique for readability
}

```
Source: [Aaron Martin (AJMANNTECH)](https://youtu.be/2tW3XDVqvqA)

In this example, we use a nested loop to evaluate every item (outer for loop) against the rest of the items (inner for loop) and start to count how many duplicates we have on the array.

```javascript
const duplicateNumbers = [1,2,3,2,1,2]
countDuplicatesSlow(duplicateNumbers)
// returns => [Found a total of: (2) number 1s,
//             Found a total of: (3) number 2s,
//             Found a total of: (1) number 3s]
```

This is a great example of an opportunity to improve Big O with a 'memoization' object. With this technique we can go from `O(n^2)` to `O(n)` which is a great improvement. I will be focusing on this in an upcoming blog.

## Recursion

With recursion, the algorithms get very slow when we have to perform binary tree searches. Usually, if we search a binary tree 
on all nodes the time complexity will be `O(2^n)` where n is the depth of the tree. 

If we look at a recursion example like this adaptation from [climbing steps on leetcode](https://leetcode.com/explore/interview/card/top-interview-questions-easy/97/dynamic-programming/569/), which asks to find how many unique ways there are to go up a set of steps, when we can take either one or two steps on each opportunity to go up. The resulting time complexity is a `O(2^n)` which is even [slower](https://www.bigocheatsheet.com/) than an `O(n^2)` nested loop.

[Code in sandbox](https://codesandbox.io/s/slow-recursion-cy4ez?file=/src/index.js)
```javascript
const recursionTreeSlow = (maxLevel) => {
    return recursion_Tree_Slow(0, maxLevel)
}

const recursion_Tree_Slow = (currentLevel, maxLevel) => {
    if(currentLevel > maxLevel){
        return 0
    }
    if(currentLevel === maxLevel){
        return 1
    }
    return recursion_Tree_Slow(currentLevel+1, maxLevel) + recursion_Tree_Slow(currentLevel+2, maxLevel)
}
```
In this slower recursion example the program unnecessarily builds the data multiple times on nodes that are the same. So the program is rebuilding data that has already been created but had not been stored, thus wasting resources. 

The 'memoization' technique can also be used in binary tree recursion but understanding the implementation might need a bit more visualization because binary trees can be a bit more abstract than arrays and objects. I will also give it a go at explaining this in an upcoming blog.

Feel more than welcome to reach out and also to help with any comments/ideas.


[LinkedIn](https://www.linkedin.com/in/santiago-salazar-pavajeau/)
[Twitter](https://twitter.com/santispavajeau)



Resources:

[Big O Cheatsheet](https://www.bigocheatsheet.com/)
[Big O tips](https://web.stanford.edu/class/archive/cs/cs106b/cs106b.1176/handouts/midterm/5-BigO.pdf)
[Learn.co on time-complexity](https://learn.co/tracks/computer-science/why-algorithms/big-o/time-complexity)
[AJMANNTECH](https://youtu.be/2tW3XDVqvqA)
[KodingKevin on Space Complexity](https://www.youtube.com/watch?v=_F29n4Z69rE)


