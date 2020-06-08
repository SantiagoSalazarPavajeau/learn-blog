---
layout: post
title:      "Hoisting variables in Javascript"
date:       2020-06-08 21:17:52 +0000
permalink:  hoisting_variables_in_javascript
---


Hoisting literally refers to the concept of raising or lifting something onto the top. In Javascript it has to do with the way variables and functions exist at a certain moment in the "running" of the code and how they are "raised" to the top of the code by the Javascript engine. However, this JavaScript behavior is quite unique and has some specific rules that need to be accounted for in order to understand how to correctly declare variables.

More specifically hoisting refers to what variables look like at compile phase vs execution phase. The compile phase is when the code that we write is prepared to be read by the machine and the execution phase when the machine reads it. So when Javascript prepares our code to be read by the machine it looks for the variables and declares them:

```
 var hoisted;
```


In fact hoisting the variable declaration, but not the initialization. So the variable won't equal the below string in compile phase.

```
 hoisted = "but not initialized"
```

The variable will be set to *undefined* during this time.


So Javascrpt hoists variable declarations and does this differently for **var**, **let** and **const**. That is, let and const are hoisted but they are not declared into anything and are innaccesible during compile phase. So while a variable declared with **var** will be initialized to *undefined* a variable declared with **let** and **const**, will not be initialized and so it will be inaccesible during compile.

Then during the execution phase of the code all variables will be set to their respective initialization value.

```
 hoisted =  "now initialized"
```

In a more concrete example if we write:

```
console.log(a)
var a = "hello"
```

Running this code will return undefined as the variable declaration of *a* will be hoisted but not the initialization, so it will just be set to *undefined* since we declared it with **var**.

On the other hand looking at the behavior of the following code:

```
console.log(a)
let a = "hello"

or

console.log(a)
const a = "hello"
```

Both will return a Reference Error because the declaration of the variable **a** is hoisted but its value is inaccesible since they are not being initialized with *undefined* like **var** does. So initializing these variables to "hello" after the console.log will indeed create an error.

Resources:

https://developer.mozilla.org/en-US/docs/Glossary/Hoisting

https://blog.bitsrc.io/hoisting-in-modern-javascript-let-const-and-var-b290405adfda


