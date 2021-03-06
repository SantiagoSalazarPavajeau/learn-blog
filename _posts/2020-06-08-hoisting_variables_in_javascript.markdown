---
layout: post
title:      "Dealing with var in Javascript"
date:       2020-06-08 17:17:53 -0400
permalink:  hoisting_variables_in_javascript
---

### Lexical Environment and Scope

The lexical environment is where Javascript stores the meta data about variables and functions. So the meta-data of the lexical environment tells Javascript the scope of a variable whether is global or local.
 
### Hoisting

Hoisting literally refers to the concept of raising or lifting something onto the top. In Javascript it has to do with the way variables and functions exist at a certain moment in the "running" of the code and how they are "raised" to the top of the code by the Javascript engine. However, this JavaScript behavior is quite unique and has some specific rules that need to be accounted for in order to understand how to correctly declare variables in order to have access to them and their values.

More specifically, hoisting refers to what variables look like in the lexical environment at compile phase vs execution phase. The compile phase is when the code that we write is prepared to be read by the machine and the execution phase when the machine reads it. So when Javascript prepares our code to be read by the machine it looks up for the variables and declares them in the lexical environment by "hoisting them".

```
 var hoisted; // variable declaration
```

In fact, during compile, Javascript hoists the variable declaration, but not the initialization. So the variable won't equal the below string in compile phase.

```
 var hoisted = "but not initialized to our value" // variable initialization
```

The **var** variable will be initialized to *undefined* during compile.


So Javascrpt in compile hoists variable declarations and does this differently for **var**, **let** and **const**. That is, **let** and **const** are hoisted but they are not declared into anything and are innaccesible during compile phase. So while a variable declared with **var** will be initialized to *undefined* a variable declared with **let** and **const**, will not be initialized and so it will be inaccesible during compile.

Then during the execution phase of the code all variables will be set to the initialization value we assigned them, so whatever string, integer, etc. we want them to be set to.

```
 var afterHoisted =  "now initialized" // in execution, after it was hoisted by compile and initialized to undefined
 let letAfterHoisted =  "now initialized" // in execution, after it was hoisted by compile and initialized to nothing
 const constAfterHoisted =  "now initialized"// in execution, after it was hoisted by compile and initialized to nothing

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

Both will return a Reference Error because the declaration of the variable **a** is hoisted but its value is inaccesible since they are not being initialized with *undefined* like **var** does. So initializing these variables to "hello" after the console.log call will indeed create a Reference Error.

### Block Context

When we look at declaring variables with **var** we also find that they do not have block scope with if statements. In the code below we try to change the value of the variables `a`, `b`, and `c`, and end up only being able to change `a`. So we can reassign a `var` variable and overwrite

```
function variables(){
  var a = 1
  let b = 2
  const c = 3
  
  if (true) {
	 var a = 4
   var d = 5
   let e = 6
   const f = 7
  }
  return `${a} ${b} ${c} ${d} ${e} ${f}` // Reference Error: e is not defined
} // "4 2 3 5" ---- **a** is changed by the block in the if statement 
```


Normal block scope behavior would be, that the variables inside the if statement block could not be accessed by the outer scope. This happens as expected when we use **let** and **const**. However, **var d**  is accesible by the outside of the if statement. Also the first instance of `var a` is overwritten by the instance in the inner scope. So the instances of `var a` and `var d`  inside the block gets hoisted to the function scope. The block lexical environment of if statements is not respected by `var`.


However, a variable declared and initiated with **var** can be set to a function scope and not have the same behavior as with a block.

```
var a = 1

function variables( ){
 var a = 4
}

console.log(a) // returns 1
```

So there is a way to limit the hoisting of **var** variables and that is by using function scope or "closures".

### Loops and var as an example

Lets look at a refactored example from the [Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures):

[LINK OF EXAMPLE IN SANDBOX](https://codesandbox.io/s/focused-feistel-9kqwv?file=/src/index.js)

```
/index.html

<p id="number">click on a number</p>
<p id="1">10</p>
<p id="2">14</p>
<p id="3">7</p>
```

```
/index.js

function showNumber(number, i) {
  document.getElementById('number').innerText = `You clicked but on all the numbers but only got ${number} and the loop var i is stuck with ${i}.`;
}

function setupNumber() {
  var numberText = [
      {'id': '1', 'number': '10'},
      {'id': '2', 'number': '14'},
      {'id': '3', 'number': '7'}
    ];

  for (var i = 0; i < numberText.length; i++) {
    var data = numberText[i];
    document.getElementById(data.id).onclick = function() {
      showNumber(data.number, i);
    }
  }
}

setupNumber();

// no matter where you click the numbers are the same => You clicked but on all the numbers but only got 7 and the loop var i is stuck with 3.

```

This loop wil only create one lexical environment for the three iterations so the **var data**  and the **var i** are  being overwritten each iteration, in other words, the **var data** and **var i** are being hoisted outside the block. To be exact, both variables are being hoisted into the **setupNumber()** function scope and ignoring the block scope. By the time we click on the numbers on the html, the loop has already finished and the variables (that has already been overwritten three times)  are set to the last iteration that was hoisted which (7 and 3).

To have 3 different values for each loop there needs to be 3 different lexical scopes. So new scopes have to be created with an inner function or closure in each iteration.

```

function showNumber(number, i) {
  document.getElementById('number').innerText = `You clicked ${number} and got ${number} and the loop var i is ${i} which corresponds!`;
}

function cointainingNewClosure(number, i){
    return function(){
        showNumber(number, i)
    }
}

function setupNumber() {
  var numberText = [
      {'id': '1', 'number': '10'},
      {'id': '2', 'number': '14'},
      {'id': '3', 'number': '7'}
    ];

  for (var i = 0; i < numberText.length; i++) {
    var data = numberText[i];
    document.getElementById(data.id).onclick = cointainingNewClosure(data.number, i)
  }
}

setupNumber(); // we get the desired output

```

Here we created a scope for each iteration using a closure. 

Fortunately, presently we have the **let** keyword to declare variables, so to debug a **var** variable that is skipping the block scope and getting hoisted into the outer scope, we can just use **let**. The **let** variable will have the block local scope. The following loop will not overwrite the **let i** and **let data** because the **let** variables have block scope.


```

for (let i = 0; i < helpText.length; i++) {
    let data = numberText[i];
    document.getElementById(item.id).onclick = function() {
      showNumber(data.number, i);
    }
  }
	
```


So in conclusion, when we are trying to debug code that uses **var** to declare and initialize variables, we need to take into account that the behavior of hoisting, scoping, etc. of **var** variables will not be the same as with **let** and **const**. And this is a powerful tool to have as a new developer working with legacy code.


Resources:

https://developer.mozilla.org/en-US/docs/Glossary/Hoisting

https://blog.bitsrc.io/hoisting-in-modern-javascript-let-const-and-var-b290405adfda

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var

https://medium.com/javascript-in-plain-english/how-to-use-let-var-and-const-in-javascript-cdf42b48d70

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures

https://www.freecodecamp.org/forum/t/how-does-js-compile-for-loops/151885


